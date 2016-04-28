Best way to see how the command line arguments work is through looking at `./matlab_runners/Demos/` which illustrates how each of the models is called

## FaceLandmarkVid and FaceLandmarkVidMulti

Parameters for input (if nothing is specified attempts to read from a webcam with default values)

    -f <filename, the video file being input, can specify multiple files>
    -device <device_num, the webcam from which to read images (default 0)>
    -fd <depth directory, the directory where depth files are stored (deprecated)> 
    -root <the root of input and output directories>

Optional camera parameters for proper head pose visualization

	-fx <focal length in x>
	-fy <focal length in y>
	-cx <optical centre in x> 
	-cy <optical centre in y>

Parameters for output:

    -ov <location of visualized track>

## FaceLandmarkImg

Single image analysis:

    -f <filename, can have multiple -f flags> - the image file being input
    -of <location of output landmark points file> 
    -oi <location of tracked video>
    -root <the root of input and output directories>

Batch image analysis:
	
    -fdir <directory> - runs landmark detection on all images (.jpg and .png) in a directory, if the directory contains .txt files (image_name.txt) with bounding boxes, it will use those for initialisation
    -ofdir <directory> - where detected landmarks should be written
    -oidir <directory> - where images with detected landmarks should be stored

Model parameters (apply to images and videos):

    -wild flag specifies when the images are more difficult, model considers extended search regions
    -multi-view <0/1>, should multi-view initialisation be used (more robust, but slower)

Output format - The file format is same as 300 faces in the wild challenge annotations (http://ibug.doc.ic.ac.uk/resources/facial-point-annotations/)

## FeatureExtraction

## Common parameters for all

    -q specifying to use quiet mode not visualizing output

Model to use parameters:

    -mloc <the location of landmark detection models>

  Options for this:
        
- `"model/main_clnf_general.txt"` (default) - trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clnf_wild.txt"` - trained on In-the-wild data, works better in noisy environments (not very well suited for head pose tracking)
- `"model/main_clm_general.txt"` - a less accurate but slightly faster CLM model trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clm-z.txt"` - trained on Multi-PIE and BU-4DFE datasets, works with both intensity and depth signals (CLM-Z)
