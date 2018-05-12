# Binaries

Windows binaries can be found here:
- 64-bit [https://github.com/TadasBaltrusaitis/OpenFace/releases/download/OpenFace_v2.0.0/OpenFace_v2.0.0_win_x64.zip](https://github.com/TadasBaltrusaitis/OpenFace/releases/download/OpenFace_v2.0.0/OpenFace_2.0.0_win_x64.zip)

For the binaries to work you need to have Visual Studio 2015 installed or need to install the 64-bit Visual C++ redistributable package, that can be found [here](https://www.microsoft.com/en-us/download/details.aspx?id=52685).

Explanation of how to use the command line binaries can be found [here](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments)

Alternatively you can use the Windows GUI (`OpenFaceOffline.exe` or `OpenFaceDemo.exe`), which has mostly the same functionality as the command line interface.

# From code
For Windows this software comes prepackaged with all the necessary binaries and dll's for compilation of the project, you still need to compile it in order to run it. You don't need to download anything additional, just open "OpenFace.sln" using Visual Studio 2015 and compile the code. The project was built and tested on Visual Studio 2015 (can't guarantee compatibility with other versions, and you would need to find/build the appropriate dll and lib files for them yourself). Code was tested on Windows 7/8/10 and Windows Server 2008 can't guarantee compatibility with other Windows versions (but in theory it should work). 

### Model download

For CE-CLM landmark detector to work you need to download additional model files, this can be done by executing the `download_models.ps1` PowerShell script. For more details on model download see - https://github.com/TadasBaltrusaitis/OpenFace/wiki/Model-acquisition


### Release Mode
NOTE be sure to run the project without debugger attached and in Release mode for speed (if running from Visual Studio). To run without debugger attach use CTRL + F5 instead of F5. To change from Debug mode to Release mode select Release from drop down menu in the toolbar. This can mean the difference between running at 5fps and 60fps on 320x240px videos. 

NOTE 2 make sure the startup project you select is one of the projects in Executables (`FaceLandmarkImg`, `FaceLandmarkVid`, `FaceLandmarkVidMulti`, or `FeatureExtraction`) as other projects (`dlib`, `FaceAnalyser`, `LandmarkDetector`) are libraries and will not work as executables. To change the start up project, right click on the project in the Solution Explorer and select *Set as StartUp project*

### Architecture
The software works both on 32 and 64 bit architectures. I  found that the x64 version seems to run faster on most machines. Furthermore, all the demo scripts assume you compiled the x64 version. To change versions go to the top bar and change Win32 to x64 (next to the Debug/Release choice).

### Testing

You can test if the models compiled correctly by launching the GUI using `OpenFaceOffline.exe` or `OpenFaceDemo.exe` from the release directory (`Release` or `x64/Release/`). This will launch a GUI for analyzing images/videos/live feeds with the former or a live feed only with the latter.

To test if the system compiled correctly or to test binaries go to `matlab_runners\Demos` and attempt to run all the demo scripts (from Matlab)
  - `run_demo_videos.m` tracking videos
  - `run_demo_video_multi.m` multiple faces in videos
  - `run_demo_images.m` landmark detection in images
  - `gaze_extraction_demo_vid.m` gaze in videos
  - `feature_extraction_demo_vid.m` various features (pose, landmarks, gaze, and Action Units from a video)

Alternatively if you don't have Matlab try these command line arguments from `Release` directory (add extra `../` if running from `x64/Release/`:

- `FaceLandmarkImg.exe -wild -fdir "../samples/" -verbose`

- `FaceTrackingVid.exe -f "../samples/changeLighting.wmv" -f "../samples/default.wmv"`

- `FeatureExtraction.exe -f "../samples/changeLighting.wmv" -verbose`

See [command line arguments](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments) for more details of how to use the binaries