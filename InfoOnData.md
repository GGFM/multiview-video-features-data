Two ways to obtain the data:

**The entire dataset (3G compressed) is available from the UCI Machine Learning Repository.
http://archive.ics.uci.edu/ml/datasets.html    (under the name "YouTube Multiview Video Games Data")**

**[This document contains pointers to obtain the data in smaller chunks (e.g., to get the text features only). ](https://docs.google.com/document/d/1BKnjdQ2od-n1nLdEVw7rCSwZG6cAbZGsTDw_HpGeO3E/edit?usp=sharing)**


---


README:

This dataset consists of feature values and class labels for about
120,000 videos (instances). Each instance is described by up to 13
feature types, from 3 high level feature families: textual, visual,
and auditory features. There are 31 class labels, 1 through 31. The
first 30 labels correspond to popular video games. Class 31 is not
specific, and means none of the 30.

Neither the identity of the videos nor the class labels (video-game
titles) are released.

The dataset should be useful particularly for research on multiview
(multimodal) learning, including multiview clustering and/or
supervised learning, co-training, early/late fusion, and ensemble
techniques.

Please cite the following when publishing using the data.

@article{madaniEtAl2013MLJ,
> title= {On Using Nearly-Independent Feature Families
> > for High Precision and Confidence}

> author = {Omid Madani and Manfred Georg and David A. Ross},
> journal = {Machine Learning},
> year = {2013},
> volume = {92},
> pages = {457-477},
> note = {published online 30 May 2013,
http://rd.springer.com/article/10.1007%2Fs10994-013-5377-0},
}

Many thanks to the Google video analysis and perception teams, and to
Tomas Izo, Jay Yagnik, and Andrea Held for their assistance and
support.

For questions about the data set, contact Omid Madani.



Details.

The instances are partitioned into training (80%), validation (10%),
and test (10%).

Each instance is described by up to 13 feature types, and there exist
a file for each feature type and partition combination (training,
validation, test).  Therefore, each of training, validation, and test
directories consists of 13 files, for a total of 39.  Entire feature
families may be missing for some instances.  See below for the list of
features.

The instances are labeled by one of 31 class labels, 1 through
31. Each label except label 31 corresponds to a video game picked at
random from roughly top 100 most popular games (as of
2012). Typically, someone played the game and recorded it and uploaded
it on YouTube to serve as a tutorial, e.g., on how to play a certain
stage of the game, to share playing experience, and so on.  For
classes 1 through 30, about 3000 instances were randomly picked and
added to the dataset. About 30k videos (~26% of all instances),
related to video gaming, that did not belong to any of the selected 30
game classes were also added to the data set (label 31).  Class 31 is
also very learnable, i.e., classifier performance can be significantly
better than a random baseline, because it is dominated by other common
classes (popular games).

Label noise: The videos for each class (game title) were obtained via
title matches and some subsequent rule-based cleaning. There remains
some label noise, perhaps around 5-10% of the positive instances for
each class.

Performance of linear SVMs (liblinear package) is reported on class 1
for reference. See below.

####  ####

The structure of each file.

Each file is in "sparse format" (akin to libsvm). Each line, if not
blank, is either a comment line, if it begins with '#', or contains
the vector information corresponding to a single instance, i.e., the
class label and the features. Such a line begins with the class label
(a number from 1 to 31), and then a sequence of feature id and value
pairs, space separated, of the instance follows. A colon ":" separated
the feature id from the value. If the value is 0, the id-value pair
may not appear (hence sparse).  All the features in a file belong to a
single feature type (eg histogram of audio SAI boxes).  The comment
line immediately preceding the instance vector line, which begins with
'#I', contains the id of the instance.

An example is shown below, where the class label for instance 312 is
5, and feature id 4 has value 0.001, feature 15 has 0.305, and so on:

.
...
#I 312
5 4:0.001  15:0.305  ...
#I 313
9 21:0.5  53:0.9  ...
...
.
.

**The ids of the instances should be used for correspondence across
> different files to collect the different features of the same
> instance, e.g. for early or late fusion.**

**Ids for training instances begin at 1 (97935 instances in training),
> for validation instances at id 201k (12177 instances in validation),
> and for test instances at id 301k (11931 instances in test).**

**If features for a video are not available for a given feature family
> (e.g., tag features missing can mean no tags were entered for the
> video by the uploader of the video), then the feature vector for the
> instance does not appear in the corresponding feature
> file. Therefore, many files contain fewer instance vectors than the
> maximum possible.**

####  ####

The Feature Families.

**The identity of the features, such as the terms in the vocabulary,
> is not revealed.**

**The 13 feature families are:**

text\_game\_lda\_1000
text\_description\_unigrams
text\_tag\_unigrams

audio\_mfcc
audio\_sai\_boxes
audio\_sai\_intervalgrams
audio\_spectrogram\_stream
audio\_volume\_stream

vision\_cuboids\_histogram
vision\_hist\_motion\_estimate
vision\_hog\_features
vision\_hs\_hist\_stream
vision\_misc

---

Feature Descriptions:

1. Text. For the text features a vocabulary of unigrams was used.

text\_game\_lda\_1000.  Latent Dirichlet allocation (LDA) was run on all of
description, title, and tags (one bag of words per video).  A 1000 dimensional
LDA topic-model was built on videos that users uploaded into the gaming category
of YouTube.

NOTE: since the training data was obtained with a title match, and the classes
are popular (frequently occurring) games, the text\_game\_lda\_1000 performs best
(high 90s in Max F1) on this data (with tags features a close second). More
generally, the text features are very semantic (predictive), compared to visual
and audio features, and perform best out of the three families, on this
dataset.

text\_description\_unigrams are the features (unigrams) extracted from the
description. The description can contain web links.

text\_tag\_unigrams are the features extracted from video tags.  Many
videos have tags that the uploader of the video entered for the video
at the time of upload.

A small fraction of the instances miss tag and/or description features.


---


2. Audio features.

For a description of audio features, please see:

@article{lyonEtAl2010sound,
> title={Sound retrieval and ranking using sparse auditory representations},
> author={R.~F.~Lyon and M.~Rehn and S.~Bengio and T.~C.~Walters and G.~Chechik},
> journal={Neural computation},
> volume={22},
> number={9},
> pages={2390--2416},
> year={2010},
> publisher={MIT Press}
}

@inproceedings{waltersEtAl2012,
> title={The intervalgram: An audio feature for large-scale melody recognition},
> author={Thomas C.~Walters and David A.~Ross and Richard F.~Lyon},
> booktitle={Proc. of the 9th International Symposium on Computer Music Modeling and Retrieval (CMMR)},
> year={2012}
}


Note that several of the audio feature families, the files, take considerable
space (the feature vectors are long and relatively dense).


---


3. Visual features.

For a description of visual features, please see:

@InProceedings{subtags11,
> author =	{Weilong Yang and GeorgeToderici},
> title = 	 {Discriminative tag learning on youtube videos with latent sub-tags},
> booktitle = {Computer Vision and Pattern Recognition (CVPR)},
> year =     {2011},
}


@inproceedings{todericiEtAl2010CVPR,
> title={Finding meaning on youtube: Tag recommendation and category discovery},
> author={Toderici, G. and Aradhye, H. and Pasca, M. and Sbaiz, L. and Yagnik, J.},
> booktitle={Computer Vision and Pattern Recognition (CVPR)},
> pages={3447--3454},
> year={2010},
> organization={IEEE}
}



Performance on class 1.

Max F1 performance on class 1 using linear SVM (L2-regularized L2-loss primal, from the liblinear package), instances l2 normalized, trained on training data, evaluated on validation, on each feature family:

text\_game\_lda\_1000    0.97
text\_description\_unigrams	0.68
text\_tag\_unigrams			0.94

audio\_mfcc				0.28
audio\_sai\_boxes				0.58  (committee of 10 perceptrons, dataset too big for batch learning)
audio\_sai\_intervalgrams			0.23 (committee of 10 perceptrons, dataset too big for batch)
audio\_spectrogram\_stream		0.047
audio\_volume\_stream			0.046

vision\_cuboids				0.59
vision\_hogs				0.21
vision\_hist\_motion\_estimate		0.076
vision\_hs\_hist\_stream			0.22
vision\_misc				0.41


Note that the performance measures are intended merely as a
reference. The choice of regularization parameter and algorithm can
make a significant difference in performance. For instance, on
vision\_hist\_motion\_estimate (which has 64 dimensions only), committee
of 10 perceptrons, each taking multiple passes over training data,
obtains around 0.11 Max F1 (linear SVM with C=1 obtains 0.076), and a
random forest classifier containing 50 trees obtains 0.16.