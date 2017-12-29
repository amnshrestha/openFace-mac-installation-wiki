The best way to familiarize yourself with the API and system capabilities is by exploring the executables provided with the library, specifically files - `FaceLandmarkImg.cpp,FaceLandmarkVid.cpp,FeatureExtraction.cpp,FaceLandmarkVidMulti.cpp`

In the following, we document the most important/interesting API calls

### Landmark detection and tracking

`LandmarkDetector::CLNF` class is the main class you will interact with, it performs the main landmark detection algorithms and stores the results. The interaction with the class is declared mainly in the `LandmarkDetectorFunc.h`, and will require an initialized `LandmarkDetector::CLNF` object. 

The best way to understand how landmark detection is performed in videos or images is to just compile and run `FaceLandmarkVid` and `FaceLandmarkImg` projects, which perform landmark detection in videos/webcam/image sequences and individual images respectively. See [command line arguments] (https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments) for these projects for more details.

#### Landmark detection in images

A minimal code example for landmark detection in an individual image is as follows:

    LandmarkDetector::FaceModelParameters det_parameters;
    LandmarkDetector::CLNF clnf_model(det_parameters.model_location);
    
    LandmarkDetector::DetectLandmarksInImage(grayscale_image, clnf_model, det_parameters);

You can also provide your own face detection to detect landmarks of a particular face in the image (example using a modified dlib face detector):

    LandmarkDetector::FaceModelParameters det_parameters;
    LandmarkDetector::CLNF clnf_model(det_parameters.model_location);
	dlib::frontal_face_detector face_detector_hog = dlib::get_frontal_face_detector();

    double confidence;
    cv::Rect_<double> bounding_box;
    bool face_detection_success = LandmarkDetector::DetectSingleFaceHOG(bounding_box, grayscale_image, clnf_model.face_detector_HOG, confidence);

    LandmarkDetector::DetectLandmarksInImage(grayscale_image, bounding_box, clnf_model, det_parameters);

Note that the bounding box has to be around the 68 landmarks being detected, so just calling an external face detector without adapting the bounding box might lead to suboptimal results.
	
#### Landmark tracking in videos/webcam/image sequences

A minimal pseudo code example for landmark tracking is as follows:

    LandmarkDetector::FaceModelParameters det_parameters;
    LandmarkDetector::CLNF clnf_model(det_parameters.model_location);	

    while(video)
    {
        LandmarkDetector::DetectLandmarksInVideo(grayscale_image, clnf_model, det_parameters);
    }

OpenFace provides a utility function that allows to read videos/image sequences/webcam in a single interface. Have a look at `Utilities::SequenceCapture`
	
### Landmark results	
	
After landmark detection is done `clnf_model` stores the landmark locations and local and global Point Distribution Model parameters inferred from the image. To access them and more use:

- 2D landmark location (in image):

   `clnf_model.detected_landmarks` contains a double matrix in following format `[x1;x2;...xn;y1;y2...yn]` describing the detected landmark locations in the image
- 3D landmark location in world space:

	`clnf_model.GetShape(fx, fy, cx, cy);` This returns a column matrix with the following format `[X1;X2;...Xn;Y1;Y2;...Yn;Z1;Z2;...Zn]`, here every element is in millimeters and represents the facial landmark locations with respect to camera (you need to know camera focal length and optical centre `fx,fy,cx,cy` for this)
- 3D landmark location in object space:

	`clnf_model.pdm.CalcShape3D(landmarks_3D, clnf_model.params_local);`

### Head pose tracking

Head pose is stored in the following format `(X, Y, Z, rot_x, roty_y, rot_z)`,  translation is in millimeters with respect to camera centre (positize Z away from camera), rotation is in radians around X,Y,Z axes with the convention R = Rx * Ry * Rz, left-handed positive sign. To get the head pose:
    
    cv::Vec6d head_pose = LandmarkDetector::GetPose(face_model, fx, fy, cx, cy);

`fx,fy,cx,cy` are camera callibration parameters needed to infer the 3D position of the head, a good assumption for webcams providing 640x480 images is 500, 500, img_width/2, img_height/2. If you are using `ImageCapture` or `SequenceCapture` utility classes, they will estimate the camera calibration parameters for you.

### Gaze

To extract eye gaze you will need to have facial landmarks detected using a `LandmarkDetector::CLNF` model that contains eye region landmark detectors (used by default). 
A minimal pseudo code example of tracking eye gaze vectors (direction vectors in the direction of eye gaze as measured from the eye location)

    LandmarkDetector::FaceModelParameters det_parameters;
    LandmarkDetector::CLNF clnf_model(det_parameters.model_location);	

    while(video)
    {
        bool success = LandmarkDetector::DetectLandmarksInVideo(grayscale_image, clnf_model, det_parameters);
				
        cv::Point3f gazeDirection0(0, 0, -1);
        cv::Point3f gazeDirection1(0, 0, -1);
		cv::Vec2d gazeAngle(0, 0);

        if (success && det_parameters.track_gaze)
        {
            GazeAnalysis::EstimateGaze(clnf_model, gazeDirection0, fx, fy, cx, cy, true);
            GazeAnalysis::EstimateGaze(clnf_model, gazeDirection1, fx, fy, cx, cy, false);
			gazeAngle = GazeAnalysis::GetGazeAngle(gazeDirection0, gazeDirection1, pose_estimate);
        }
    }

`fx,fy,cx,cy` are camera callibration parameters needed to infer the 3D position of the head, a good assumption for webcams providing 640x480 images is 500, 500, img_width/2, img_height/2. If you are using `ImageCapture` or `SequenceCapture` utility classes, they will estimate the camera calibration parameters for you.
	
### Action Units

Facial Action Units can be extracted in each image in a static manner or extracted from a video in a dynamic manner. The dynamic model is more accurate if enough video data is available for a person (roughly more than 300 frames that contain a number of non-expressive/neutral frames). 

To extract AUs from an image:

    // Load landmark detector
    LandmarkDetector::FaceModelParameters det_parameters(arguments);
    LandmarkDetector::CLNF face_model(det_parameters.model_location);
    
    // Load facial feature extractor and AU analyser
    FaceAnalysis::FaceAnalyserParameters face_analysis_params(arguments);
	face_analysis_params.OptimizeForImages();	
    FaceAnalysis::FaceAnalyser face_analyser(face_analysis_params);

    bool success = LandmarkDetector::DetectLandmarksInImage(grayscale_image, clnf_model, det_parameters);
    face_analyser.PredictStaticAUsAndComputeFeatures(captured_image, clnf_model.detected_landmarks);

	auto aus_intensity = face_analyser.GetCurrentAUsReg();
	auto aus_presence = face_analyser.GetCurrentAUsClass();
	
`aus_intensity` and `aus_presence` will contain a `std::vector<std::pair<string, double>>`, each vector contains the AU name and presence or intensity value.

To extract AUs in a video look at `FeatureExtraction.cpp` for a detailed example that performs offline AU normalization, making it more accurate. This requires writing the AU predictions to file or memory so that they could later be corrected. 