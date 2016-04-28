# Binaries

Windows binaries can be found here - [http://www.cl.cam.ac.uk/~tb346/software/OpenFace_0.1_command_line.zip](http://www.cl.cam.ac.uk/~tb346/software/OpenFace_0.1_command_line.zip)

# From code
For Windows this software comes prepackaged with all the necessary binaries and dll's for compilation of the project, you still need to compile it in order to run it. You don't need to download anything additional, just open "OpenFace.sln" using Visual Studio 2015 and compile the code. The project was built and tested on Visual Studio 2015 (can't guarantee compatibility with other versions, and you would need to find/build the appropriate dll and lib files for them yourself). Code was tested on Windows 7/8/10 and Windows Server 2008 can't guarantee compatibility with other Windows versions (but in theory it should work). 

### Release Mode
NOTE be sure to run the project without debugger attached and in Release mode for speed (if running from Visual Studio). To run without debugger attach use CTRL + F5 instead of F5. To change from Debug mode to Release mode select Release from drop down menu in the toolbar. This can mean the difference between running at 5fps and 60fps on 320x240px videos. 

### Architecture
The software works both on 32 and 64 bit architectures. I  found that the x64 version seems to run faster on most machines.

### Testing

To test if the system compiled correctly or to test binaries go to `matlab_runners\Demos` and attempt to run all the demo scripts (from Matlab)
  - `run_demo_videos.m` tracking videos
  - `run_demo_video_multi.m` multiple faces in videos
  - `run_demo_images.m` landmark detection in images
  - `gaze_extraction_demo_vid.m` gaze in videos
  - `feature_extraction_demo_vid.m` various features (pose, landmarks, gaze, and Action Units from a video)

Alternatively if you don't have matlab try these command line arguments from `Release` directory (add extra `../` if running from `x64/Release/`:

- `FaceLandmarkImg.exe -wild -fdir "../videos/" -ofdir "../matlab_runners/demo_img/" -oidir "../matlab_runners/demo_img/"`

- `FaceTrackingVid.exe -f "../videos/changeLighting.wmv" -f "../videos/0188_03_021_al_pacino.avi" -f "../videos/0217_03_006_alanis_morissette.avi"`

See [command line arguments](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments) for more details of how to use the binaries