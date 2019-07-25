# Install script 

On most systems you can install OpenFace using the `bash ./download_models.sh` followed by `sudo bash ./install.sh` script. This is recommended if you are not as familiar with compiling C++ code or installing libraries. However, this is not recommended if you already have a number of dependencies installed (see below) and want to link to them instead of getting another version. See below steps 

For more details on model downloads see - https://github.com/TadasBaltrusaitis/OpenFace/wiki/Model-acquisition

# Advanced Ubuntu installation (if not using ./install.sh, or if it fails)

For Unix based systems and different compilers, I included Cmake files for cross-platform and cross-IDE support.

This code has been tested on Ubuntu 14.04, 16.04, and 18.04 with a C++17 GCC compiler (clang should also work, but was not tested extensively)

## Dependency installation

OpenFace requires cmake, OpenCV 4.0.0 (or newer), OpenBLAS, dlib, C++17 compiler (tbb and boost are optional additional dependencies that will be used if present).

If you already have any of the following dependencies you can skip those steps

1. Get newest GCC, done using:

        sudo apt-get update
        sudo apt-get install build-essential
        sudo apt-get install g++-8

    *Note if on Ubuntu 16.04 or lower*, you will need the following lines before installing newest g++:

        sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y
        sudo apt-get -y update

	
2. Cmake:

    `sudo apt-get install cmake`

	*Note if on Ubuntu 16.04 or lower*, CMake version required by OpenFace is at least 3.8 and Ubuntu 16.04 apt-get only support up to CMake 3.5, to install a newer version of CMake follow:

        sudo apt-get --purge remove cmake-qt-gui -y
        sudo apt-get --purge remove cmake -y
        mkdir -p cmake_tmp
        cd cmake_tmp
        wget https://cmake.org/files/v3.10/cmake-3.10.1.tar.gz
        tar -xzvf cmake-3.10.1.tar.gz -qq
        cd cmake-3.10.1/
        ./bootstrap
        make -j4
        sudo make install
        cd ../..
        sudo rm -rf cmake_tmp

	
3. Get OpenBLAS

    `sudo apt-get install libopenblas-dev`

4. Download and compile OpenCV 4.1.0

    4.1 Install OpenCV dependencies:

        sudo apt-get install git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

        sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev

    4.2 Download OpenCV 4.1.0 from https://github.com/opencv/opencv/archive/4.1.0.zip

        wget https://github.com/opencv/opencv/archive/4.1.0.zip

    4.3 Unzip it and create a build folder:

        sudo unzip 4.1.0.zip
        cd opencv-4.1.0
        mkdir build
        cd build

    4.4 Build it using:

        cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_TIFF=ON -D WITH_TBB=ON ..
        make -j2
        sudo make install

5. Download and compile dlib:

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

6. Get Boost (optional):

    `sudo apt-get install libboost-all-dev`
	
## Actual OpenFace installation

1. Get OpenFace

    `git clone https://github.com/TadasBaltrusaitis/OpenFace.git`

2. Create an out-of-source build directory to store the compiled artifacts:

        cd OpenFace
        mkdir build
        cd build

3. Compile the code using (if using g++ version 8, change to clang or other version appropriately):

        cmake -D CMAKE_CXX_COMPILER=g++-8 -D CMAKE_C_COMPILER=gcc-8 -D CMAKE_BUILD_TYPE=RELEASE ..
        make

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