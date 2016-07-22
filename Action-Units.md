## Facial Action Coding System

![Examples of Action Units](https://raw.githubusercontent.com/wiki/TadasBaltrusaitis/OpenFace/images/AUs.jpg)

Facial Action Coding System (FACS) is a system to taxonomize human facial movements by their appearance on the face. Movements of individual facial muscles are encoded by FACS from slight different instant changes in facial appearance. Using FACS it is possible to code nearly any anatomically possible facial expression, deconstructing it into the specific Action Units (AU) that produced the expression. It is a common standard to objectively describe facial expressions. 

OpenFace is able to recognize a subset of AUs, specifically: 1, 2, 4, 5, 6, 7, 9, 10, 12, 14, 15, 17, 20, 23, 25, 26, 28, and 45.

You can find more details about FACS and AUs [here](https://en.wikipedia.org/wiki/Facial_Action_Coding_System) and [here](https://www.cs.cmu.edu/~face/facs.htm)

## Intensity and presence of AUs

AUs can be described in two ways
- Presence - if AU is visible in the face (for example AU01_c
- Intensity - how intense is the AU (minimal to maximal) on a 5 point scale

OpenFace provides both of these scores. For presence of AU 1 the column `AU01_c` in the output file would encode 0 as not present and 1 as present. For intensity of AU 1 the column `AU01_r` in the output file would range from 0 (not present), 1 (present at minimum intensity), 5 (present at maximum intensity), with continuous values in between.

## Extraction from images and extraction from videos

OpenFace is able to extract Action Units both from images, image sequences and videos

### Individual Images

Use `FaceLandmarkImg` project and executable for this, it will output AU predictions for each image. Please note that the accuracy of AU prediction on individual images is not as high as that of AU prediction on videos because videos allow for person specific calibration.

### Image sequences and videos

If you want to extract 

- Static vs dynamic

- How they are trained and where the instructions for training are

- Datasets used for training

