---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Fly through boxes! 
permalink: /rbe595/fall2026/proj/p2b/
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
  - [5.4. Extra Credit](#extra)
- [6. Allowed and Disallowed functions](#funcs)
- [7. Collaboration Policy](#coll)
- [8. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, Oct 09, 2026.**  

<a name='prob'></a>
## 2. Problem Statement 
In this project, you will implement the navigation (planning and control) stack from Project 2a on a Crazyflie flying inside a Gaussian-splat reconstruction of the Washburn flight volume. You will tune the loop in simulation against that scene and then fly the same code on the real drone under motion capture. The starter code can be downloaded from <a href="https://app.box.com/s/khzufw1i2awjcyenf6aj9mmppzbjoyb9">here</a>. The map is inside `map1_2b.txt` and the start and goal locations are in `task.json`.

<a name='environment'></a>
## 3. Environment
The map is known in advance, in the same file format as Project 2a, so the parser and the collision checks you wrote there work unchanged. The map for this experiment is `map1_2b.txt` in the starter code, and the environment it describes is shown in Fig. 1.

**Everything is in metres, in the Vicon frame.** The map, the start and goal pairs, and the pose the drone reports are all in that one frame — there is no separate scale factor for you to apply. Its origin and axis directions are shown in Fig. 1: x runs along the course, y across it, z up, with z = 0 at the floor.

The start and goal pairs are in `task.json`, each given as an (x, y, z) triple in metres.

**The boundary line in `map1_2b.txt` is the flyable box, not the room.** Its zmin is 0.30 m and no part of your plan may go below it. Its zmax is 1.80 m, and the obstacle heights in the file are the real ones -- the tallest is 1.36 m -- which leaves 0.44 m between that obstacle and the ceiling.


<div class="fig fighighlight">
  <img src="/assets/2026/rbe595/p2/Frames.png" width="100%">
  <div class="figcaption">
    Fig 1: The flight volume, rendered from the Gaussian splat you will fly in. The view looks along +x; the axes are drawn at the Vicon origin, which sits on the L marker taped to the floor. The four obstacles are the ones listed in `map1_2b.txt`.  
  </div>
  <div style="clear:both;"></div>
</div>

<a name='implementation'></a>
## 4. Implementation
You will be flying a Crazyflie inside the Gaussian-splat reconstruction of the flight volume. The goal is to navigate through a known map given the oracle position: the drone's pose comes from the Vicon motion-capture system, so you may treat it as ground truth and spend your effort on planning and control rather than on state estimation. Essentially, you will be implementing a path planner, a waypoint trajectory generator and a position controller. To do this, you will modify your code from Project 2a and integrate it with the `splat_hitl` and `cf_vicon_stack` packages (more details on Turning setup are given in `install.md` file in the starter package and WPI's Turning documentation can be found <a href="https://docs.turing.wpi.edu/">here</a>). 

**Your controller is a position loop whose output is a velocity.** The drone takes `cmd_hover`, which is a velocity command, and the Crazyflie closes the inner rate loop in its own firmware. Do not port the cascaded inner loop from Project 2a. That is nine gains to tune, not eighteen.

The goal is to navigate through the scene as fast as possible. <b>Show your quadrotor pose in the map in matplotlib through the run.</b>

A video tutorial on Turning access by the previous TA Deepak is shown below and can be downloaded from <a href="https://app.box.com/s/a3iel0zc7dwfvmsh28qkinhd1txp0chd">here</a>.

<div class="fig fighighlight">
  <iframe src="https://app.box.com/embed/s/a3iel0zc7dwfvmsh28qkinhd1txp0chd?sortColumn=date" width="330" height="400" frameborder="0" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>
  <div class="figcaption">
    Fig 2: Video tutorial of Turing Access. 
  </div>
  <div style="clear:both;"></div>
</div>


<a name='collision'></a>
## 4.1. Collision Handling
Your quadrotor should  fly as fast as possible. However, a real quadrotor is not allowed to collide with anything (<a href="https://www.youtube.com/watch?v=TVrxvqYlCDs">video</a>). Therefore, we have zero tolerance towards collision - if you collide, you crash, you get zero for that test. Your run is also scored automatically, against the scene's distance field: the monitor latches a failure the first time the drone's centre comes within 0.10 m of reconstructed geometry, or leaves the mapped volume. It does not reset -- there is one verdict per run.

As you program your controller you will find that it overshoots, and trajectory smoothing will pull the flown path off the planned one.

**Inflate the obstacles.** The map describes a point robot and exact boxes. You are flying a drone with a 75 mm radius, through a reconstruction in which the same box surface sits a few centimetres away from the number in the file. Remember the boundary as well as the blocks, because clipping a wall is also a crash. **Start from about 100 mm of inflation.** The amount used at test time may differ; use the number you are given then. 


<a name='sub'></a>

## 5. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 5.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``p2b_GroupGROUPNUM.zip``. Find your group number on Canvas. If your ``GROUPNUM`` is 1, then for our example the submission file should be named ``p2b_Group1.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``p2b_GroupGROUPNUM/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, **be sure to index them using RELATIVE paths, never absolute ones** -- your code has to run on a machine that is not yours, and an absolute path from your laptop will not resolve on the grader's. If you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
p2b_GroupGROUPNUM.zip
├── Code
|   ├── Wrapper.py
|   ├── path_planner.py
|   ├── trajectory_generator.py
|   ├── control.py
|   ├── environment.py
|   └── Any other helper modules you wrote
├── Report.pdf
├── RunVideo.mp4
├── VisVideo.mp4
├── VidTop.mp4          <- extra credit only
├── VidObl.mp4          <- extra credit only
└── README.md
```

Submit only what you wrote. Do **not** include the starter pack, the scene
(splat, ESDF, point cloud), `map1_2b.txt`, `task.json`, or any flight logs --
we already have them, and they will push you past the size limit. Your
`Wrapper.py` should take the path to the starter pack as a command line
argument with a sensible relative default, so it can be pointed at our copy.

<a name='report'></a>

### 5.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.

<a name='video'></a>

### 5.3. Video

Record your successful run in `.mp4` format, during your demo or before it, and submit it in the zip file. Name it `RunVideo.mp4`.

Your matplotlib visualisation -- the quadrotor pose in the map, with the obstacles, the planned path and the trajectory -- should be a second video, named `VisVideo.mp4`.

<a name='extra'></a>

### 5.4. Extra Credit

Implementing the extra credit can earn you bonus points. Generate a video of your RRT\* in action, like the ones shown below: the tree expanding, the nodes being searched, and the final path and trajectory. Do this for your live run -- the recorded Vicon poses -- on the splat given to you. Attach both a top-down and an oblique view, named `VidTop.mp4` and `VidObl.mp4`.

<div class="fig fighighlight">
  <iframe src="https://app.box.com/embed/s/r3x2p0i7lcd6lrmyzg850agfux9o61nk?sortColumn=date" width="330" height="400" frameborder="0" allow="local-network-access ; clipboard-read; clipboard-write " allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>
  <iframe src="https://app.box.com/embed/s/mchhl63kkbqjijhcbtdm6tava3k1or33?sortColumn=date" width="330" height="400" frameborder="0" allow="local-network-access; clipboard-read ; clipboard-write" allowfullscreen webkitallowfullscreen msallowfullscreen></iframe>
  <div class="figcaption">
    Fig 3: Examples of the extra credit -- the RRT\* tree expanding, the nodes searched, and the final path and trajectory, seen from two viewpoints.
  </div>
  <div style="clear:both;"></div>
</div>

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

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2026/rbe595). 

<a name='coll'></a>

## 7. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE595-F02-ST Fall 2026 website](https://pear.wpi.edu/teaching/rbe595/fall2026.html).

<a name='ack'></a>

## 8. Acknowledgements

This fun project is inspired by <a href="https://prg.cs.umd.edu/enae788m">ENAE788M: Hands-On Autonomous Aerial Robotics</a> at the University of Maryland, College Park and <a href="https://alliance.seas.upenn.edu/~meam620/wiki/index.php?n=Main.Spring2015">MEAM620: Advanced Robotics</a> at the University of Pennsylvania. 
