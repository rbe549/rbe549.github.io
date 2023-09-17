---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Mini Drone Race -- The Perception Saga!
permalink: /rbe595/fall2023/proj/p3a/
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
**11:59:59 PM, Oct 25, 2023.**

TODO: Add real experiment photos.

<a name='prob'></a>
## 2. Problem Statement 
Drone racing is a fast growing sport that is a great combination of technology and skill. In this sport, professional pilots fly small quadrotors through complex tracks at high speeds. Developing a fully autonomous racing drone is difficult due to challenges that span dynamics modeling, onboard perception, localization and mapping, trajectory generation, and optimal control. Recently, the Lockheed Martin hosted a competition called <a href="https://www.youtube.com/watch?v=2k0VF2a2ftc">AlphaPilot</a> with a whopping 1M USD grand prize.

In this project, you'll build an autonomy stack to navigate through multiple windows. The project is split into two parts: the first part being the perception stack and the second part being the planning, control and integration stack deployed on a real quadrotor.

<a name='environment'></a>
## 3. Environment

TODO: Add real photos

The track is made of multiple windows with their approximate 3D positions known but unknown orientations (within a reasonable range). In the first stage of project 3, i.e., project 3a, the goal would be to detect/segment the closest window using a deep learning approach and compute it's 3D pose. More details on the algorithmic parts of the perception stack are explained in the next section.  

<a name='windowloc'></a>
### 3.1. Window Locations
An example of the window location file format is given below:

```
# An example window environment file
# boundary xmin ymin zmin xmax ymax zmax
# window x y z xdelta ydelta zdelta qw qx qy qz xangdelta yangdelta zangdelta
boundary 0 0 0 45 35 6
window 1 1 1 0.2 0.2 0.2 0.52 0.85 0 0 5 5 5
```
- The format uses # for comments.
- The rectangular boundary extents for this environment are `(x0, y0, z0) = (0, 0, 0)` (lower left) and `(x1, y1, z1) = (45, 35, 6)` (upper right).
- A window line also specifies the center of the window, its approximate pose and variances. All values are whitespace  delimited and all numerical values are represented as a float. 
- Specifically, `x y z` represents the approximate center of the window in **meters**. 
- `xdelta ydelta zdelta` represents the variation in **meters** that is possible from `x y z` values (note that this is a uniform distribution and the probability of the window being outside this is 0).
- `qw qx qy qz` represents the approximate orientation of the window.
- `xangdelta yangdelta zangdelta` represents the ZYX euler angle variation in **degrees** that is possible from the approximate orientation given (again this is a uniform distribution and the probability of the window being outside this is 0). 
- No windows can overlap
- Please pay attention to the units.


TODO: Define frames of reference


<a name='perceptionstack'></a>
## 4. Perception Stack
There are three distinct parts in the perception stack.


<a name='dldetector'></a>
## 4.1. Window Detector
In the first step, implement a deep network to detect/segment the corners of the window or detect/segment the entire window. Here you have a flexibility in the design choice. You can use any network architecture or your choice. Furthermore, your network has to run fast enough on the NVIDIA Orin Nano, so be cognizant of your parameters and architecture.

Your goal would be to build a lightweight neural network that has good accuracy that can be used in the next steps. To accomplish this, you will have to generate windows in simulation (Blender) from different views and backgrounds and scenarios and train your network. We will describe the specifications of the window next. The window is rectangular (can be square as well) with checkerboard pattern on the edges. Each checkerboard pattern is (highlighted in blue in Fig. 3) 7x6 (for X or horizontal and Y or vertical) directions. The distance from checkboard pattern to either edge is the same dimension as one checkerboard square (yellow highlight in Fig. 3). The WPI logo will always be centered vertically and the PeAR logo will be centered horizontally (however, these can be absent too or be changed to other logos). The window will be the same WPI crimson color `(rgb(172, 43, 55))` but can appear different in different lighting. Your data can randomize the viewpoint (or window pose), window lighting, window design with respect to logos, noise, window shape within the confines of the definition among others. The thickness of the window is negligible compared to the height and width of the window. There can be multiple windows in the frame. Also, each window is X-Y symmetric.  

TODO: Add Photo

