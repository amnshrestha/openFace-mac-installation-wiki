For Unix based systems and different compilers, I included Cmake files for cross-platform and cross-IDE support.

# Ubuntu 14 installation

This code has been tested on Ubuntu 14.04.1

## Dependency installation

This requires cmake, OpenCV 3.1.0 (or newer), tbb and boost.

To acquire all of the dependencies follow the steps:

1. Get newest GCC, done using:

    `sudo apt-get update`

    `sudo apt-get install build-essential`

2. Cmake: 

    `sudo apt-get install cmake`

3. Get BLAS (for dlib)

    `sudo apt-get install libopenblas-dev liblapack-dev`

4. OpenCV 3.1.0

    4.1 Install OpenCV dependencies:

        sudo apt-get install git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

        sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev checkinstall

    4.2 Download OpenCV 3.1.0 from https://github.com/Itseez/opencv/archive/3.1.0.zip 

        wget https://github.com/Itseez/opencv/archive/3.1.0.zip

    4.3 Unzip it and create a build folder:	

        sudo unzip 3.1.0.zip
        cd opencv-3.1.0
        mkdir build
        cd build

    4.4 Build it using:

        cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_SHARED_LIBS=OFF ..

	`make -j2`

	`sudo make install`
	
5. Get Boost: 

    `sudo apt-get install libboost1.55-all-dev`

    alternatively: `sudo apt-get install libboost-all-dev`

## Actual OpenFace installation

1. Get OpenFace 

    `git clone https://github.com/TadasBaltrusaitis/OpenFace.git`

2. Compile it using

    `cd OpenFace`

    `cmake -D CMAKE_BUILD_TYPE=RELEASE . `

    `make`

3. Test it with 
    - for videos:	

        `./bin/FaceLandmarkVid -f "./videos/changeLighting.wmv" -f "./videos/0188_03_021_al_pacino.avi" -f "./videos/0217_03_006_alanis_morissette.avi" -f "./videos/0244_03_004_anderson_cooper.avi"`

    - for images:

        `./bin/FaceLandmarkImg -fdir "./videos/" -ofdir "./demo_img/" -oidir "./demo_img/" -wild`

    - for multiple faces in videos:

        `./bin/FaceLandmarkVidMulti -f ./videos/multi_face.avi`

    - for feature extraction (facial landmarks, head pose, AUs, gaze and HOG and similarity aligned faces):

        `./bin/FeatureExtraction -rigid  -verbose -f "./videos/default.wmv" -of "output_features/default.txt" -simalign output_features/aligned`

# Troubleshooting

If you experience a problem with "cannon connect to X server" when trying to execute the tracker, a solution can be found here http://askubuntu.com/questions/64820/wkhtmltopdf-wkhtmltoimage-cannot-connect-to-x-server, to resolve run:
    `apt-get install xvfb`
