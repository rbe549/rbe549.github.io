---
layout: page
mathjax: true
title: SexySemanticMapping
permalink: /fall2022/hw/hw2/
---

Table of Contents:
- [1. Due Date](#due)
- [2. Introduction](#intro)
- [3. Data](#data)
- [4. Sexy Semantic Mapping](#sexysemanticmapping)
  - [4.1. Building The Map](#mapbuilding)
  - [4.2. Semantic Point Painting The Map](#semanticpainting)
- [5. Submission Guidelines](#sub)
  - [5.1. File tree and naming](#files)
  - [5.2. Report](#report)
  - [5.3. Mapping Video](#video)
- [6. Collaboration Policy](#coll)
- [7. Acknowledgements](#ack)

<a name='due'></a>
## 1. Due Date 
**11:59:59 PM, December 16, 2022. This homework is to be done individually.**

<a name='intro'></a>
## 2. Introduction

Self driving cars often use LIDARs as a central depth sensing modality. But often during mapping or localization, higher level semantic information is desired. One such cases is when parked cars drastically reduce what the car sees in-terms of features, thereby making it hard for localization or mapping. You would end up mapping the cars instead of buildings. These are called pseudo-dynamic obstacles. Furthermore, moving cars present a big challenge, making it hard for LIDAR based Simultaneous Localization And Mapping (SLAM) systems to reject these objects from becoming mapping artifacts. To exeaccerbate the situation further, current LIDAR sensors have very low vertical resolution, making it hard to detect symantics (labels of what the objects are). To this end, experts in the fild of robotics and computer vision decided to combine LIDARs with cameras to obtain a semantic painted point cloud (See Fig. 1). 



<div class="fig fighighlight">
  <img src="/assets/2019/hw2/semanticpc.jpg" width="100%">
  <div class="figcaption">
    Figure 1: Semantic Painted Point-cloud using LIDAR and camera data.
  </div>
  <div style="clear:both;"></div>
</div>


Your goal is to build a map from raw LIDAR point cloud and then transfer the predicted semantic labels from the camera image onto the LIDAR point cloud. We will talk about these steps next.

<a name='data'></a>
## 3. Data
Download any data sequence from <a href="https://www.cvlibs.net/datasets/kitti-360/index.php">KITTI-360 dataset</a> (you will have to create a free account for this). Please **DO NOT** download the entire dataset. You are **ONLY** allowed to use the raw LIDAR point clouds (or scans), RGB Images and sensor intrinsics and extrinsics. You are **NOT ALLOWED** to utilize the ground truth annotations of semantics or poses for this homework! We'll talk about the goal of the homework next.


<a name='sexysemanticmapping'></a>
## 4. Sexy Semantic Mapping
This homework involves two key steps which are described in the subsequent subsections.

<a name='mapbuilding'></a>

## 4.1. Building The Map
The first step involves building a high-definition map, this generally involves all the concepts you have seen from project 3 applied to LIDAR point cloud data. However, to make life simpler, we will work on a toy version. You are required to **UTILIZE** Yes! You read it right, I said utilize, that means you are **ALLOWED** to use any third party code for this part (specific things which you are not allowed will be discussed next). Feel free to utilize any variant of ICP to build your map such as the classical Point-To-Point ICP or Point-To-Plane ICP or Doppler ICP or Semantic ICP, you are free to choose your favorite ICP flavor. The only constraint is that you are **NOT ALLOWED** to utilize any deep learning ICP method here. A sample map is shown in Fig. 2 (Note that your ICP map will not be as good as this). Do **NOT** forget to check the license of the code you are using and cite them. Once the map is built, we need to color it (as seen in Fig. 1).


<div class="fig fighighlight">
  <img src="/assets/2019/hw2/lidarpc.png" width="100%">
  <div class="figcaption">
    Figure 2: LIDAR based mapping.
  </div>
  <div style="clear:both;"></div>
</div>

<a name='semanticpainting'></a>

## 4.2. Semantic Point Painting The Map
Now, we are going to utilize the RGB images to obtain semantic information. **UTILIZE** any semantic segmentation neural network to predict the semeantic labels on every image frame. Now, utilize this information to transfer the semantic color labels to the generated map. You need to use the extrinsics between the sensors to transfer the labels from RGB image to LIDAR point cloud. You are **NOT ALLOWED** to use any third party function for this operation. To check your transformation operation you can transfer the RGB colors to see if they make sense, such as a red car should be red in the point cloud and the colors should not bleend much into other parts (you'll have some error due to imperfect semantic predictions, do not worry about it), See Fig. 3 for an example of how this would look like.



<div class="fig fighighlight">
  <img src="/assets/2019/hw2/lidarpccolor.png" width="100%">
  <div class="figcaption">
    Figure 3: LIDAR map colored with RGB image data.
  </div>
  <div style="clear:both;"></div>
</div>

Finally, once you have ensured that your extrinsics are correctly utilized, you can transfer the seamntic labels/colors onto the LIDAR ICP map and your result should conceptually look like Fig. 4 (do not worry if it does not look as good).

<div class="fig fighighlight">
  <img src="/assets/2019/hw2/lidarpccolorsemantics.png" width="100%">
  <div class="figcaption">
    Figure 4: LIDAR map colored with semantics from the RGB image data.
  </div>
  <div style="clear:both;"></div>
</div>

<a name='sub'></a>
## 5. Submission Guidelines

<b> If your submission does not comply with the following guidelines, you'll be given ZERO credit. </b>

<a name='files'></a>
### 5.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_hw2.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``. For our example, the submission file should be named ``abc_hw2.zip``. The file **must have the following directory structure** because we'll be autograding assignments. The file to run for your project should be called ``Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. Please **DO NOT** include data in your submission.

```
YourDirectoryID_hw1.zip
│   README.md
|   Your Code files 
|		├── Any subfolders you want along with files
|   Wrapper.py 
|   SemanticMapping.mp4 
└──	Report.pdf
```
<a name='report'></a>
### 5.2. Report

For each section of the homework, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented.  You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in previous assignments.
- Talk in detail about the ICP approach you utilized along with the mathematical formulation.
- Talk about the semantic segmentation network you utilized, along with network details and training dataset details.
- Clearly, write down the equations you utilized to transfer the RGB semantic labels onto the LIDAR point cloud.


<a name='video'></a>

### 5.3. Mapping Video

You are required to make a visualization that shows your semantic mapping in proess along with RGB images and 3D point cloud and generate an ``.mp4`` video called ``SemanticMapping.mp4``. <a href="https://www.youtube.com/watch?v=C6Y92YfUkQU">Here's an example</a> of how the video can look like. You are free to **USE ANY** visualization toolbox or code for generating this video. 


<a name='coll'></a>
## 6. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE/CS549 Spring 2022 website](https://nitinjsanket.github.io/teaching/rbe549/fall2022.html).

<a name='ack'></a>
## 7. Acknowledgements
This super fun homework was suggested by <a href="http://naitri.github.io/">Naitri Rajyaguru</a> from the University of Maryland, College Park. 