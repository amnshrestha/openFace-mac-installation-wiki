A number of publicly available datasets have been used to train and evaluate the OpenFace system, this page lists all of the datasets used. Without the availability of these datasets, OpenFace would not be possible and we thank their creators. Also please consider citing the datasets if you are using the relevant submodules of OpenFace.

## Datasets used for training and evaluating facial landmarks
- [CMU Multi-PIE](http://www.cs.cmu.edu/afs/cs/project/PIE/MultiPie/Multi-Pie/Home.html)
- [300W](https://ibug.doc.ic.ac.uk/resources/facial-point-annotations/), we used the HELEN and LFPW training subsets for training and the rest for testing

## Datasets used for training AUs in OpenFace

Action Unit predictors have been trained and evaluated on the following datasets:
- [Bosphorus](http://bosphorus.ee.boun.edu.tr/)
- [BP4D from FERA2015](http://sspnet.eu/fera2015/)
- [DISFA](http://www.engr.du.edu/mmahoor/DISFA.htm)
- [FERA2011](http://sspnet.eu/fera2011/fera2011data/)
- [SEMAINE from FERA2015](http://sspnet.eu/fera2015/)
- [UNBC](http://www.pitt.edu/~emotion/um-spread.htm)
- [CK+](http://www.pitt.edu/~emotion/ck-spread.htm)

More details on the actual training procedure can be found here - https://github.com/TadasBaltrusaitis/OpenFace/blob/master/matlab_version/AU_training/instructions.txt

## Datasets used for evaluating head pose

- [ICT-3DHP](http://projects.ict.usc.edu/3dhp/)
- [Boston University](http://www.cs.bu.edu/groups/ivc/data.php)
- [Biwi](https://data.vision.ee.ethz.ch/cvl/gfanelli/head_pose/head_forest.html)

## Datasets used for evaluating eye gaze

- [MPII](https://www.mpi-inf.mpg.de/departments/computer-vision-and-multimodal-computing/research/gaze-based-human-computer-interaction/appearance-based-gaze-estimation-in-the-wild/), note we used a private subset with full face images that is not available to the public