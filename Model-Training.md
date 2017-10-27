There are a number of trained models that OpenFace uses and the code for training all of them is available.

# Facial Landmark Detection

OpenFace uses the [Constrained Local Neural Field](http://www.cl.cam.ac.uk/~tb346/pub/papers/iccv2013.pdf) model for facial landmark detection, it requires three trained modules - Point Distribution Model, Patch Experts, and landmark verification module.

## Point Distribution model

To train the statistical model of shape go to:
`matlab_version/pdm_generation` and read the readme file there

More details [here](http://www.cl.cam.ac.uk/~tb346/pub/thesis/phd_thesis.pdf) in Chapter 4.2 specifically 4.2.5

## Patch Experts

You can find the patch training code here - [https://github.com/TadasBaltrusaitis/CCNF](https://github.com/TadasBaltrusaitis/CCNF)

More details about the method itself [here](http://www.cl.cam.ac.uk/~tb346/pub/thesis/phd_thesis.pdf) in Chapters 4.3, 6.2, and 6.3 and [here](http://www.cl.cam.ac.uk/~tb346/pub/papers/iccv2013.pdf)

## Face validation

A module for validating face detections (training and inference), it is used for tracking in videos so as to know when reinitialisation is needed:
`matlab_version/face_validation`

Follow the readme there for instructions

# Action Unit recognition

More information about Facial Action Units can be found here - [https://github.com/TadasBaltrusaitis/OpenFace/wiki/Action-Units](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Action-Units)

To train Facial Action Unit detectors follow the instructions in the readme file:
`matlab_version/AU_training`

More details about the method used [here](http://www.cl.cam.ac.uk/~tb346/pub/papers/wacv2016.pdf)
