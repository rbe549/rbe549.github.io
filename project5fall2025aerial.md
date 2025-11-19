---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: The Final Race!
permalink: /rbe595/fall2025/proj/p5/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Environment](#environment)
  - [3.1. Racing Windows](#racingwindows)
  - [3.2. Unknown Window](#unknwonwindow)
  - [3.3. Run Back](#runback)
- [4. Attempt Termination](#attemptterm)
- [5. Scoring Criterion](#scoring)
- [6. D-Day of the Competition](#dday)
- [7. Implementation](#implementation)
- [8. Submission Guidelines](#sub)
  - [8.1. File tree and naming](#files)
  - [8.2. Report](#report)
  - [8.3. Video](#video)
- [9. Allowed and Disallowed functions](#funcs)
- [10. Collaboration Policy](#coll)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, Dec 12, 2025. Live Race on the same day from 10:00AM to 1:00PM. Teams will go in the order of their team number.** <br><br>

**YOU ARE NOT ALLOWED TO USE ANY LATE DAYS FOR THIS PROJECT!**


<a name='prob'></a>
## 2. Problem Statement 
Congratulations for making this far into the course. We know that you've worked very hard to get here and learned a lot of new concepts along the way. Now, it's time to put everything together. The aim of this project is to win the race on an obstacle course which utilizes all the algorithms you built from projects 1 through 4. Download the VizFlyt environment from <a href="https://app.box.com/s/c49un79r8c2jwpudkr3jvtb1u2fv9xy9">here</a>.

Remember that your quadrotor in VizFlyt comes with a front facing RGB camera along with noisy odometry estimates (obtained through controlling the quadrotor). The structure of the track (obstacle course) is known before along with a prior on pose of the obstacles as a uniform distribution, similar to previous projects. An oblique overview of the track is shown in Fig. 1 with the various stages marked.

<div class="fig fighighlight">
  <img src="/assets/2025/rbe595/p5/Track.jpg" width="100%">
  <div class="figcaption">
    Fig 1: VizFlyt Track.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='environment'></a>
## 3. Environment
The final track has **three** stages. Description of each stage is given next. The track in VizFlyt can change slightly (a gentle nudge in positions and orientations), enough to let hard-coded robots to crash.


<a name='racingwindows'></a>
## 3.1. Racing Windows

In the first stage (see Fig. 1), you will take off from the helipad/origin and you have to navigate though a set of three drone racing windows from project 3 as fast as possible without any collisions. The description of the windows are the same as in project 3.

<a name='unknwonwindow'></a>
## 3.2. Unknown Window

In the second stage (see Fig. 1), you have to navigate though an unknown shaped window from project 4 as fast as possible without any collisions. The description of the window is the same as in project 4.

<a name='runback'></a>
## 3.3. Run Back

In the final stage (see Fig. 1), you have to fly back through the same course back to origin (start) as soon as possible without collisions. Here, you have to first pass through the unknown window and the racing windows. All the windows appear white from the back. Once you reach origin or your attempt is terminated (see next section), your run is complete and your time is recorded in simulation.  



<a name='attemptterm'></a>
## 4. Attempt Termination

Doing any of the following will instantly terminate your attempt:
- Hitting the nets or crossing the extents of the boundary.
- Flying over a height of racing windows.
- Failing to navigate in any of the previous stages before proceeding to the next one.
- Crashing into any of the obstacles/track objects and/or the nets.


<a name='scoring'></a>
## 5. Scoring Criterion
Taking off from the helipad gives the team 10 points, then the stages 1 through 3 (finish) have the following point splits: 30, 25, 35 totaling a maximum of 100 points for each attempt. The team's attempt will be terminated if any of the things mentioned in <a href="#attemptterm">Section 4</a> happen. If the number of stages between two teams are tied, then the team with the lower simulation time comes out on top.


<a name='dday'></a>
## 6. D-Day of the Competition

On the day of the competition (happening over Zoom), the teams will go in the order of their team number. Each team will have a maximum time of 3 minutes per attempt to show the instructors their code in action and a maximum time of 15 minutes for all attempts combined. Between attempts, the team can use any amount of time (within the allotted 15 minutes of maximum time) to fix any software bugs or do changes in software. Only the best trial will be graded. All the teams need to be present for the entirety of the competition and the teams will go in the order of their team number unless they have requested a different order before. 

A sample photo of the VizFlyt track is shown in Fig. 1. The final track will be a perturbed version of the track given to you to test your algorithms. he competition will be from 10:00 AM to 1:00 PM over Zoom.

<!-- The competition will be from 10:00 AM to 1:00 PM. The lab will be setup close to the final track (only minor nudges will be made after) with the lighting conditions and so on at 7:30 AM on the day of the competition. You can go and measure anything you want, collect any data you desire and check the track until 9:30 AM on the day of the competition. From 9:30AM to 10:00 AM nobody will be allowed near the arena and the instructors will displace each obstacle randomly with a maximum disturbance of $$\pm$$0.5m in X and Y directions and $$\pm$$20 degrees in Yaw (just enough that hard-coding will not work). -->

The team with the highest points will win. Note that, completing the course (within the 3 minute slot per attempt) will get that team the maximum of 100 points.

The teams can place the quadrotor at the start in any desired orientation. Also, if a team wants to improve their live-demo score, they can request for an additional slot after all the teams have finished their demo with a penalty of 20 points of the total project 5 grade.


<a name='implementation'></a>
## 7. Implementation

This project is totally open ended! You can use any open-source code available online to solve any part of the problem. Make sure you CITE them. You are expected to use the implementations from the previous four projects but are NOT limited to it. You are NOT allowed to modify VizFlyt to obtain more information such as accurate odometry. This includes adding/subtracting any sensors. All your codes **MUST** run on the NVIDIA Jetson Orin Nano **without any cloud computation**. Your results are ONLY limited by your imagination and creativity!


<a name='sub'></a>

## 8. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 8.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p5.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p5.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p5/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p5.zip
├── Code
|   ├── main.py
|   └── Any subfolders you want along with files
├── Report.pdf
├── Video.mp4
└── README.md
```

<a name='report'></a>

### 8.2. Report

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Link to a ghosted photo?  

<a name='video'></a>

### 8.3. Video

Record your successful run in `.mp4` format during your demo (screen grab or saved frames convered to video) or before and submit it in the zip file. Make sure to name it as `Video.mp4`. Record the video in the highest resolution and fastest fps possible on your recording device with minimum being 1080p at 30fps.

<a name='funcs'></a>

## 9. Allowed and Disallowed functions

<b> Allowed:</b>

- Any code in the world

<b> Disallowed:</b>

- Other student's code in class

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2025/rbe595). 

<a name='coll'></a>

## 10. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE595-F02-ST Fall 2025 website](https://pear.wpi.edu/teaching/rbe595/fall2025.html).

