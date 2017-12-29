# Exaple uses

One way of interacting with OpenFace is through command line arguments (e.g. Command Prompt or PowerShell in Windows and xterm on Unix systems). For this you need to first compile the OpenFace code or download the executables (for Windows).

`FeatureExtraction` executable is used for sequence analysis, while `FaceLandmarkImg` executable is for individual image analysis. Some common example uses are outlined below. For Ubuntu or Mac OS X omit the `.exe`. For the commands to work the current directory has to be in the same directory as the executables, or you need to specify the full path to the executable.

If you want to extract OpenFace features (by features we refer to all the features extracted by OpenFace: facial landmarks, head pose, eye gaze, facial action units, similarity aligned faces, and HOG) from a **video** file in location `C:\my videos\video.avi`, assuming that only one person appears in that video file, execute the following command on the command line:

`FeatureExtraction.exe -f "C:\my videos\video.avi"`

This will create a `processed` directory in the same directory (working directory) as `FeatureExtraction` that will contain the processed features (more [details](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Output-Format)).

If you want to extract OpenFace features from multiple video files, e.g. `C:\my videos\video1.avi`, `C:\my videos\video2.avi`, `C:\my videos\video3.avi`, execute the following command on the command line:

`FeatureExtraction.exe -f "C:\my videos\video1.avi" -f "C:\my videos\video2.avi" -f "C:\my videos\video3.avi"`

If you only want to extract head pose from the video file in `C:\my videos\video1.avi`, execute the following command on the command line:

`FeatureExtraction.exe -f "C:\my videos\video1.avi" -pose`

If instead of a video file you have a **sequence of images** in a directory with each image representing a frame (e.g. 001.jpg, 002.jpg, 003.jpg ... 200.jpg in directory `"C:\my videos\sequence1"`), you can extract features from that sequence:

`FeatureExtraction.exe -fdir "C:\my videos\sequence1"`

If you want to extract OpenFace features from an **image** file in location `C:\my images\img.jpg` (can contain multiple people), execute the following command on the command line:

`FaceLandmarkImg.exe -f "C:\my images\img.jpg"`

This will create a `processed` directory in the same directory (working directory) as `FaceLandmarkImg` that will contain the processed features (more [details](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Output-Format) ).

If you want to extract OpenFace features from multiple video files, e.g. `C:\my images\img1.jpg`, `C:\my images\img2.jpg`, `C:\my images\img3.jpg`, execute the following command on the command line:

`FaceLandmarkImg.exe -f "C:\my images\img1.jpg" -f "C:\my images\img2.jpg" -f "C:\my images\img3.jpg"`

Alternatively, if you want to process all the images in the directory `C:\my images`, you can use:

`FaceLandmarkImg.exe -fdir "C:\my images"`

This will process all of the images (.jpg, jpeg, .bmp, .png files) in the directory.

NOTE, the difference between using `-fdir` flag between `FeatureExtraction` and `FaceLandmarkImg` is that `FeatureExtraction` assumes a sequence of images coming from a video that contains only one face, while `FaceLandmarkImg` assumes a random collection of images with no relationship between them, that can contain multiple faces.

Another good way to see how the command line arguments work is through looking at `./matlab_runners/Demos/` which illustrates how each of the models is called.

# Documentation
   
## FaceLandmarkImg

**Single image analysis**

   `-f <filename>` the image file being input, can have multiple `-f` flags

   `-out_dir <directory>` name of the output directory, where processed features will be places (i.e. CSV file for landmarks, gaze, and aus, HOG feature file, image with detected landmarks and a meta file)
   
   `-root <dir>` the root directory so `-f` can be specified relative to it

   `-inroot <dir>` the input root directory so `-f` can be specified relative to it

