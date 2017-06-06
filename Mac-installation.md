# Mac installation

Here's what I did to get OpenFace running on my Mac under OS X El Capitan 10.11.4.  Your needs may differ depending on what you already have installed, but the following may help.

## Prerequisites

* I recommend installing [Homebrew](http://brew.sh) as the easiest way to get a variety of Open Source libraries.  Think of it as the Mac equivalent of `apt-get`.  Homebrew usually installs things under `/usr/local/Cellar` and then creates links to the version you're using in `/usr/local/bin`, `/usr/local/lib` etc.  This will be relevant later.

* You'll want the the [Command Line Tools for Xcode](https://developer.apple.com/downloads/). If you can't use these for any reason, you can build `gcc` using Homebrew -- in fact, it will happen automatically -- but it'll be much slower.

* Get [XQuartz](https://www.xquartz.org) (an X Window system for OS X).  You don't actually need it to *run* OpenFace, but having the X libraries and include files on your system will make OpenFace (and various other things) much easier to build.

* Install *boost*, TBB, and OpenCV with:

    `brew tap homebrew/science`

    `brew install boost tbb opencv3`

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



*[Quentin Stafford-Fraser](http://quentinsf.com), May 2016*
