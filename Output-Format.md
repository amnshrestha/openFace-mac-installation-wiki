The two main projects that provide output files (that are not just videos) are **FaceLandmarkImg** and **FeatureExtraction** and their output is explained in detail in this page

## FaceLandmarkImg

The detected landmarks in an image are output in a text file, with a postfix `_det_n` if there are multiple faces in an image:

    version: 1
    npoints: 68
    {
    603.825 193.492
    604.934 209.987
    ...
    662.5 252.346
    654.784 251.901
    }

Each line corresponds to a facial landmark location in pixels `(x y)`, the landmark indices follow the following scheme:

<img src="http://ibug.doc.ic.ac.uk/media/uploads/images/300-w/figure_1_68.jpg" height="400" width="400" >

## FeatureExtraction
Single output file

Similarity aligned work

HOG features