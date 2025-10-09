---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Fly through boxes! 
permalink: /rbe595/fall2025/proj/p2b/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Environment](#environment)
- [4. Implementation](#implementation)
  - [4.1. Collision Handling](#collision)
- [5. Submission Guidelines](#sub)
  - [5.1. File tree and naming](#files)
  - [5.2. Report](#report)
  - [5.3. Video](#video)
- [6. Allowed and Disallowed functions](#funcs)
- [7. Collaboration Policy](#coll)
- [8. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, Nov 01, 2025.**  

<a name='prob'></a>
## 2. Problem Statement 
In this project, you will implement the navigation (planning and control) stack from Project 2a on a simulated VizFlyt quadrotor. The starter code and the map can be download from <a href="https://app.box.com/s/khzufw1i2awjcyenf6aj9mmppzbjoyb9">here</a>. The map is inside `mapSplat.txt` file and start and goal locations are already loaded in the environment file.

<a name='environment'></a>
## 3. Environment
The map is known prior-art through a map file format from Project 2a. An example map file for the experiment is given in the starter code. The map of this environment is shown in Fig. 1. The coordinate frames are also defined as shown in Fig. 1 and the start and goal locations are marked as red and green circles respectively. If the start/goal location are given as 0 in the Z axis, that means you need to takeoff/land in that location respectively. In this case, you are expected to fly at a height of 0.5 (scaled units of Splat Frame) after takeoff and before landing. Although the boxes in Fig. 1 are not 1 unit in height (Z), you have to assume that it covers the entire area as given in `map1.txt`, this means that you are not allowed to fly over the boxes.


<div class="fig fighighlight">
  <img src="/assets/2025/rbe595/p2/Frames.png" width="100%">
  <div class="figcaption">
    Fig 1: Sample environment for testing in VizFlyt world given in `mapSplat.txt`. Perspective view of the environment with coordinate axes. Red and green circles show start and goal locations respectively, dashed circle show being the obstacle.  
  </div>
  <div style="clear:both;"></div>
</div>

<a name='implementation'></a>
## 4. Implementation
You will be using a VizFlyt simulated Quadrotor for the experiments. The goal is to navigate through a known map given the oracle position (you will assume that your odometry and controls are perfect in this case). Essentially, you will be implementing a path planner, waypoint trajectory generator and a position controller. To do this, you will modify your code from Project 2a and integrate it with VizFlyt package (more details on Turning setup are given in `install.md` file in the starter package and WPI's Turning documentation can be found <a href="https://docs.turing.wpi.edu/">here</a>). You are free to perform this project either with a position controller or a velocity controller. The goal is to navigate though the scene as fast as possible. <b>Show your quadrotor pose in the map in matplotlib through the run.</b>.

A video tutorial on Turning access by your TA Deepak is shown below and can be downloaded from <a href="https://app.box.com/s/a3iel0zc7dwfvmsh28qkinhd1txp0chd">here</a>.

<div class="fig fighighlight">
  <iframe src="https://app.box.com/embed/s/a3iel0zc7dwfvmsh28qkinhd1txp0chd?sortColumn=date" width="330" height="400" frameborder="0" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>
  <div class="figcaption">
    Fig 2: Video tutorial of Turing Access. 
  </div>
  <div style="clear:both;"></div>
</div>


<a name='collision'></a>
## 4.1. Collision Handling
Your quadrotor should  fly as fast as possible. However, a real quadrotor is not allowed to collide with anything (<a href="https://www.youtube.com/watch?v=TVrxvqYlCDs">video</a>). Therefore, we have zero tolerance towards collision - if you collide, you crash, you get zero for that test. For this part, collisions will be counted as if the free space of the robot is an open set; if you are on the boundary of a collision, you are in collision.

As you program your controller, you'll know how well it works, it will have overshoots. Additionally, trajectory smoothing may also deviate the actual trajectory from the planned path. Therefore, you should make good use of the margin parameter and set your speed carefully. Please be aware that the robot is assumed to be a cuboid with a tall vertical height. You should make sure that no part of the robot collides with any obstacles. 


<a name='sub'></a>

## 5. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 5.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p2b.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p2b.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p2b/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p2b.zip
├── maps
|   └── mapSplat.txt
├── p2phaseb_colmap
|   └── *
├── p2phaseb_colmap_splat
|   └── *
├── main.py
├── control.py
├── environment.py
├── path_planner.py
├── quad_dynamics.py
├── simulator.py
├── tello.py
├── trajectory_generator.py
├── install.md
├── Other files and folders
|   └── Any subfolders you want along with 
├── Report.pdf
├── Video.mp4
└── README.md
```

<a name='report'></a>

### 5.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.

<a name='video'></a>

### 5.3. Video

Record the splat view (robot camera seeing the front when it executes the trajectory) in VizFlyt. Also include side by side your plot of the robot pose with obstacles in matplotlib. Make sure the video is in `.mp4` format and submit it in the zip file and name the video as `Video.mp4`.  Show the RRT* tree, planned path and trajectory as in Project 2a in the matplotlib plot. 

<a name='funcs'></a>

## 6. Allowed and Disallowed functions

<b> Allowed:</b>

- Any functions regarding reading, writing and displaying/plotting images in `cv2`, `matplotlib`
- Basic math utilities including convolution operations in `numpy` and `math`
- Any functions for pretty plots and visualizations
- Any assets for visualizations
- Quaternion libraries
- Any library that perform transformation between various representations of attitude
- Any code for alignment of timestamps


<b> Disallowed:</b>
- All functions disallowed in P2a

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2025/rbe595). 

<a name='coll'></a>

## 7. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE595-F02-ST Fall 2025 website](https://pear.wpi.edu/teaching/rbe595/fall2025.html).

<a name='ack'></a>

## 8. Acknowledgements

This fun project is inspired by <a href="https://prg.cs.umd.edu/enae788m">ENAE788M: Hands-On Autonomous Aerial Robotics</a> at the University of Maryland, College Park and <a href="https://alliance.seas.upenn.edu/~meam620/wiki/index.php?n=Main.Spring2015">MEAM620: Advanced Robotics</a> at the University of Pennsylvania. 
