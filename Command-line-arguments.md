Best way to see how the command line arguments work is through looking at `./matlab_runners/Demos/` which illustrates how each of the models is called

## FaceLandmarkVid and FaceLandmarkVidMulti

Parameters for input (if nothing is specified attempts to read from a webcam with default values)

   `-f <filename>` the video file being input, can specify multiple files

   `-device <device_num>` the webcam from which to read images (default 0)

   `-fd <depth directory>` the directory where depth files are stored (deprecated)

   `-root <directory>` the root of input and output so `-f` and `-ov` can be specified relative to it

Parameters for output:

   `-ov <location of visualized track>` where to output video file with tracked landmarks

## FaceLandmarkImg

Single image analysis:

   `-f <filename>` the image file being input, can have multiple `-f` flags

   `-of <filename>` location of output landmark points file

   `-oi <filename>` location of output image with landmarks

   `-root <dir>` the root directory so `-f`, `-of` and `-oi` can be specified relative to it

Batch image analysis:
	
    -fdir <directory> - runs landmark detection on all images (.jpg and .png) in a directory, if the directory contains .txt files (image_name.txt) with bounding boxes, it will use those for initialisation
    -ofdir <directory> - where detected landmarks should be written
    -oidir <directory> - where images with detected landmarks should be stored

Model parameters (apply to images and videos):

    -wild flag specifies when the images are more difficult, model considers extended search regions
    -multi-view <0/1>, should multi-view initialisation be used (more robust, but slower)

Output format - The file format is same as 300 faces in the wild challenge annotations (http://ibug.doc.ic.ac.uk/resources/facial-point-annotations/)

## FeatureExtraction

    -f <filename> - the video file being input, can specify multiple ones
    -fdir <directory>
        run the feature extraction on every image (.jpg and .png) in a directory (the output will be stored in individual files for the whole directory)
    -asvid 
        if this flag is specified the images in -fdir directory will be treated as if they came from a video, that is they form a sequence, so tracking will be done instead of detection per videos)
    -root <the root of input and output directories>
	
Parameters for output

	-outroot <the root directory relevant to which the output files are created> (optional)
	
	-op <location of output pose file>, the file format is as follows: frame_number, timestamp(seconds), confidence, detection_success, X, Y, Z, Rx, Ry, Rz
	-ogaze <location of output file>, the file format is as follows: frame_number, timestamp(seconds), confidence, detection_success, x_0, y_0, z_0, x_1, y_1, z_1
		The gaze is output as 2 vectors, the vectors are in world coordinate space describing the gaze direction of both eyes as normalized gaze vectors
	-of <location of output landmark points file>, the file format is as follows: frame_number, timestamp(seconds), confidence, detection_success, x_1 x_2 ... x_n y_1 y_2 ... y_n
	-of3D <location of output 3D landmark points file>, the file format is as follows: frame_number, timestamp(seconds), confidence, detection_success, X_1 X_2 ... X_n Y_1 Y_2 ... Y_n Z_1 Z_2 ... Z_n
	-ov <location of tracked video>

	-oparams <output geom params file>, the file format is as follows: frame_number, timestamp(seconds), confidence, detection_success, scale, rx, ry, rz, tx, ty, p0, p1, p2, p3, p4, p5, p6, p7, p8 ... (rigid and non rigid shape parameters)
	-oaus <output AU file>, the file format is as follows: frame_number, timestamp(seconds), confidence, detection_success, AU01_r, AU02_r, AU04_r, ... (_r implies regression _c classification)
	-hogalign <output HOG feature location>, outputs HOG in a binary file format (see ./matlab_runners/Demos/Read_HOG_files.m for a script to read it in Matlab)
	-simalign <output directory for aligned face image>, outputs a similarity aligned and cropped face for further analysis

	-world_coord <1/0>, should rotation be measured with respect to the camera or world coordinates, see Head pose section for more details>

	Additional parameters for output
	
	-verbose visualise the HOG features if they are being output
	-rigid use a slightly more robust version of similarity alignment. This uses and experimentally determined set of more stable feature points instead of all of them for determining similarity alignment
	-simscale <default 0.7> scale of the face for similarity alignment
	-simsize <default 112> width and height of image in pixels when similarity aligned
	-g output images should be grayscale (for saving space)
## Common parameters for all

    -q specifying to use quiet mode not visualizing output

Model to use parameters:

    -mloc <the location of landmark detection models>

  Options for this:
        
- `"model/main_clnf_general.txt"` (default) - trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clnf_wild.txt"` - trained on In-the-wild data, works better in noisy environments (not very well suited for head pose tracking)
- `"model/main_clm_general.txt"` - a less accurate but slightly faster CLM model trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clm-z.txt"` - trained on Multi-PIE and BU-4DFE datasets, works with both intensity and depth signals (CLM-Z)

Optional camera parameters for proper head pose visualization:

	-fx <focal length in x>
	-fy <focal length in y>
	-cx <optical centre in x> 
	-cy <optical centre in y>