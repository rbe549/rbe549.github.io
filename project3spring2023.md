---
layout: page
mathjax: true
title: EinsteinVision
permalink: /spring2023/proj/p3/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Data Collection](#data)
- [4. Chechkpoint 1: Traditional Approach](#ph1)
	- [4.1. Corner Detection](#corner)
	- [4.2. Adaptive Non-Maximal Suppression (ANMS)](#anms)
	- [4.3. Feature Descriptor](#feat-descriptor)
	- [4.4. Feature Matching](#feat-matching)
	- [4.5. RANSAC for outlier rejection and to estimate Robust Homography](#homography)
	- [4.6. Blending Images](#blending)
- [5. Phase 2: Deep Learning Approach](#ph2)
	- [5.1. Data Generation](#datagen)
	- [5.2. Supervised Approach](#ph2sup)
	- [5.3. Unsupervised Approach](#ph2unsup)
- [6. Notes about Test Set](#testset)
- [7. Extra Credit](#extra)
- [8. Submission Guidelines](#sub)
  - [8.1. Starter Code](#starter)
  - [8.2. File tree and naming](#files)
  - [8.3. Report](#report)
- [9. Allowed and Disallowed functions](#funcs)
- [10. Extra Credit](#extracredit)
- [11. Collaboration Policy](#coll)
- [12. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, April 02, 2023.** This project is to be done in groups of 2 and has a 10 min presentation. Various Checkpoints are due at different dates.

<a name='prob'></a>
## 2. Problem Statement 
So far we have been working with calibrated cameras, stitched images, reconstructed an entire scene using classical methods and even did a fancy reconstruction using NeRF. It's time to take it up a notch and build beautiful visualizations which takes in sensory information and provide insights into how robots are thinking. Visualizations are not only important for debugging the issues in the software but they are also a basic human robot interaction (HRI) problem. A classic such visualization is the infamous `rviz` as shown in Fig. 1.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/rviz.png" width="100%">
  <div class="figcaption">
    Fig 1: RViz Visualization of sensor data on a self-driving car.
  </div>
  <div style="clear:both;"></div>
</div>


Another example of this how it can look is given in Fig. 2.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavisionrudimentary.png" width="100%">
  <div class="figcaption">
    Fig 2: Tesla's backend visulization in autonomous mode.
  </div>
  <div style="clear:both;"></div>
</div>


Both rviz and tesla's earlier dashboards fail at HRI problem, rendering (pun intended) it useless for common usage.
 
Tesla's recent dashboard (Fig. 3) is a really good example of visualization which not only provides a lot of insights about information it perceives, it is intuitive and interactive. 

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavision.png" width="100%">
  <div class="figcaption">
    Fig 2: Tesla's frontend visulization in autonomous mode.
  </div>
  <div style="clear:both;"></div>
</div>


So in this project you will build an equivalent of Tesla's dashboard wherein you are provided with a set of videos recorded from cameras of a Tesla and the output will be a video of how you would visualize the data. You are free to use any network models available in the wild and render the visualizations using blender.  It's an open ended problem and so there is no "right solution. Consequently, you are graded for your creativity in approaching the problem and the effectiveness and prettiness of the visualizations. 


<a name='data'></a>
## 3. Dataset


<a name='ckpt1'></a>
## 4. Checkpoint 1: Basic Features
In this checkpoint, you are required to implement basic features which are absolutely essential. 


1. **Lanes:** Identify and show the lanes on the road, they could be dashed, solid and different color (white and yellow). Each lane has significance and essential to identify.
2. **Vehicles:** In this checkpoint you just need to identify the vehicle, and represent it as some blob/rectangle etc. As long you can cluster them and categorize that it’s a vehicle its fine.
3. **Pedestrians:**  like earlier, you need identify and locate pedestrians in the scene.
4. **Traffic lights:** Indicate the traffic signals and it’s color.
5. **Road signs:** There are sign boards on the road you need identify and represent, need to primarily indicate stop and speed limit signs
  
All of the above are essential when you are designing your autonomous vehicle’s vision system. 

<a name='ckpt2'></a>
## 5. Checkpoint 2:  Advanced Features
In this checkpoint, you will build on top of the previous one by enhancing/adding more features.

1. **Vehicles:** unlike earlier checkpoint, you need to classify and subclassify different vehicles. More particularly, 
  - Cars: Sedan, SUV, hatchback
  - Trucks
  - Pickup trucks
  - Bicycle
  - Motorcycle
2. **Road signs:** Along with the sign boards, you should also indicate road signs on the ground.
3. **Objects:** you need to indicate objects like poles, traffic cones and traffic cylinders. 

All of the ones in this checkpoint are adding more granularity in your vision system which can aid in algorithmic decisions in navigation modules.  

<a name='ckpt3'></a>
## 6. Checkpoint 3: Bells and Whistles

1. **Break lights and indicators of the other vehicles:** By identifying other vehicle break lights and indicator signals, your navigation module can get speeds and other lane changing decisions. 
2. **Pedestrian pose:** You need to identify pedestrian pose each frame instead of just classifying them.


<a name='extra'></a>
## 7. Extra Credit : Cherry on Top
Implementing the extra credit can give you upto 25% of bonus score.

Like a cherry on top, you need to identify and indicate:

1. Speed bumps 
2. Collision prediction
3. Parking space identification

<a name='sub'></a>

## 8. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

<a name='starter'></a>

### 8.1. Starter Code
Download the Starter Code for both Phase 1 and Phase 2 from [here](https://drive.google.com/file/d/1-WBB5fJFSaQqH3MvuJrsDbnOHXcc0XCU/view?usp=sharing).

<a name='files'></a>

### 8.2. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p1.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``. For our example, the submission file should be named ``abc_p1.zip``. The file **must have the following directory structure** because we might be autograding assignments. The file to run for your project should be called ``YourDirectoryID_p1/Phase1/Code/Wrapper.py`` for Phase 1; ``YourDirectoryID_p1/Phase2/Code/Train.py`` and ``YourDirectoryID_p1/Phase2/Code/Test.py`` for running training and testing models in Phase 2 (the supervised or unsupervised approach will be chosen by the command line flag `--ModelType` from our autograder). For panorama stitching using deep learning based homography estimation, fill your code in ``YourDirectoryID_p1/Phase2/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>100MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p1.zip
|   Phase1 
|   ├── Code
|   |   ├── Wrapper.py
|   |   └── Any subfolders you want along with files
|   Phase2
|   ├── Code
|   |   ├── Train.py
|   |   ├── Test.py
|   |   ├── Wrapper.py
|   |   └── Any subfolders you want along with files
├── Report.pdf
└── README.md

```

<a name='report'></a>

### 8.3. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper.

<b> Phase 1</b>
- For Phase 1, present input and output images after each section (corner detection, ANMS, feature extraction, feature matching, feature matches after RANSAC) and final panorama for all the Train and Test Images, including the data you collected. The Train images can be found in ``YourDirectoryID_p1\Phase1\Data\Train\`` and the Test set will be released 24 hours before the deadline. Add all the images into your `Report.pdf`.
- Talk about what you did to blend the common region between images.
- Talk about how you rejected images with no/very less overlap.

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Test images will be released 24 hours before the deadline.
</p>

<b> Phase 2</b>
- Present classical feature based (from Phase 1), supervised, unsupervised estimated homographies against a synthetic ground truth for any 4 images from Train/Val/Test set. A sample image is shown below.
<div class="fig fighighlight">
  <img src="/assets/2019/p1/HomographyDisp.png" width="50%">
  <div class="figcaption">
    Fig. 18: Image overlayed with homography estimated by deep learning model shown in yellow and ground truth shown in red.
  </div>
  <div style="clear:both;"></div>
</div>

- Present the average EPE (average L2 error between predicted and ground truth homographies) results for both supervised and unsupervised approaches along with algorithm run-time for forward pass of the network after the graph has been initialized. Present EPE results on Train, Val and Test sets. EPE is defined as the average of \\(\vert \vert \widetilde{H_{4Pt}} - H_{4Pt} \vert \vert_2\\) for all the 4 points and all the images.
- Present the network architecture used in your report (a snapshot of the Tensorflow graph will do if the layers are named sensibly).
- Present input and output panoramas using supervised and unsupervised approaches for all the test images from Phase 1's Test set. You dont need to present outputs of Train set here.


<a name='funcs'></a>

## 9. Allowed and Disallowed functions

<b> Allowed:

- Any functions regarding reading, writing and displaying/plotting images in `cv2`, `matplotlib`
- Basic math utitlies including convolution operations in `numpy` and `math`
- `torch.nn` API for implementing network architecture
- `torchvision.transform` for data augmentation
- Any functions for pretty plots
- Any functions for filtering and implementing gaussian blur
- Any function for warping and blending for Phase 1
- `cv2.getPerspectiveTransform`
- `cv2.warpPerspective` (For more info, refer: [Geometric_Transformations](Geometric_Transformations))
- <a href="https://github.com/kornia/kornia">`Kornia`</a> package for warping images

<b> Disallowed:
- Any third party code for implementing spatial transformer network except one in `Kornia`
- Any third party code for implementing TensorDLT or a function that converts 4 point homography to $$3 \times 3$$ homography matrix (apart from functions specified)
- Any third party code for implementing architecture or augmentation
- `Keras` or any other layer API

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/spring2023/rbecs549).  


<a name='extracredit'></a>

## 10. Extra Credit
Perform creative things in your code to improve results or tackle corner cases to earn upto 10% extra credit. 


<a name='coll'></a>

## 11. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE/CS549 Spring 2023 website](https://nitinjsanket.github.io/teaching/rbe549/spring2023.html).

<a name='ack'></a>

## 12. Acknowledgements

This fun homework was inspired by a similar project in University of Maryland's <a href="http://prg.cs.umd.edu/cmsc733">CMSC733</a> (Classical and Deep Learning Approaches for Geometric Computer Vision).

***
