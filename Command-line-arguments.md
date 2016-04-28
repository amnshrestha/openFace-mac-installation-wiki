Best way to see how the command line arguments work is through looking at `./matlab_runners/Demos/` which illustrates how each of the models is called

## FaceLandmarkVid and FaceLandmarkVidMulti

**Parameters for input**

   `-f <filename>` the video file being input, can specify multiple files

   `-device <device_num>` the webcam from which to read images (default 0)

   `-fd <depth directory>` the directory where depth files are stored (deprecated)

   `-root <directory>` the root of input and output so `-f` and `-ov` can be specified relative to it

**Parameters for output**

   `-ov <location of visualized track>` where to output video file with tracked landmarks

## FaceLandmarkImg

**Single image analysis**

   `-f <filename>` the image file being input, can have multiple `-f` flags

   `-of <filename>` location of output landmark points file

   `-oi <filename>` location of output image with landmarks

   `-root <dir>` the root directory so `-f`, `-of` and `-oi` can be specified relative to it

**Batch image analysis**
	
   `-fdir <directory>` - runs landmark detection on all images (.jpg and .png) in a directory, if the directory contains .txt files (image_name.txt) with bounding box (`min_x min_y max_x max_y`), it will use those for initialisation

   `-ofdir <directory>` directory where detected landmarks should be written

   `-oidir <directory>` directory where images with detected landmarks should be stored

Output format - The file format is same as 300 faces in the wild challenge annotations (http://ibug.doc.ic.ac.uk/resources/facial-point-annotations/)

## FeatureExtraction

**Input parameters**

   `-f <filename>` the video file being input, can specify multiple `-f` 

   `-fdir <directory>` run the feature extraction on every image (.jpg and .png) in a directory (the output will be stored in individual files for the whole directory)

   `-asvid` if this flag is specified the images in -fdir directory will be treated as if they came from a video, that is they form a sequence, so tracking will be done instead of detection per videos)

   `-root <dir>` the root for input
	
**Parameters for output**

   `-outroot <dir>` the root directory relevant to which the output files are created
	
   `-of <filename>` location of file

   `-ov <filename>` location location of tracked output video

   `-hogalign <filename>` output HOG feature location, outputs HOG in a binary file format (see ./matlab_runners/Demos/Read_HOG_files.m for a script to read it in Matlab)

   `-simalign <output directory>` output directory for aligned face images, outputs a similarity aligned and cropped face for further analysis

   `-world_coord <1/0>` should rotation be measured with respect to the camera or world coordinates

**Additional parameters for output**
	
   `-verbose` visualise the HOG features if they are being output

   `-rigid` use a slightly more robust version of similarity alignment. This uses and experimentally determined set of more stable feature points instead of all of them for determining similarity alignment, on by default

   `-simscale <float>` scale of the face for similarity alignment (default 0.7), different values won't work with AU extraction

   `-simsize <int>` width and height of image in pixels when similarity aligned (default 112), different values won't work with AU extraction

   `-g` output images should be grayscale (for saving space)

You might not always want to extract all the output features (gaze, Action Units, landmarks, pose, etc.), you can restrict output using the following flags:

   `-no2Dfp` do not output 2D landmarks in pixels

   `-no3Dfp` do not output 3D landmarks in milimeters

   `-noMparams` do not output rigid and non-rigid shape parameters

   `-noPose` do not output head pose (location and rotation)

   `-noAUs` do not output the Facial Action Units

   `-noGaze` do not output eye gaze

## Common parameters for all

    -q specifying to use quiet mode not visualizing output

**Model to use parameters**

    -mloc <the location of landmark detection models>

  Options for this:
        
- `"model/main_clnf_general.txt"` (default) - trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking (CLNF model)
- `"model/main_clnf_wild.txt"` - trained on In-the-wild data, works better in noisy environments (not very well suited for head pose tracking),  (CLNF in-the-wild model)
- `"model/main_clm_general.txt"` - a less accurate but slightly faster CLM model trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clm-z.txt"` - trained on Multi-PIE and BU-4DFE datasets, works with both intensity and depth signals (CLM-Z)

**Model parameters**

   `-wild` flag specifies when the images are more difficult, model considers extended search regions

   `-multi-view <0/1>` should multi-view initialisation be used (more robust, but slower), off by default

**Optional camera parameters for proper head pose and eye gaze computation**

	-fx <focal length in x>
	-fy <focal length in y>
	-cx <optical centre in x> 
	-cy <optical centre in y>