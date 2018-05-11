*NOTE* I don't have a Mac to easily test and amend these instructions, but a lot of people managed to get it working. If anyone wants to edit the wiki with better and more up to date Mac instructions please do!

# Mac installation

Here's what I did to get OpenFace running on my Mac under OS X El Capitan 10.11.4.  Your needs may differ depending on what you already have installed, but the following may help.

## Prerequisites

* The landmark detection model is not included due to file size, you can download it using the `download_models.sh` script. For more details see - https://github.com/TadasBaltrusaitis/OpenFace/wiki/Model-acquisition

* I recommend installing [Homebrew](http://brew.sh) as the easiest way to get a variety of Open Source libraries.  Think of it as the Mac equivalent of `apt-get`.  Homebrew usually installs things under `/usr/local/Cellar` and then creates links to the version you're using in `/usr/local/bin`, `/usr/local/lib` etc.  This will be relevant later.

* You'll want the the [Command Line Tools for Xcode](https://developer.apple.com/downloads/). If you can't use these for any reason, you can build `gcc` using Homebrew -- in fact, it will happen automatically -- but it'll be much slower.

* Get [XQuartz](https://www.xquartz.org) (an X Window system for OS X).  You don't actually need it to *run* OpenFace, but having the X libraries and include files on your system will make OpenFace (and various other things) much easier to build.

* Install *boost*, TBB, and OpenCV with:

    `brew tap homebrew/science`

    `brew install tbb opencv3`

    `brew install boost`

    `brew install openblas`

You can also compile OpenCV from code directly.

Boost versions above 1.65 might clash with the dlib version used in the code, in order to compile the code successfully you might need to downgrade to boost 1.50 (or above, but below 1.65). To install a particular version of boost follow - https://stackoverflow.com/questions/104322/how-do-you-install-boost-on-macos

Alternatively, have a look at - https://github.com/TadasBaltrusaitis/OpenFace/issues/340, https://github.com/TadasBaltrusaitis/OpenFace/issues/258

## Tweaks

I needed to tell CMake where to find the OpenCV3 libraries.  At the time of writing, v2 is the default under Homebrew and is installed in the standard locations, so to use v3, you need to change the appropriate `find_package` line in CMakeLists.txt to give it a hint about where to look; something like:

    find_package( OpenCV HINTS /usr/local/Cellar/opencv3/3.1.0_3/ )

(specifying the appropriate version)

In addition, you may find it useful to tell CMake how to find your X Windows libraries, since they may not be in the same place as expected on Linux.  Add the following lines to CMakeLists.txt (e.g. after the similar section for Boost):

    find_package( X11 REQUIRED )
    MESSAGE("X11 information:")
    MESSAGE("  X11_INCLUDE_DIR: ${X11_INCLUDE_DIR}")
    MESSAGE("  X11_LIBRARIES: ${X11_LIBRARIES}")
    MESSAGE("  X11_LIBRARY_DIRS: ${X11_LIBRARY_DIRS}")
    include_directories( ${X11_INCLUDE_DIR} )

## Building

After that, the build process is very similar to Linux.

    cmake -D CMAKE_BUILD_TYPE=RELEASE .
    make

and you should have binaries in the `bin` directory. e.g. you can run:

    bin/FaceLandmarkVid

A small point: on my system, selecting Quit from the application's menu didn't work, but typing Cmd-Q did.

## Troubleshooting

Boost versions above 1.65 clash with the dlib version used in the code, in order to compile the code successfully you might need to downgrade to boost 1.50 (or above, but below 1.65). To install a particular version of boost follow - https://stackoverflow.com/questions/104322/how-do-you-install-boost-on-macos

If you're still having issues with boost have a look at - https://github.com/TadasBaltrusaitis/OpenFace/issues/258

*[Quentin Stafford-Fraser](http://quentinsf.com), May 2016*


