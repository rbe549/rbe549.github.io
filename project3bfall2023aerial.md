---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Mini Drone Race -- The Planning And Control Saga!
permalink: /rbe595/fall2023/proj/p3b/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Environment](#environment)
  - [3.1. Window Locations](#windowloc)
- [4. Implementation](#implementation)
  -[4.1. Collision Handling](#collision)
- [5. Testing (Live Demo)](#testset)
- [6. Submission Guidelines](#sub)
  - [6.1. File tree and naming](#files)
  - [6.2. Report](#report)
  - [6.3. Video](#video)
- [7. Allowed and Disallowed functions](#funcs)
- [8. Collaboration Policy](#coll)
- [9. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, Nov 07, 2023.**

TODO: Add real experiment photos.

<a name='prob'></a>
## 2. Problem Statement 
In this project, you will implement a planning and control stack and integrate it with the perception stack from Project 3a on a real quadrotor (DJI Tello Edu).

<a name='environment'></a>
## 3. Environment

TODO: Add real photos

The track is made of multiple windows with their approximate 3D positions known but unknown orientations (within a reasonable range). Now given the 3D pose of the closest window from project 3a, your goal is to navigate through the window from your current position. Note that there are no other obstacles apart from windows in the scene. More details on the algorithmic parts of the navigation stack (planning and control) are explained in the next section.  

<a name='windowloc'></a>
### 3.1. Window Locations
The same format from project 3a is used for this part.

<a name='navigationstack'></a>
## 4. Navigation Stack
You are free to use any method to plan your path from the current position to the closet window. This is relatively simple as you are given the approximate position of the window and there are no other obstacles in the scene. Hence, if you fly close to the approximate position, you'll be able to see atleast a part of the window. Then you can center the quadrotor towards the window and fly though it. In this ideology, you are generating position waypoints for your controller to follow (for which you'll use the `DJITelloPy` package). Alternatively, you can also perform visual servoing to center yourself towards the window and fly through it.  



Evaluation, Plot, Video


<div class="fig fighighlight">
  <img src="/assets/2023/rbe595/TraditionalOverview.png" width="100%">
  <div class="figcaption">
    Fig 1: Overview of panorama stitching using traditional method.
  </div>
  <div style="clear:both;"></div>
</div>

TODO: Decide window location format

<a name='testset'></a>
## 5. Testing (Live Demo) And Grading
On the day of the deadline, each team will be given a 15 minute slot for demoing their code in action to the instructors. The instructors will place the windows as they wish (approximate window locations will be given to you). The task is the fly through all windows/gates as fast as possible without any collisions. You can get as many attempts as you want to accomplish this within your 15 minute time slot. Your final grade for the demo is decided based on completion (the number of gates you fly through) and time. 


<a name='sub'></a>

## 6. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 6.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p3b.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p3b.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p2b/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p3b.zip
├── Code
|   ├── Wrapper.py
|   └── Any subfolders you want along with files
├── Report.pdf
├── Video.mp4
└── README.md
```

<a name='report'></a>

### 6.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Link to a ghosted photo?  

<a name='video'></a>

### 6.3. Video

Record your successful run in `.mp4` format during your demo or before and submit it in the zip file. Make sure to name it as `Video.mp4`. Record the video in the higest resolution and fastest fps possible on your recording device with minimum being 1080p at 30fps. Record the video horizontally and such that the nets do not cover the frame. Use a super steady hand or a tripod for good quality shots.

<a name='funcs'></a>

## 7. Allowed and Disallowed functions

<b> Allowed:</b>

- Any functions regarding reading, writing and displaying/plotting images in `cv2`, `matplotlib`
- Basic math utilities including convolution operations in `numpy` and `math`
- Any functions for pretty plots
- Quaternion libraries
- Any library that perform transformation between various representations of attitude
- Any code for alignment of timestamps


<b> Disallowed:</b>

- Any function that implements in-part or full UKF

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2023/rbe595). 

<a name='coll'></a>

## 8. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE595-F02-ST Fall 2023 website](https://pear.wpi.edu/teaching/rbe595/fall2023.html).

