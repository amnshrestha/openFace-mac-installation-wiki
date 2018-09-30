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

Alternatively, if you cannot access Dropbox, you can find C++ patch experts here (you will need to download all four .dat files):

OneDrive:

* [scale 0.25](https://onedrive.live.com/download?cid=2E2ADA578BFF6E6E&resid=2E2ADA578BFF6E6E%2153072&authkey=AKqoZtcN0PSIZH4)
* [scale 0.35](https://onedrive.live.com/download?cid=2E2ADA578BFF6E6E&resid=2E2ADA578BFF6E6E%2153079&authkey=ANpDR1n3ckL_0gs)
* [scale 0.50](https://onedrive.live.com/download?cid=2E2ADA578BFF6E6E&resid=2E2ADA578BFF6E6E%2153074&authkey=AGi-e30AfRc_zvs)
* [scale 1.00](https://onedrive.live.com/download?cid=2E2ADA578BFF6E6E&resid=2E2ADA578BFF6E6E%2153070&authkey=AD6KjtYipphwBPc)

Google drive:

* [scale 0.25](https://drive.google.com/uc?export=download&id=1TM_L_qNgd513z5i_T4CuXOF1Vl5DDu1l)
* [scale 0.35](https://drive.google.com/uc?export=download&id=1o2DmUO7jzjIbimsRXmjbNilPl_pcHvnJ)
* [scale 0.50](https://drive.google.com/uc?export=download&id=1bo0TAEH-2j8feBb3nYKPk5k0ODgfpEPz)
* [scale 1.00](https://drive.google.com/uc?export=download&id=1b8semX96A2yNe194PvKh_rkU1frcI4jr)