<a name='detfilt'></a>
## 4.2. Detection Filtering
Once you have obtained detections/segmentations from the previous step, you might notice that there are multiple windows in the frame, you will need to filter and extract the closest window to fly to. 


TODO: Add Photo

<a name='sim2real'></a>
## 4.3. Sim2Real
You have trained your neural network in simulation with domain randomization in the hopes of a good sim2real transfer. To test this, you will collect data from your DJI Tello's camera (you don't have to fly it if you don't want to for this part). Then run inference on your collected data and inspect to see how well your stack works. If it does not work well (as well as you think, it will **NOT** be perfect, do not worry) , go back and change your data generation or use other methods to perform a better sim2real transfer. Let your creativity run wild in each of the parts here.




TODO: Add Photo



TODO: Add block diagram

<a name='pose'></a>
## 4.4. Pose
In order for the quadrotor to fly through the window in the project 3b, the easiest way is to detect the 3D pose of the window with respect to the current quadrotor position. The first step in doing this is camera calibration.

<a name='calib'></a>
## 4.4.1. Calibration
Camera Intrinsic calibration entails with estimating the camera calibration matrix $$K$$ which includes the focal length and the principal point and the distortion parameters. You'll can use the awesome calibration package developed by ETHZ [Kalibr](https://github.com/ethz-asl/kalibr/wiki/multiple-camera-calibration) or the <a href="https://www.mathworks.com/help/vision/camera-calibration.html">Matlab's Calibration toolbox</a> to do this. You'll need either a checkerboard or an april grid to calibrate the camera. We found that using the April grid gave us superior results. Feel free to print one (don't forget to turn off autoscaling or scaling of any sort before printing). Bigger april grids or checkerboard in general give more accurate results. Large calibration targets from <a href="https://calib.io/?gad=1&gclid=Cj0KCQjw2qKmBhCfARIsAFy8buJ6-5FR2Kehobn5eJlkqbKqofbV3DYM-HAFvoXqYQNxD-Nvb2xM4XYaAmVQEALw_wcB">calib.io</a> are located in UH100B (Fig. 4) which you are free to use if you don't want to print your own. Please be careful with these calibration targets as they are super expensive and hard to replace.

TODO: Add Image

You also sometimes need to color calibrate the camera. A simple way to do this is to use a color calibration grid as shown in Fig. 6. It has different standard colors which you'll match up visually. Another simple way is to capture a scene with "gray" color and make sure that it looks gray. Note that your results will vary based on your monitor's color calibration as well. Color calibration is important so that the complete dynamic range of all channels are used when converting from bayer to RGB image. You might not need this step is your data has enough variation such that your neural network models are robust to color variation.

<a name='pnp'></a>
## 4.4.2. Pose Using PnP
Now that you have the camera calibration matrix $$K$$, the detections of window corners from your deep learning + filtering stack on the 2D image, the 3D dimensions of parts of the window, you can estimate the 3D pose $$[R, T]$$ of the window using the <a href="https://docs.opencv.org/4.x/d5/d1f/calib3d_solvePnP.html">`solvePnP`</a> function in OpenCV.


<a name='posefilt'></a>
## 4.4.3. Pose Filtering
Optionally, if your detections are not very accurate, you can filter your pose using variants of Bayesian filters or a factor graph. Let your creativity run wild in this part.



Evaluation, Plot, Video




<div class="fig fighighlight">
  <img src="/assets/2023/rbe595/TraditionalOverview.png" width="100%">
  <div class="figcaption">
    Fig 1: Overview of panorama stitching using traditional method.
  </div>
  <div style="clear:both;"></div>
</div>



<a name='testset'></a>
## 5. Testing (Live Demo)
On the day of the deadline, each team will be given a 15 minute slot for demoing their code in action to the instructors. The instructors will place the obstacles as they wish (obstacle locations will be given to you in the map). The task is the fly to the goal location as fast as possible without any collisions. You can get as many attempts as you want to accomplish this within your 15 minute time slot.


<a name='sub'></a>

## 6. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 6.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p3a.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``.For our example, the submission file should be named ``abc_p3a.zip``. The file **must have the following directory structure**. The file to run for your project should be called ``YourDirectoryID_p2b/Code/Wrapper.py``. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p3a.zip
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

