---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: The Final Race!
permalink: /rbe595/fall2023/proj/p5/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Environment](#environment)
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
**11:59:59 PM, Nov 20, 2023.**

**YOU ARE NOT ALLOWED TO USE ANY LATE DAYS FOR THIS PROJECT!**

TODO: Add real experiment photos.

<a name='prob'></a>
## 2. Problem Statement 
Congratulations for making this far into the course. We know that you've worked very hard to get here and learnt a lot of new concepts along the way. Now, it's time to put everything together. The aim of this project is to win the race on an obstacle course which utilizes all the algorithms you built from projects 1 through 4.

TODO: Add image of gap

Remember that your DJI Tello Edu comes with a suite of sensors, a front facing RGB color camera, a down facing grayscale camera, an IMU along with the altimeter on-board. You can obtain the camera feed (only one camera works at a time), attitude estimates and altimeter measurements from the <a href="https://djitellopy.readthedocs.io/en/latest/tello/">`DJITelloPy`</a> package. You are **ALLOWED** to use any package to access any other information on the quadrotor. You can use any/all of the sensors to complete the course as quickly as you can. Also, the structure of the track (obstacle course) is known before along with a prior on pose of the obstacles as a uniform distribution, similar to previous projects. An oblique overview of the track is shown in Fig. 1.

TODO: Add real photos of the track



<a name='environment'></a>
## 3. Environment
The final track has **four** stages. Description of each stage is given next. As in the previous projects, you are given approximate locations of all the obstacles on the track in the environment file.

<a name='racingwindows'></a>
## 3.1. Racing Windows

In the first stage, you will take off from the helipad and you have to navigate though a set of three drone racing windows from project 3 as fast as possible without any collisions. The description of the windows are the same as in project 3.

<a name='unknwonwindow'></a>
## 3.2. Unknown Window

In the second stage, you have to navigate though an unknown shaped window from project 3 as fast as possible without any collisions. The description of the window is the same as in project 4.

<a name='dynamicwindow'></a>
## 3.3. Dynamic Window

In the third/penultimate stage, you have to navigate though an "orange" square window with a clock like hand rotating in the middle. The rotation speed of the hand is fixed but is unknown. You have to fly through this dynamic window as fast as possible without any collisions. The dimensions of the window are shown in Fig. ZZ.


<a name='flyback'></a>
## 3.4. Fly back

Once you have passed through the three stages, you have to fly back through the second racing window to end your attempt. 

<a name='attemptterm'></a>
## 4. Attempt Termination

Doing any of the following will instantly terminate your attempt:
- Crossing the yellow line on the floor.
- Flying over a height of 2.5 m.
- Failing to navigate in any of the previous stages before proceeding to the next one.
- Crashing into any of the obstacles/track objects and/or the nets.
- Landing due to battery failsafe.
- Going over the maximum time of 2 mins per attempt.

<a name='scoring'></a>
## 5. Scoring Criterion
Taking off from the helipad gives the team 10 points, then the stages 1 through 4 (finish) have the following point splits: 20, 20, 30, 20, totalling a maximum of 100 points for each attempt. The team's attempt will be terminated if any of the things mentioned in <a href="#attemptterm">Section 4</a> happen. If the number of stages between two teams are tied, then the team with the lower time comes out on top.


<a name='dday'></a>
## 6. D-Day of the Competition

On the day of the competition, the teams will go in the order of their team number. Each team will have a maximum time of 2 minutes per attempt and a maximum time of 15 minutes for all attempts combined. Between attempts, the team can use any amount of time (within the alloted 15 minutes of maximum time) to fix any software/hardware bugs or do changes in hardware/software (including change of batteries). Only the best trial will be graded.

A sample photo of the real track is shown in Fig. ZZ.


Exactly 2 hours before the demo, each team can go and check the track, collect any data you will need. You will also be given the environment file at this time. No one will be allowed to enter the track 1 hour before the demo. The instructors will displace each obstacle randomly with a maximum disturbance as given in the environment file.

The team with the highest points will win. Note that, completing the course (within the 2 minute slot per attempt) will get that team the maximum of 100 points.

The teams can place the quadrotor on the helipad in any desired orientation. Also, if a team wants to improve their live-demo score, they can request for an additional slot after all the teams have finished their demo with a penalty of 20 points of the total project 5 grade.


<a name='implementation'></a>
## 7. Implementation

This project is totally open! You can use any open-source code available online to solve any part of the problem. Make sure you CITE them. You are expected to use the implementations from the previous four projects but are NOT limited to it. You are NOT allowed to modify the DJI Tello Edu or the NVIDIA Jetson Orin Nano platform. This includes adding/subtracting any sensors. All your codes **MUST** run on the NVIDIA Jetson Orin Nano without any cloud computation. Your results are ONLY limited by your imagination and creativity!


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
On the day of the deadline, each team will be given a 15 minute slot for demoing their code in action to the instructors. The instructors will place the window as they wish (approximate window pose will be given to you). The task is the fly through the an unknown gap as fast as possible without any collisions. You can get as many attempts as you want to accomplish this within your 15 minute time slot. Your final grade for the demo is decided based on completion (successful flight through a window) and time. 


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

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE5959-F02-ST Fall 2023 website](https://nitinjsanket.github.io/teaching/rbe595/fall2023.html).

