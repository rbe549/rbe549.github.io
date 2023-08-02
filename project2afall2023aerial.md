---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Tree Planning Through The Trees! 
permalink: /rbe595/fall2023/proj/p2a/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Environment](#environment)
- [4. Implementation](#implementation)
  - [4.1. Map](#map)
  - [4.2. Path Planner](#pathplanner)
  - [4.3. Trajectory Generation](#trajgen)
    -[4.3.1. Collision Handling](#collision)
  - [4.4. Controller](#controller)
- [5. Notes About Test Set](#testset)
- [6. Submission Guidelines](#sub)
  - [6.1. File tree and naming](#files)
  - [6.2. Report](#report)
- [7. Allowed and Disallowed functions](#funcs)
- [8. Collaboration Policy](#coll)
- [9. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, September 29, 2023.**


TODO: Add a block diagram of the entire project

<a name='prob'></a>
## 2. Problem Statement 
In this project, you will implement a path and trajectory planning, and a control stack for a quadrotor to navigate from a start position to a goal position through a pre-mapped or known 3D environment. In the first part of the project 2 AKA project 2a, this will be done in simulation. 

<a name='environment'></a>
## 3. Environment
The simulation environment is based on Blender which you worked on during HW0.  Please download the environment and the included starter code from <a href="">here</a>. The map and the start and goal positions have to be loaded dynamically as described in a text file (more on this in the following sections). 

<a name='implementation'></a>
## 4. Implementation
We will talk about each part in the following sub-sections. We'll describe the map file first.

<a name='map'></a>
## 4.1. Map
An example of the environment file format is given below:

```
# An example environment
# boundary xmin ymin zmin xmax ymax zmax
# block xmin ymin zmin xmax ymax zmax r g b
boundary 0 0 0 45 35 6
block 1.0 1.0 1.0 3.0 3.0 3.0 0 255 0
block 20.0 10.0 0.0 21.0 20.0 6.0 0 0 255
```
- The format uses # for comments.
- The rectangular boundary extents for this environment are `(x0, y0, z0) = (0, 0, 0)` (lower left) and `(x1, y1, z1) = (45, 35, 6)` (upper right).
- A block line also specifies the rectangular boundary, and the blocks' color as a RGB triple with values from 0-255. All values are whitespace  delimited and all numerical values are represented as a float.
- All distances (values provided) are in meters.
- Blocks can overlap.

There will be sample files provided to you for testing. This will be available in a folder called `sample_maps`.

To make life simple, we have made the obstacles cuboidal. In reality, obstacles can be of any shape and size, hence you can think of these rectangular obstacles as the bounding volume of arbitrarily shaped obstacles something that you might obtain from a low resolution occupancy grid mapping algorithm. 

Write a simple function to read and display this map in Blender. 

<a name='pathplanner'></a>
## 4.2. Path Planner
In this case, you'll write an RRT* global path planner function that takes as input the start position, goal position and the map path from the previous step (You will run your reading and displaying map function from inside the path planner). You are free to use any distance metric in this case. You have complete oracle information (noise free) of the global state (position, velocity, acceleration, orientation and angular velocity) of your robot with respect to the given map. A general rule of thumb is to bloat the obstacles by the size of the robot to avoid collisions.

<a name='trajgen'></a>
## 4.3. Trajectory Generation
The path generated in the previous step is not smooth and dynamically feasible as the paths often include sharp turns where the robot will overshoot significantly. In this step, you will convert the waypoints (path) obtained from the previous step into a quintic spline (5th order polynomial). Note that you will need to decide on a velocity profile to turn the path from RRT* into a trajectory. The speed of the robot does not need to be a constant and is your choice (you can vary it based on the local curvature for example, be creative here).  

Write a function that takes as input a path from RRT* and converts it into a trajectory as a function of time. You should pre-compute as many quantities as possible and avoid fitting polynomial curves in every call of the function. 

<a name='collision'></a>
## 4.3.1. Collision Handling
Your quadrotor should  fly as fast as possible. However, a real quadrotor is not allowed to collide with anything (<a href="https://www.youtube.com/watch?v=TVrxvqYlCDs">video</a>). Therefore, we have zero tolerance towards collision - if you
collide, you crash, you get zero for that test. For this part, collisions will be counted as if the free space of the robot is an open set; if you are on the boundary of a collision, you are in collision.

As you program your controller, you'll know how well it works, it will have overshoots. Additionally, trajectory smoothing may also deviate the actual trajectory from the planned path. Therefore, you should make good use of the margin parameter and set your speed carefully. Please be aware that the robot is assumed to be a cuboid with a tall vertical height. You should make sure that no part of the robot collides with any obstacles. However, we guarantee that for all the testing maps (you'll read about this in the test set section), with the specified start and goal locations, there will always be openings that allow cuboid with a width and length of 0.3 m and height of 0.5 m to pass through.

<a name='controller'></a>
## 4.4. Controller
You will then implement a controller function that makes sure that your quadrotor follows the desired trajectory. The controller takes as input the current state, current time, ADD DETAILS HERE.


<!-- - Global and local based on noise! -->


<a name='testset'></a>
## 5. Notes About Test Set
A test set will be released 24 hours before the deadline. You can download the test set from <b>here</b>. Your report MUST include the output from both the train and test sets. 


<a name='sub'></a>

## 6. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 6.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p2a.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p2a.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p2a/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>20MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p2a.zip
├── Code
|   ├── Wrapper.py
|   └── Any subfolders you want along with files
├── Report.pdf
└── README.md
```

<a name='report'></a>

### 6.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Link to the `rotplot` videos comparing attitude estimation using Madgwick Filter from Project 1a, UKF and Vicon. Sample video can be seen [here](https://www.youtube.com/watch?feature=player_embedded&v=iCe3o-9moUM).
- Plots for all the train and test sets. In each plot have the angles estimated from madgwick filter, UKF and Vicon along with proper legend.  
- A sample report for a similar project is given in the `.zip` file given to you with the name `SampleReport.pdf`. Treat this report as the benchmark or gold standard which we'll compare your reports to for grading.


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

<a name='ack'></a>

## 9. Acknowledgements

This fun project is inspired by <a href="https://prg.cs.umd.edu/enae788m">ENAE788M: Hands-On Autonomous Aerial Robotics</a> at the University of Maryland, College Park and <a href="https://alliance.seas.upenn.edu/~meam620/wiki/index.php?n=Main.Spring2015">MEAM620: Advanced Robotics</a> at the University of Pennsylvania. 