**Batch image analysis**
	
   `-fdir <directory>` - runs landmark detection on all images (.jpg, .jpeg, .png, and .bmp) in a directory
   
   `-bboxdir` optional directory that contains .txt files (image_name.txt) with bounding box (`min_x min_y max_x max_y`), it will use those for initialisation instead of an internal face detector

   `-out_dir <directory>` name of the output directory, where processed features will be places (i.e. CSV file for landmarks, gaze, and aus, HOG feature file, image with detected landmarks and a meta file)

For more details on output format see [here](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Output-Format#facelandmarkimg)

## FeatureExtraction

**Input parameters**

   `-f <filename>` the video file being input, can specify multiple `-f` 

   `-fdir <directory>` run the feature extraction on every image (.jpg, .jpeg, .png, and .bmp) in a directory (the output will be stored in individual files for the whole directory)

   `-device <device id>` the device id of a webcam to perform feature extraction from a live feed
   
   `-root <dir>` the root for input

   `-inroot <dir>` the root for input

   `-au_static` if this flag is specified the AU prediction will be performed as if on static images rather than videos, see [here](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Action-Units#static-vs-dynamic) for a more detailed explanation
	
**Parameters for output (for more details on output format see [here](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Output-Format#featureextraction))**

   `-out_dir <dir>` the root directory relevant to which the output files are created
	
   `-of <filename>` the filename of the output (if not specified will use original filename, the name of the directory in case of `-fdir` and webcam + timestamp in case of a webcam)

   `-oc <FOURCC_CODE>` the codec of the output video file (list of FOURCC codes can be found here - https://www.fourcc.org/codecs.php)
 
 
**Additional parameters for output**
	
   `-verbose` visualise the processing steps live: tracked face with gaze, similarity aligned face, and HOG feaures (not visualized by default), this flag turns all of them on, below flags allow for more fine-grained control

   `-vis-track` visualise the tracked face

   `-vis-hog` visualise the HOG features

   `-vis-align` visualise similarity aligned faces

   `-q` supress any live visualisation (this supresses all the other visualization flags)

   `-simscale <float>` scale of the face for similarity alignment (default 0.7)

   `-simsize <int>` width and height of image in pixels when similarity aligned (default 112)

   `-g` output images should be grayscale (for saving space)

By default the executable will output all features (tracked videos, HOG files, similarity aligned images and a .csv file with landmarks, action units and gaze). You might not always want to extract all the output features, you can specify the desired output using the following flags:

   `-2Dfp` output 2D landmarks in pixels

   `-3Dfp` output 3D landmarks in milimeters

   `-pdmparams` output rigid and non-rigid shape parameters

   `-pose` output head pose (location and rotation)

   `-aus` output the Facial Action Units

   `-gaze` output gaze and related features (2D and 3D locations of eye landmarks)

   `-hogalign` output extracted HOG feaure file
   
   `-simalign` output similarity aligned images of the tracked faces

   `-tracked` output video with detected landmarks

## FaceLandmarkVid and FaceLandmarkVidMulti

**Parameters for input**

   `-f <filename>` the video file being input, can specify multiple files by having multiple `-f` flags

   `-device <device_num>` the webcam from which to read images (default 0)

   `-inroot <directory>` the root of input so `-f` can be specified relative to it
   
## Common parameters for all

    -q specifying to use quiet mode not visualizing output (useful for running on servers)

**Model to use parameters**

    -mloc <the location of landmark detection models>

  Options for this:
        
- `"model/main_clnf_general.txt"` (default) - trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking (CLNF model)
- `"model/main_clnf_wild.txt"` - trained on In-the-wild data, works better in noisy environments (not very well suited for head pose tracking),  (CLNF in-the-wild model)
- `"model/main_clm_general.txt"` - a less accurate but slightly faster CLM model trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking

**Model parameters**

   `-wild` flag specifies when the images are more difficult, model considers extended search regions

   `-multi-view <0/1>` should multi-view initialisation be used (more robust, but slower), off by default

**Optional camera parameters for proper head pose and eye gaze computation**

	-fx <focal length in x>
	-fy <focal length in y>
	-cx <optical centre in x> 
	-cy <optical centre in y>