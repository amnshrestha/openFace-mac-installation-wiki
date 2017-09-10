For Unix based systems and different compilers, I included Cmake files for cross-platform and cross-IDE support.

# Ubuntu installation

This code has been tested on Ubuntu 14.04.1 with GCC, and on 15.10 with Clang 3.7.1.

You can also run the `install.sh` script for installing on Ubuntu 16.04 (it combines the following steps into one script)

## Dependency installation

This requires cmake, OpenCV 3.1.0 (or newer), tbb and boost.

To acquire all of the dependencies follow the instructions pertaining to your Operating System:

### Ubuntu 14.04

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

### Ubuntu 15.10

1. Get LLVM, Clang and libc++:

    1.1 Install LLVM and Clang dependencies:

    `sudo apt-get update`

    `sudo apt-get install build-essential`

    1.2 Install LLVM:

    `sudo apt-get install llvm`

    1.3 Install Clang along with relevant packages:

    * Check that the correct repository is enabled by inspecting '/etc/apt/sources.list':

        `sudo gedit /etc/apt/sources.list`

        If 'universe' is not included, modify the file so that it is:

        `deb http://us.archive.ubuntu.com/ubuntu wily main universe`

        After any such changes, update the system with:

        `sudo apt-get update`

    * Get Clang and libc++:

      `sudo apt-get install clang-3.7 libc++-dev libc++abi-dev`

2. Get Cmake:

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

        cmake -D CMAKE_CXX_COMPILER=clang++ -D CMAKE_CXX_FLAGS="-std=c++11 -stdlib=libc++ -I/usr/include/libcxxabi" -D CMAKE_EXE_LINKER_FLAGS="-std=c++11 -stdlib=libc++ -lc++abi" -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=<path to opencv_contrib modules> -D BUILD_TIFF=ON -D WITH_V4L=ON -D WITH_GTK=ON -D BUILD_opencv_dnn=OFF -D WITH_TBB=ON ..

	`make -j2`

	`sudo make install`

5. Get Boost 1.61.0:

    5.1 Install boost dependency:

    `sudo apt-get install libbz2-dev`

    5.2 Get the boost source:

    Download [boost_1_61_0.tar.bz2](http://www.boost.org/users/history/version_1_61_0.html).

    In the directory where you want to put the boost installation, execute:

    `tar --bzip2 -xf /path/to/boost_1_61_0.tar.bz2`

    5.3 Build and install:

        cd path/to/boost_1_61_0
        ./bootstrap.sh --with-toolset=clang --prefix=/usr/local
        ./b2 toolset=clang cxxflags="-std=c++11 -stdlib=libc++ -I/usr/include/libcxxabi" linkflags="-stdlib=libc++" --prefix=/usr/local -j 10 define=BOOST_SYSTEM_NO_DEPRECATED stage release
        sudo ./b2 install toolset=clang cxxflags="-std=c++11 -stdlib=libc++" linkflags="-stdlib=libc++" --prefix=/usr/local

## Actual OpenFace installation

1. Get OpenFace

    `git clone https://github.com/TadasBaltrusaitis/OpenFace.git`

2. Create an out-of-source build directory to store the compiled artifacts:

        cd OpenFace
        mkdir build
        cd build

3. Compile the code using instructions pertaining to your operating system:

    * Ubuntu 14.04

        `cmake -D CMAKE_BUILD_TYPE=RELEASE .. `

        `make`

    * Ubuntu 15.10

        `cmake -D CMAKE_CXX_COMPILER=clang++ -D CMAKE_CXX_FLAGS="-std=c++11 -stdlib=libc++ -I/usr/include/libcxxabi" -D CMAKE_EXE_LINKER_FLAGS="-std=c++11 -stdlib=libc++ -lc++abi" -D CMAKE_BUILD_TYPE=RELEASE ..`

        `make`
 

3. Test it with
    - for videos:

        `./bin/FaceLandmarkVid -f "../samples/changeLighting.wmv" -f "../samples/2015-10-15-15-14.avi"`

    - for images:

        `./bin/FaceLandmarkImg -fdir "../samples/" -ofdir "./demo_img/" -oidir "./demo_img/" -wild`

    - for multiple faces in videos:

        `./bin/FaceLandmarkVidMulti -f ../samples/multi_face.avi`

    - for feature extraction (facial landmarks, head pose, AUs, gaze and HOG and similarity aligned faces):

        `./bin/FeatureExtraction -rigid  -verbose -f "../samples/default.wmv" -of "output_features/default.txt" -simalign output_features/aligned`

# Troubleshooting

## X server

If you experience a problem with "cannot connect to X server" when trying to execute the tracker, a solution can be found here http://askubuntu.com/questions/64820/wkhtmltopdf-wkhtmltoimage-cannot-connect-to-x-server, to resolve run:
    `apt-get install xvfb`

## Anaconda

When Anaconda is installed, somehow OpenCV finds the outdated GCC 4.x instead of GCC 5.4, according to: https://stackoverflow.com/questions/40322301/compile-opencv-3-on-ubuntu-16-04-linking-error-usr-lib-x86-64-linux-gnu-libsox

This results in OpenFace giving the error:
usr/lib/x86_64-linux-gnu/libsoxr.so.0: undefined reference to `GOMP_parallel@GOMP_4.0'
collect2: error: ld returned 1 exit status