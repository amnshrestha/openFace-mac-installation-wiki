## FaceLandmarkVid and FaceLandmarkVidMulti

Parameters for input (if nothing is specified attempts to read from a webcam with default values)

	-f <filename, the video file being input>
	-device <device_num, the webcam from which to read images (default 0)>
	-fd <depth directory, the directory where depth files are stored (deprecated)> 

Optional camera parameters for proper head pose visualization

	-fx <focal length in x>
	-fy <focal length in y>
	-cx <optical centre in x> 
	-cy <optical centre in y>

Parameters for output:

    -ov <location of visualized track>

Model parameters:

    -mloc <the location of landmark detection models>

  Options for this:
        
- `"model/main_clnf_general.txt"` (default) - trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clnf_wild.txt"` - trained on In-the-wild data, works better in noisy environments (not very well suited for head pose tracking)
- `"model/main_clm_general.txt"` - a less accurate but slightly faster CLM model trained on Multi-PIE of varying pose and illumination and In-the-wild data, works well for head pose tracking
- `"model/main_clm-z.txt"` - trained on Multi-PIE and BU-4DFE datasets, works with both intensity and depth signals (CLM-Z)