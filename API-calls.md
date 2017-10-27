The best way to familiarize yourself with the API and system capabilities is by exploring the executables provided with the library, specifically files - `FaceLandmarkImg.cpp,FaceLandmarkVid.cpp,FeatureExtraction.cpp,FaceLandmarkVidMulti.cpp`

In the following, we document the most important/interesting API calls

### Landmark detection and tracking

`LandmarkDetector::CLNF` class is the main class you will interact with, it performs the main landmark detection algorithms and stores the results. The interaction with the class is declared mainly in the `LandmarkDetectorFunc.h`, and will require an initialized `LandmarkDetector::CLNF` object. 

The best way to understand how landmark detection is performed in videos or images is to just compile and run `FaceLandmarkVid` and `FaceLandmarkImg` projects, which perform landmark detection in videos/webcam and images respectively. See [command line arguments] (https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments) for these projects for more details.

A minimal code example for landmark detection is as follows:

    LandmarkDetector::FaceModelParameters det_parameters;
    LandmarkDetector::CLNF clnf_model(det_parameters.model_location);
    
    LandmarkDetector::DetectLandmarksInImage(grayscale_image, clnf_model, det_parameters);

A minimal pseudo code example for landmark tracking is as follows:

    LandmarkDetector::FaceModelParameters det_parameters;
    LandmarkDetector::CLNF clnf_model(det_parameters.model_location);	

    while(video)
    {
        LandmarkDetector::DetectLandmarksInVideo(grayscale_image, clnf_model, det_parameters);
    }

After landmark detection is done `clnf_model` stores the landmark locations and local and global Point Distribution Model parameters inferred from the image. To access them and more use:

- 2D landmark location (in image):

   `clnf_model.detected_landmarks` contains a double matrix in following format `[x1;x2;...xn;y1;y2...yn]` describing the detected landmark locations in the image
- 3D landmark location in world space:

	`clnf_model.GetShape(fx, fy, cx, cy);` This returns a column matrix with the following format `[X1;X2;...Xn;Y1;Y2;...Yn;Z1;Z2;...Zn]`, here every element is in millimeters and represents the facial landmark locations with respect to camera (you need to know camera focal length and oprical centre `fx,fy,cx,cy` for this)
- 3D landmark location in object space:

	`clnf_model.pdm.CalcShape3D(landmarks_3D, clnf_model.params_local);`

### Head pose tracking

Head pose is stored in the following format `(X, Y, Z, rot_x, roty_y, rot_z)`,  translation is in millimeters with respect to camera centre (positize Z away from camera), rotation is in radians around X,Y,Z axes with the convention R = Rx * Ry * Rz, left-handed positive sign. 

There are two methods that can return the head pose:
   - Getting the head pose w.r.t. world coordinates with a perspective camera correction (default):
      `Vec6d GetPose(const CLNF& clnf_model, float fx, float fy, float cx, float cy);`
   - Getting the head pose w.r.t. camera with a perspective camera correction (	0,0,0 rotation is when a person is looking directly at the camera, useful for modeling attention etc.):
      `Vec6d GetPoseWRTCamera(const CLNF& clnf_model, float fx, float fy, float cx, float cy);`

`fx,fy,cx,cy` are camera callibration parameters needed to infer the 3D position of the head, a good assumption for webcams providing 640x480 images is 500, 500, img_width/2, img_height/2	

### Gaze

To extract eye gaze you will need to have facial landmarks detected using a `LandmarkDetector::CLNF` model that contains eye region landmark detectors (used by default). You also need to make sure that `det_parameters.track_gaze = true`

A minimal pseudo code example of tracking eye gaze vectors (direction vectors in the direction of eye gaze as measured from the eye location)

    LandmarkDetector::FaceModelParameters det_parameters;
    det_parameters.track_gaze = true;
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

### Action Units

Facial Action Units can be extracted in each image in a static manner or extracted from a video in a dynamic manner. The dynamic model is more accurate if enough video data is available for a person (roughly more than 300 frames that contain a number of non-expressive frames). 

To extract AUs from an image:

    // Load landmark detector
    LandmarkDetector::FaceModelParameters det_parameters(arguments);
    LandmarkDetector::CLNF face_model(det_parameters.model_location);
    
    // Load facial feature extractor and AU analyser
    FaceAnalysis::FaceAnalyserParameters face_analysis_params(arguments);
    FaceAnalysis::FaceAnalyser face_analyser(face_analysis_params);

    bool success = LandmarkDetector::DetectLandmarksInImage(grayscale_image, clnf_model, det_parameters);
    auto ActionUnits = face_analyser.PredictStaticAUs(read_image, clnf_model, false)

ActionUnits will contain a `std::pair<std::vector<std::pair<string, double>>, std::vector<std::pair<string, double>>>` a pair of vectors (first for AU intensity and second for AU presence). Each vector contains the AU name and presence or intensity value.

To extract AUs in a video look at `FeatureExtraction.cpp` for a detailed example that performs offline AU normalization.