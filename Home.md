# OpenFace 2.0.0: a facial behavior analysis toolkit

Over the past few years, there has been an increased interest in automatic facial behavior analysis and understanding. We present OpenFace – a tool intended for computer vision and machine learning researchers, affective computing community and people interested in building interactive applications based on facial behavior analysis. OpenFace is the ﬁrst toolkit capable of facial landmark detection, head pose estimation, facial action unit recognition, and eye-gaze estimation with available source code for both running and training the models. The computer vision algorithms which represent the core of OpenFace demonstrate state-of-the-art results in all of the above mentioned tasks. Furthermore, our tool is capable of real-time performance and is able to run from a simple webcam without any specialist hardware.

![Multicomp logo](https://github.com/TadasBaltrusaitis/OpenFace/blob/master/imgs/muticomp_logo_black.png)

![Rainbow logo](https://github.com/TadasBaltrusaitis/OpenFace/blob/master/imgs/rainbow-logo.gif)

OpenFace is an implementation of a number of research papers from the Multicomp group, Language Technologies Institute at the Carnegie Mellon University and Rainbow Group, Computer Laboratory, University of Cambridge. The founder of the project and main developer is Tadas Baltrušaitis.

Special thanks goes to Louis-Philippe Morency and his MultiComp Lab at Carnegie Mellon University for help in writing and testing the code, Erroll Wood for the gaze estimation work, and Amir Zadeh and Yao Chong Lim on work on the CE-CLM model.

## Functionality

The system is capable of performing a number of facial analysis tasks:

- Facial Landmark Detection

![Sample facial landmark detection image](https://github.com/TadasBaltrusaitis/OpenFace/blob/master/imgs/multi_face_img.png)

- Facial Landmark and head pose tracking (links to YouTube videos)

<a href="https://www.youtube.com/watch?v=V7rV0uy7heQ" target="_blank"><img src="http://img.youtube.com/vi/V7rV0uy7heQ/0.jpg" alt="Multiple Face Tracking" width="240" height="180" border="10" /></a>
<a href="https://www.youtube.com/watch?v=vYOa8Pif5lY" target="_blank"><img src="http://img.youtube.com/vi/vYOa8Pif5lY/0.jpg" alt="Multiple Face Tracking" width="240" height="180" border="10" /></a>

- Facial Action Unit Recognition

<img src="https://github.com/TadasBaltrusaitis/OpenFace/blob/master/imgs/au_sample.png" height="280" width="600" >

- Gaze tracking (image of it in action)

<img src="https://github.com/TadasBaltrusaitis/OpenFace/blob/master/imgs/gaze_ex.png" height="182" width="600"  >

- Facial Feature Extraction (aligned faces and HOG features)

![Sample aligned face and HOG image](https://github.com/TadasBaltrusaitis/OpenFace/blob/master/imgs/appearance.png)


## Installation

[Windows](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Windows-Installation)

[Ubuntu](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Unix-Installation)

[Mac](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Mac-Installation)

### Quickstart usage of OpenFace with Docker (thanks Edgar Aroutiounian)

Do:

```
$ docker run -it --rm algebr/openface:latest
```

And this will open up a shell in a prebuilt OpenFace project.

Then find its container ID:

```
$ docker ps
CONTAINER ID        IMAGE                    COMMAND             CREATED              STATUS              PORTS               NAMES
3a73fbce562e        algebr/openface:latest   "/bin/bash"         About a minute ago   Up About a minute                       musing_wiles
```

Then you can copy an image to the running container:

```
$ docker cp samples/sample1.jpg 3a73fbce562e:/home/openface-build
```

And in the first shell you can test it out:

```
$ build/bin/FaceLandmarkImg -f sample1.jpg
```

## Use

[Command line interface](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Command-line-arguments)

GUI for Windows

Messaging server (Coming soon)

## Citation

If you use any of the resources provided on this page in any of your publications we ask you to cite the following work and the work for a relevant submodule you used.

#### Overall system

**OpenFace 2.0: Facial Behavior Analysis Toolkit**
Tadas Baltrušaitis, Amir Zadeh, Yao Chong Lim, and Louis-Philippe Morency,
*IEEE International Conference on Automatic Face and Gesture Recognition*, 2018 

#### Facial landmark detection and tracking

**Convolutional experts constrained local model for facial landmark detection**
A. Zadeh, T. Baltrušaitis, and Louis-Philippe Morency. 
*Computer Vision and Pattern Recognition Workshops*, 2017

**Constrained Local Neural Fields for robust facial landmark detection in the wild**
Tadas Baltrušaitis, Peter Robinson, and Louis-Philippe Morency. 
in IEEE Int. *Conference on Computer Vision Workshops, 300 Faces in-the-Wild Challenge*, 2013.  

#### Eye gaze tracking

**Rendering of Eyes for Eye-Shape Registration and Gaze Estimation**
Erroll Wood, Tadas Baltrušaitis, Xucong Zhang, Yusuke Sugano, Peter Robinson, and Andreas Bulling 
in *IEEE International. Conference on Computer Vision (ICCV)*,  2015 

#### Facial Action Unit detection

**Cross-dataset learning and person-specific normalisation for automatic Action Unit detection**
Tadas Baltrušaitis, Marwa Mahmoud, and Peter Robinson 
in *Facial Expression Recognition and Analysis Challenge*, 
*IEEE International Conference on Automatic Face and Gesture Recognition*, 2015 

# Commercial license

For inquiries about the commercial licensing of the OpenFace toolkit please visit https://www.flintbox.com/public/project/50632/ 

# Final remarks

I did my best to make sure that the code runs out of the box but there are always issues and I would be grateful for your understanding that this is research code and a research project. If you encounter any problems/bugs/issues please contact me on github or by emailing me at tadyla@gmail.com for any bug reports/questions/suggestions. I prefer questions and bug reports on github as that provides visibility to others who might be encountering same issues or who have the same questions.

# Copyright

Copyright can be found in the Copyright.txt

You have to respect boost, TBB, dlib, OpenBLAS, and OpenCV licenses.

Furthermore you have to respect the licenses of the datasets used for model training - https://github.com/TadasBaltrusaitis/OpenFace/wiki/Datasets
