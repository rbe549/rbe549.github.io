---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Tree Planning Through The Trees! 
permalink: /rbe595/fall2025/proj/p2a/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Environment](#environment)
- [4. Implementation](#implementation)
  - [4.1. Map](#map)
  - [4.2. Path Planner](#pathplanner)
  - [4.3. Trajectory Generation](#trajgen)
    - [4.3.1. Collision Handling](#collision)
  - [4.4. Controller](#controller)
- [5. Notes About Test Set](#testset)
- [6. Submission Guidelines](#sub)
  - [6.1. File tree and naming](#files)
  - [6.2. Report](#report)
  - [6.3. Video](#video)
- [7. Allowed and Disallowed functions](#funcs)
- [8. Collaboration Policy](#coll)
- [9. Group Contribution](#groupcontrib)
- [10. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, September 30, 2025.**

<a name='prob'></a>
## 2. Problem Statement 
In this project, you will implement a path and trajectory planning (motion planning), and tune a control stack for a quadrotor to navigate from a start position to a goal position through a pre-mapped or known 3D environment. In the first part of the project 2 AKA project 2a, this will be done in simulation. Fig. 1 shows a block diagram overview of the project. <br><br>

**Download the starter code and simulation environment from [here](https://app.box.com/s/e5ikimrmalpvhba09ws92c0ydlt7u7xv).**

<div class="fig fighighlight">
  <img src="/assets/2023/rbe595/p2/Overview.png" width="100%">
  <div class="figcaption">
    Figure 1: Overview of the project.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='environment'></a>
## 3. Environment
The simulation is done using Matplotlib. All visualization functions are already implemented in the starter code, which you can download from <a href="">here</a>. The map for simulation is loaded using a .txt file in the maps folder. The start and goal coordinates are defined already in the code.  Feel free to make the visualization better and prettier.


<a name='implementation'></a>
## 4. Implementation
We will talk about each part in the following sub-sections. Recall that you are implementing four things: (a) A Map/Environment reader, (b) An RRT* path planner to obtain paths from start to goal, (c) A trajectory planner to shorten the paths from RRT* output and make them (dynamically) feasible trajectories, and finally (d) A PID controller to follow the trajectory generated from previous step to navigate from start to goal without collisions. We'll describe the map file first.

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

- The format uses # for comments. This is for user reference only.
- The rectangular boundary extents for this environment are `(x0, y0, z0) = (0, 0, 0)` (lower left) and `(x1, y1, z1) = (45, 35, 6)` (upper right).
- A block line also specifies the rectangular boundary, and the blocks' color as a RGB triple with values from 0-255. All values are whitespace  delimited and all numerical values are represented as a float.
- All distances (values provided) are in meters.
- Blocks can overlap.

There will be sample files provided to you for testing. This will be available in a folder called `maps`.

To make life simple, we have made the obstacles cuboidal. In reality, obstacles can be of any shape and size, hence you can think of these rectangular obstacles as the bounding volume of arbitrarily shaped obstacles something that you might obtain from a low resolution occupancy grid mapping algorithm. 

We have given you `environment.py` file, which has `Environment3D` class for storing map information. You need to implement functions, `parse_map_file`, `is_point_in_free_space`, `is_line_collision_free` in this script. Make sure to stick to the data-types given in the class definition. There is a `safety_margin` parameter, which basically decides how close can you go to an obstacle. You can try changing this value in order to generate waypoints. Once you are done implementing these functions, you can then move ahead to path planning.



<a name='pathplanner'></a>
## 4.2. Path Planner
Now, you'll write an RRT* global path planner function that takes as input the start position, goal position and the map path from the previous step. You are free to use any distance metric in this case. You have complete oracle information (noise free) of the global state (position, velocity, acceleration, orientation and angular velocity) of your robot with respect to the given map. A general rule of thumb is to bloat the obstacles by the size of the robot to avoid collisions. You are given `path_planner.py` script for implementing this code. You need to implement functions in class `PathPlanner` to make it work. Waypoints generated by the path planner are stored in `self.waypoints`. We have given a visualizer function already to visualize the RRT* tree. You are free to change this function to make visualizations better.



<a name='trajgen'></a>
## 4.3. Trajectory Generation
The path generated in the previous step is not smooth and dynamically feasible as the paths often include sharp turns where the robot will overshoot significantly. In this step, you will convert the waypoints (path) obtained from the previous step into a cubic/quintic spline (3rd/5th order polynomial) though all the waypoints. Optionally, you might want to reduce the path lengths by joining nodes together. This is based on any logic you can come up with. But ensure that no collisions are encountered along the path (See next subsection). Note that you will need to decide on a velocity and acceleration profiles to turn the path from RRT* into a trajectory which will be given by the spline trajectory. Further, you have to decide on the time per segment and the total time the trajectory needs to take. This can be based on simple heuristics such as segment length or you can find an optimal solution using Quadratic Programming as we covered in class. 

`trajectory_generator.py` contains the class for generating a spline trajectory from the waypoints obtained from `RRT* Planner`. Implement the `generate_spline_trajectory` function in this file. The function should return `trajectory_points`, `time_points`, `velocities` and `accelerations`


To summarize, write a function that takes as input a path from RRT* and converts it into a polynomial trajectory as a function of time. You should pre-compute the spline coefficients and avoid fitting polynomial curves in every call of the function. 

<a name='collision'></a>
## 4.3.1. Collision Handling
Your quadrotor should  fly as fast as possible. However, a real quadrotor is not allowed to collide with anything (<a href="https://www.youtube.com/watch?v=TVrxvqYlCDs">video</a>). Therefore, we have zero tolerance towards collision - if you collide, you crash, you get zero for that test. For this part, collisions will be counted as if the free space of the robot is an open set; if you are on the boundary of a collision, you are in collision. Note that you have to implement your own collision checker. This is as simple as joining two points with a straight line with some discretization parameter and checking if any of the points lie inside the bloated obstacle blocks. You will use this function not only to stop your code during a collision but also to reject invalid paths in RRT* and trajectory generator. **Note that, you start collision check only after you have reached the start point.**

As you program your controller, you'll know how well it works, it will have overshoots. Additionally, trajectory smoothing may also deviate the actual trajectory from the planned path. Therefore, you should make good use of the margin parameter (you can make it bigger than the actual size of the quadrotor) and set your speed carefully (reduce speed and gains to reduce overshoots). Please be aware that the robot is assumed to be a cuboid with a tall vertical height. You should make sure that no part of the robot collides with any obstacles. 

<a name='controller'></a>
## 4.4. Controller
You will then tune a cascaded controller that makes sure that your quadrotor follows the desired trajectory as well as possible. This controller is inspired from the [PX4 Stack](https://docs.px4.io/main/en/flight_stack/controller_diagrams.html). The controller takes as input the current state, current time and desired state. In particular, you will tune the outermost position control loop which is a PID controller (3 gains per axis, hence 9 total gains) and the penultimate velocity control loop which is a PID controller (3 gains per axes, hence 9 total gains). Tune your controller on the fly, when traversing trajectories generated by `trajectory_generator.py` script. For controller, you have `control.py`, with the gains initially sert to 0. Gains can be found and tuned in `quad_control` class


<a name='testset'></a>
## 5. Notes About Test Set
 A test set will be released 24 hours before the deadline. <!-- You can download the test set from <a href="https://drive.google.com/file/d/1TaAmSDYPab69xQ3cQu9Dm82C-P3_nXol/view?usp=drive_link">here</a>.  -->
Your report MUST include the output from both the train and test sets. This will include the maps and the start and goal locations as $$(X,Y,Z)$$ tuple (you can manually feed these inputs and do not need to write a parsing function). 


<a name='sub'></a>

## 6. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 6.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p2a.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p2a.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p2a/Code/main.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>200MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p2a.zip
├── src
|   ├── maps
|   |   ├── map1.txt
|   |   └── map4.txt
|   ├── main.py
|   ├── control.py
|   ├── environment.py
|   ├── path_planner.py
|   ├── quad_dynamics.py
|   ├── simulator.py
|   ├── tello.py
|   ├── trajectory_generator.py
|   ├── Other files and folders
|   └── Any subfolders you want along with files
├── Video.mp4   
├── Report.pdf
└── README.md
```

<a name='report'></a>

### 6.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder from previous projects and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Plots of desired and actual position and velocity for all the maps in train and test sets as shown in Fig. 4.
- Show a screenshot of the explored tree for all the above cases with the path taken in the tree in red color.


<a name='video'></a>

### 6.3. Video

The video should be a screen capture of your code in action from both an oblique and top view as shown in the video below. Note that a screen capture is not a video recorded from your phone. 


<iframe src="https://app.box.com/embed/s/wvji9blw9zou8qkg42du14qe7upuj7zi?sortColumn=date" width="500" height="400" frameBorder="0" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>

<a name='funcs'></a>

## 7. Allowed and Disallowed functions

<b> Allowed:</b>

- Any functions regarding reading, writing and displaying/plotting 
- Basic math utilities 
- Any functions for pretty plots and Blender plots
- Quaternion libraries
- Any library that perform transformations between various representations of attitude
- Any code interpolation 
- Any least square solver


<b> Disallowed:</b>

- Any function that implements in-part or full RRT*
- Any function that implements in-part or full trajectory polynomial fitting or solving
- Any function that implements in-part or full controller architecture 


If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2025/rbe595). 

<a name='coll'></a>

## 8. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE595-F02-ST Fall 2025 website](https://pear.wpi.edu/teaching/rbe595/fall2025.html).

<a name='groupcontrib'></a>

## 9. Group Contribution
If you have any issues/problems with other group members' work, please send us a private Piazza post right after the submission.


<a name='ack'></a>

## 10. Acknowledgements

This fun project is inspired by <a href="https://prg.cs.umd.edu/enae788m">ENAE788M: Hands-On Autonomous Aerial Robotics</a> at the University of Maryland, College Park and <a href="https://alliance.seas.upenn.edu/~meam620/wiki/index.php?n=Main.Spring2015">MEAM620: Advanced Robotics</a> at the University of Pennsylvania. 
