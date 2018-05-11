There are a number of trained models that OpenFace uses and the code used for training them is available.

# Facial Landmark Detection

OpenFace uses the [Convolutional Expert Constrained Local Model](http://www.cl.cam.ac.uk/~tb346/pub/papers/cvpr2017-menpo.pdf) model for facial landmark detection, it requires three trained modules - Point Distribution Model, Patch Experts, and landmark verification module.

## Point Distribution model

To train the statistical model of shape go to:
`model_training/pdm_generation` and read the readme file there

More details [here](http://www.cl.cam.ac.uk/~tb346/pub/thesis/phd_thesis.pdf) in Chapter 4.2 specifically 4.2.5

## Patch Experts

You can find the patch expert training code here:
- `./model_training/ce-clm_training` for CEN patch expert training
- `./model_training/ccnf` for LNF and SVR patch expert training 

## Face validation

A module for validating face detections (training and inference), it is used for tracking in videos so as to know when reinitialisation is needed:
`matlab_version/face_validation`

Follow the readme there for instructions

# Action Unit recognition

More information about Facial Action Units can be found here - [https://github.com/TadasBaltrusaitis/OpenFace/wiki/Action-Units](https://github.com/TadasBaltrusaitis/OpenFace/wiki/Action-Units)

To train Facial Action Unit detectors follow the instructions in the readme file:
`model_training/AU_training`

More details about the method used [here](http://www.cl.cam.ac.uk/~tb346/pub/papers/wacv2016.pdf)
