*NOTE* I don't have a Mac to easily test and amend these instructions, but it has been tested through Travis Continuous Integration tool and a lot of people managed to get it working. If anyone wants to edit the wiki with better and more up to date Mac instructions please do!

# Mac installation

## Prerequisites

* The landmark detection model is not included due to file size, you can download it using the bash `download_models.sh` script. For more details see - https://github.com/TadasBaltrusaitis/OpenFace/wiki/Model-acquisition

* I recommend installing [Homebrew](http://brew.sh) as the easiest way to get a variety of Open Source libraries.  Think of it as the Mac equivalent of `apt-get`.  Homebrew usually installs things under `/usr/local/Cellar` and then creates links to the version you're using in `/usr/local/bin`, `/usr/local/lib` etc.  This will be relevant later.

* You'll want the [Command Line Tools for Xcode](https://developer.apple.com/downloads/). If you can't use these for any reason, you can build `gcc` using Homebrew -- in fact, it will happen automatically -- but it'll be much slower.

* Get [XQuartz](https://www.xquartz.org) (an X Window system for OS X).  You don't actually need it to *run* OpenFace, but having the X libraries and include files on your system will make OpenFace (and various other things) much easier to build.

* Install *boost*, TBB, dlib, OpenBLAS, and OpenCV with:

    brew update
    brew install tbb
    brew install openblas
    brew install dlib
    brew install opencv3
    brew install boost

In addition, you may find it useful to tell CMake how to find your X Windows libraries, since they may not be in the same place as expected on Linux.  Add the following lines to CMakeLists.txt (e.g. after the similar section for Boost):

    find_package( X11 REQUIRED )
    MESSAGE("X11 information:")
    MESSAGE("  X11_INCLUDE_DIR: ${X11_INCLUDE_DIR}")
    MESSAGE("  X11_LIBRARIES: ${X11_LIBRARIES}")
    MESSAGE("  X11_LIBRARY_DIRS: ${X11_LIBRARY_DIRS}")
    include_directories( ${X11_INCLUDE_DIR} )

## Building

After that, the build process is very similar to Linux (in OpenFace directory execute the following).

    mkdir build
    cd build
    cmake -D CMAKE_BUILD_TYPE=RELEASE CMAKE_CXX_FLAGS="-std=c++11" -D CMAKE_EXE_LINKER_FLAGS="-std=c++11" .. 
    make

and you should have binaries in the `bin` directory. e.g. you can run:

    build/bin/FaceLandmarkVid -device 0

