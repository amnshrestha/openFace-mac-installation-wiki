# From binaries

Binaries for Windows coming soon

# From code
For Windows this software comes prepackaged with all the necessary binaries and dll's for compilation of the project, you still need to compile it in order to run it. You don't need to download anything additional, just open "OpenFace.sln" using Visual Studio 2015 and compile the code. The project was built and tested on Visual Studio 2015 (can't guarantee compatibility with other versions, and you would need to find/build the appropriate dll and lib files for them yourself). Code was tested on Windows 7/8/10 and Windows Server 2008 can't guarantee compatibility with other Windows versions (but in theory it should work). 

## Release Mode
NOTE be sure to run the project without debugger attached and in Release mode for speed (if running from Visual Studio). To run without debugger attach use CTRL + F5 instead of F5. To change from Debug mode to Release mode select Release from drop down menu in the toolbar. This can mean the difference between running at 5fps and 60fps on 320x240px videos. 

## Architecture
The software works both on 32 and 64 bit architectures. I  found that the x64 version seems to run faster on most machines.