---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Fly through "real" trees! 
permalink: /rbe595/fall2023/proj/p2b/
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
**11:59:59 PM, Oct 09, 2023.** Live demos are due on the same day as well for which you will need to book time slots (More details on this will be posted on Piazza). 

<a name='prob'></a>
## 2. Problem Statement 
In this project, you will implement the navigation (planning and control) stack from Project 2a on a real quadrotor. The starter code and the map can be download from <a href="https://github.com/pearwpi/rbe595_p2b">here</a>. The maps and start and goal locations are inside `TrainSetP2b.zip` file. All the codes have to run on the provided NVIDIA Jetson Orin Nano (you can install whatever packages you need) and the trajectory has to be executed on the DJI Tello Edu Quadrotor. 

<a name='environment'></a>
## 3. Environment
The map is known prior-art through a map file format from Project 2a (which can change during your live demo, more details on this later). An example map file for the real experiments is given in the starter code. The map of this environment is shown in Fig. 1. The coordinate frames are also defined as shown in Fig. 1 and the start and goal locations are marked as red and green circles respectively. If the start/goal location are given as 0 in the Z axis, that means you need to takeoff/land in that location respectively. In this case, you are expected to fly at a height of 1m after takeoff and before landing. Note that you will be given a similar map file during the live demo (with same coordinate conventions). Although the boxes in Fig. 1 are not 2m in height (Z), you have to assume that it covers the entire area as given in `map1.txt`, this means that you are not allowed to fly over the boxes.


<div class="fig fighighlight">
  <img src="https://umdausfire.github.io/img/fire198/asn5/Frames.png" width="100%">
  <div class="figcaption">
    Fig 1: Sample environment for testing in real world given in map1.txt. Left: Panoramic view of the environment, Right: Perspective view of the environment with coordinate axes. Red and green circles show start and goal locations respectively.  
  </div>
  <div style="clear:both;"></div>
</div>


<a name='implementation'></a>
## 4. Implementation
You will be using the DJI Tello Edu Quadrotor for the experiments (See Figs. 2 and 3). The goal is to navigate through a known map given the oracle position (you will assume that your odometry and controls are perfect in this case). Essentially, you will be implementing a path planner, waypoint trajectory generator and a position controller. To do this, you will modify your code from Project 2a and integrate it with the <a href="https://github.com/damiafuentes/DJITelloPy">DJITelloPy</a> package or any other package that allows for control of the DJI Tello Edu using Python. You are free to perform this project either with a position controller or a velocity controller. The goal is to navigate though the scene as fast as possible. <b>Show your quadrotor pose in the map in Blender using the odometry through the run.</b>.


<div class="fig fighighlight">
  <img src="https://umdausfire.github.io/img/fire198/asn5/Tellos.png" width="100%">
  <div class="figcaption">
    Fig 2: DJI Tello Edu we'll be using in the flight experiments, Left: In action, Right: Zoomed-in view.
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/rbe595/p2/Overview.png" width="100%">
  <div class="figcaption">
    Figure 3: Army of DJI Tello Edu's and their respective NVIDIA Jetson Orin Nano computers used in experiments.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='collision'></a>
## 4.1. Collision Handling
Your quadrotor should  fly as fast as possible. However, a real quadrotor is not allowed to collide with anything (<a href="https://www.youtube.com/watch?v=TVrxvqYlCDs">video</a>). Therefore, we have zero tolerance towards collision - if you collide, you crash, you get zero for that test. For this part, collisions will be counted as if the free space of the robot is an open set; if you are on the boundary of a collision, you are in collision.

As you program your controller, you'll know how well it works, it will have overshoots. Additionally, trajectory smoothing may also deviate the actual trajectory from the planned path. Therefore, you should make good use of the margin parameter and set your speed carefully. Please be aware that the robot is assumed to be a cuboid with a tall vertical height. You should make sure that no part of the robot collides with any obstacles. Furthermore, one practical tip is to have relatively large margins for obstacles such that odometry errors (your oracle state is coming from the odometry of the robot) are not large enough for the robot to crash into obstacles.


<a name='testset'></a>
## 5. Testing (Live Demo)
On the day of the deadline, each team will be given a 15 minute slot for demoing their code in action to the instructors in the given map file, start and goal locations. The instructors will place the obstacles as they wish (obstacle locations will be given to you in the map). The task is the fly to the goal location as fast as possible without any collisions. You can get as many attempts as you want to accomplish this within your 15 minute time slot. All the codes have to run on the provided NVIDIA Jetson Orin Nano (you can install whatever packages you need) and the trajectory has to be executed on the DJI Tello Edu Quadrotor. 


<a name='sub'></a>

## 6. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 6.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p2b.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p2b.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p2b/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p2b.zip
├── Code
|   ├── Wrapper.py
|   └── Any subfolders you want along with files
├── Report.pdf
├── RunVideo.mp4
├── VisVideo.mp4
└── README.md
```

<a name='report'></a>

### 6.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Link to a ghosted photo?  

<a name='video'></a>

### 6.3. Video

Record your successful run in `.mp4` format during your demo or before and submit it in the zip file,. Make sure to name it as `RunVideo.mp4`. Record the video in the highest resolution and fastest fps possible on your recording device with minimum being 1080p at 30fps. Record the video horizontally and such that the nets do not cover the frame. Use a super steady hand or a tripod for good quality shots. 

Record the visualization of your run, where the quadrotor will navigate through the scene. Use odometry from the Tello as a position source on your map. Name this video as `VisVideo.mp4` and keep the resolution atleast 1080p at 30fps. Show the RRT* tree, planned path and trajectory as in Project 2a. 

<a name='funcs'></a>

## 7. Allowed and Disallowed functions

<b> Allowed:</b>

- Any functions regarding reading, writing and displaying/plotting images in `cv2`, `matplotlib`
- Basic math utilities including convolution operations in `numpy` and `math`
- Any functions for pretty plots and visualizations
- Any assets for visualizations
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

<a name='ack'></a>

## 9. Acknowledgements

This fun project is inspired by <a href="https://prg.cs.umd.edu/enae788m">ENAE788M: Hands-On Autonomous Aerial Robotics</a> at the University of Maryland, College Park and <a href="https://alliance.seas.upenn.edu/~meam620/wiki/index.php?n=Main.Spring2015">MEAM620: Advanced Robotics</a> at the University of Pennsylvania. 
