### Landmark detection and tracking

`LandmarkDetector::CLNF` class is the main class you will interact with, it performs the main landmark detection algorithms and stores the results. The interaction with the class is declared mainly in the LandmarkDetectorFunc.h, and will require an initialised LandmarkDetector::CLNF object. 

The best way to understand how landmark detection is performed in videos or images is to just compile and run FaceLandmarkVid and FaceLandmarkImg projects, which perform landmark detection in videos/webcam and images respectively. See later in the readme for the command line arguments for these projects.

A minimal code example for landmark detection is as follows:

LandmarkDetector::FaceModelParameters det_parameters
LandmarkDetector::CLNF clnf_model(det_parameters.model_location);

LandmarkDetector::DetectLandmarksInImage(grayscale_image, clnf_model, det_parameters);

A minimal code example for landmark tracking is as follows:

LandmarkDetector::FaceModelParameters det_parameters
LandmarkDetector::CLNF clnf_model(det_parameters.model_location);	

while(video)
{
	 LandmarkDetector::DetectLandmarksInVideo(grayscale_image, clnf_model, det_parameters);
}

After landmark detection is done clnf_model stores the landmark locations and local and global Point Distribution Model parameters inferred from the image. To access them and more use:

2D landmark location (in image):
	clnf_model.detected_landmarks contains a double matrix in following format [x1;x2;...xn;y1;y2...yn] describing the detected landmark locations in the image
	
3D landmark location (with respect to camera):
	clnf_model.GetShape(fx, fy, cx, cy);
	// fx,fy,cx,cy are camera callibration parameters needed to infer the 3D position of the head with respect to camera, a good assumption for webcams is 500, 500, img_width/2, img_height/2	
	// This returns a column matrix with the following format [X1;X2;...Xn;Y1;Y2;...Yn;Z1;Z2;...Zn], here every element is in millimeters and represents the facial landmark locations with respect to camera
	
3D landmark location in object space:
	clnf_model.pdm.CalcShape3D(landmarks_3D, clnf_model.params_local);

### Head pose tracking

	// Head pose is stored in the following format (X, Y, Z, rot_x, roty_y, rot_z)
	// translation is in millimeters with respect to camera centre
	// Rotation is in radians around X,Y,Z axes with the convention R = Rx * Ry * Rz, left-handed positive sign
	// The rotation can be either in world or camera coordinates (for visualisation we want rotation with respect to world coordinates)

	There are four methods in total that can return the head pose
	
	//Getting the head pose w.r.t. camera assuming orthographic projection
	Vec6d GetPoseCamera(CLM& clnf_model, double fx, double fy, double cx, double cy, CLMParameters& params);
	
	//Getting the head pose w.r.t. world coordinates assuming orthographic projection
	Vec6d GetPoseWorld(CLM& clnf_model, double fx, double fy, double cx, double cy, CLMParameters& params);
	
	//Getting the head pose w.r.t. camera with a perspective camera correction
	Vec6d GetCorrectedPoseCamera(CLM& clnf_model, double fx, double fy, double cx, double cy, CLMParameters& params);

	//Getting the head pose w.r.t. world coordinates with a perspective camera correction
	Vec6d GetCorrectedPoseWorld(CLM& clnf_model, double fx, double fy, double cx, double cy, CLMParameters& params);

	// fx,fy,cx,cy are camera callibration parameters needed to infer the 3D position of the head with respect to camera, a good assumption for webcams providing 640x480 images is 500, 500, img_width/2, img_height/2	

### Gaze

### Action Units

### Appearance features
