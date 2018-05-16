OpenFace comes pre-packaged with most models required to run it. However, due to github file size restrictions it does not include the patch expert files required for the CE-CLM algorithm. 

# Automatic download

In order to download them to be used in the C++ executables you can use the two provided scripts:
- download_models.ps1 (PowerShell script for Windows)
- download_models.sh (bash script for Ubuntu and Mac OS X)


The above scripts will download the models required to run the C++ code. For Matlab models you will need to use the manual approach

# Manual download

To download the C++ and matlab models go to - https://www.dropbox.com/sh/o8g1530jle17spa/AADRntSHl_jLInmrmSwsX-Qsa?dl=0

The C++ models have the `.dat` extension and should be place in the `lib\local\LandmarkDetector\model\patch_experts` folder if you are compiling form code, and in the `model\patch_experts` folder if you have downloaded the binaries.

The Matlab models have the `.mat` extension and should be placed in the `.\matlab_version\models\cen` folder
