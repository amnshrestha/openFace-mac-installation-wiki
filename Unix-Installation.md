# Install script 

On most systems you can install OpenFace using the `download_models.sh` followed by `./install.sh` script, if that does not work or you already have a number of dependencies installed and want to link to them use the following steps.

For more details on model downloads see - https://github.com/TadasBaltrusaitis/OpenFace/wiki/Model-acquisition

# Advanced Ubuntu installation

For Unix based systems and different compilers, I included Cmake files for cross-platform and cross-IDE support.

This code has been tested on Ubuntu 14.04, 16.04, and 18.04 with GCC

## Dependency installation

OpenFace requires cmake, OpenCV 3.1.0 (or newer), tbb, OpenBLAS, dlib and boost.

To acquire all of the dependencies follow the instructions pertaining to your compiler:

### Ubuntu gcc compiler instructions

If you already have any of the following dependencies you can skip those steps

1. Get newest GCC, done using:

    `sudo apt-get update`

    `sudo apt-get install build-essential`

2. Cmake:

    `sudo apt-get install cmake`

3. Get OpenBLAS

    `sudo apt-get install libopenblas-dev`

4. Download and compile OpenCV 3.4.0

    4.1 Install OpenCV dependencies:

        sudo apt-get install git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

        sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev

    4.2 Download OpenCV 3.4.0 from https://github.com/opencv/opencv/archive/3.4.0.zip

        wget https://github.com/opencv/opencv/archive/3.4.0.zip

    4.3 Unzip it and create a build folder:

        sudo unzip 3.4.0.zip
        cd opencv-3.4.0
        mkdir build
        cd build

    4.4 Build it using:

        cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_SHARED_LIBS=OFF ..

	`make -j2`

	`sudo make install`

5. Get Boost:

    `sudo apt-get install libboost-all-dev`

6. Download and compile dlib:

    wget http://dlib.net/files/dlib-19.13.tar.bz2;
    tar xf dlib-19.13.tar.bz2;
    cd dlib-19.13;
    mkdir build;
    cd build;
    cmake ..;
    cmake --build . --config Release;
    sudo make install;
    sudo ldconfig;
    cd ../..;    
	
## Actual OpenFace installation

1. Get OpenFace

    `git clone https://github.com/TadasBaltrusaitis/OpenFace.git`

2. Create an out-of-source build directory to store the compiled artifacts:

    cd OpenFace
    mkdir build
    cd build

3. Compile the code using:

    `cmake -D CMAKE_BUILD_TYPE=RELEASE CMAKE_CXX_FLAGS="-std=c++11" -D CMAKE_EXE_LINKER_FLAGS="-std=c++11" .. `

	`make`
 

3. Test it with
    - for videos:

        `./bin/FaceLandmarkVid -f "../samples/changeLighting.wmv" -f "../samples/2015-10-15-15-14.avi"`

    - for images:

        `./bin/FaceLandmarkImg -fdir "../samples/" -wild`

    - for multiple faces in videos:

        `./bin/FaceLandmarkVidMulti -f ../samples/multi_face.avi`

    - for feature extraction (facial landmarks, head pose, AUs, gaze and HOG and similarity aligned faces):

        `./bin/FeatureExtraction -verbose -f "../samples/default.wmv"`

# Troubleshooting

## X server

If you experience a problem with "cannot connect to X server" when trying to execute the tracker, a solution can be found here http://askubuntu.com/questions/64820/wkhtmltopdf-wkhtmltoimage-cannot-connect-to-x-server, to resolve run:
    `apt-get install xvfb`

## Anaconda

When Anaconda is installed, somehow OpenCV finds the outdated GCC 4.x instead of GCC 5.4, according to: https://stackoverflow.com/questions/40322301/compile-opencv-3-on-ubuntu-16-04-linking-error-usr-lib-x86-64-linux-gnu-libsox

This results in OpenFace giving the error:
usr/lib/x86_64-linux-gnu/libsoxr.so.0: undefined reference to `GOMP_parallel@GOMP_4.0'
collect2: error: ld returned 1 exit status