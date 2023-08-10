---
layout: page
mathjax: true
coursetitle: RBE595-F02-ST -- Hands-On Autonomous Aerial Robotics
title: Alohomora
permalink: /rbe595/fall2023/proj/p0/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Phase 1: IMU Attitude Estimation](#phase1)
  - [2.1. Problem Statement](#prob1)
  - [2.2. Reading the data](#data1)
  - [2.3. Sensor Calibration](#calib1)
  - [2.4. Implementation](#implementation1)
- [3. Phase 2: Waypoint Navigation and AprilTag Landing](#phase2)
  - [3.1. Problem Statement](#prob2)
  - [3.2. Codebase](#codebase)
  - [3.3. Implementation](#implementation2)
- [4. Submission Guidelines](#sub)
  - [4.1. File tree and naming](#files)
  - [4.2. Report](#report)
  - [4.3. Video for Phase 2](#video) 
- [5. Allowed and Disallowed functions](#funcs)
- [6. Collaboration Policy](#coll)
- [7. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, August 24, 2023.**

<a name='phase1'></a>
## 2. Phase 1: IMU Attitude Estimation

<a name='prob1'></a>
### 2.1. Problem Statement 
In this phase, you will implement three methods to estimate the three dimensional orientation/attitude. You are given data from an [ArduIMU+ V2](https://www.sparkfun.com/products/retired/9956), a six degree of freedom Inertial Measurement Unit (6-DoF IMU) sensor i.e., readings from a 3-axis gyroscope and a 3-axis accelerometer. You will estimate the underlying 3D orientation and compare it with the ground truth data given by a [Vicon motion capture system](https://www.vicon.com/).

<a name='data1'></a>
### 2.2. Reading the Data
The `Phase1\Data` folder has two subfolders, one which has the raw IMU data `Phase1\Data\Train\IMU` and another one which has the Vicon data `Phase1\Data\Train\Vicon`. The data in each folder is numbered for correspondence, i.e., `Phase1\Data\Train\IMU\imuRaw1.mat` corresponds to `Phase1\Data\Train\Vicon\viconRot1.mat`. Download the project 0 package from [here](https://drive.google.com/file/d/1EhSywBhsXSyUUijVkeRaEO-5auc8paVO/view?usp=sharing) which includes the data in `Phase1` folder. These data files are given in a `.mat` format. In order to read these files in Python, use the snippet provided below:

```
>>> from scipy import io
>>> x = io.loadmat("filename.mat")
```

This will return data in a dictionary format. Please disregard the following keys and corresponding values: `__version__`, `__header__`, `__global__`. The keys: `vals` and `ts` are the main data you need to use. `ts` are the timestamps and `vals` are values from the IMU. Each column of `vals` denotes data in the following order: $$ \begin{bmatrix} a_x & a_y & a_z & \omega_z & \omega_x & \omega_y\end{bmatrix}^T$$. Note that these values are not in physical units and need to undergo a conversion. 

To convert the acceleration values to $$ms^{-2}$$, follow these steps. 

$$  
\tilde{a_x} = \frac{a_x + b_{a,x}}{s_x} \\
$$

Follow the same steps for $$a_y$$ and $$a_z$$. Here $$\tilde{a_x}$$ represents the value of $$a_x$$ in physical units, $$b_{a,x}$$ is the bias and $$s_x$$ is the scale factor of the accelerometer. 

To read accelerometer bias and scale parameters, load the `Data\IMUParams.mat` file. `IMUParams` is a $$2 \times 3$$ vector where the first row denotes the scale values $$\begin{bmatrix} s_x & s_y & s_z \end{bmatrix}$$. The second row denotes the biases (computed as the average biases of all sequences using vicon) $$\begin{bmatrix} b_{a, x} & b_{a, y} & b_{a, z} \end{bmatrix}$$. 

To convert $$\omega$$ to $$rads^{-1}$$, 

$$
\tilde{\omega} = \frac{3300}{1023} \times \frac{\pi}{180} \times 0.3 \times \left(\omega - b_{g}\right)
$$

Here, $$\tilde{\omega}$$ representes the value of $$\omega$$ in physical units and $$b_g$$ is the bias. $$b_g$$ is calculated as the average of first few hundred samples (assuming that the IMU is at rest in the beginning). 

From the Vicon data, you will need the following 2 keys: `ts` and `rots`. `ts` is the timestamps of size $$1 \times N$$ as before and `rots` is a $$3\times 3\times  N$$ matrix denoting the `Z-Y-X` Euler Angles rotation matrix estimated by Vicon. 

An image of the rig used for data collection is shown below:

<div class="fig fighighlight">
  <img src="/assets/2019/p1/IMURig.png" width="100%">
  <div class="figcaption">
    Figure 1: IMU Rig used for data collection.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='calib1'></a>
### 2.3. Sensor Calibration
Note that the registration between the IMU coordinate system and the Vicon global coordinate system might not be aligned at start. You might have to align them. 

The Vicon and IMU data are NOT hardware synchronized, although the timestamps `ts` of the respective data are correct. Use `ts` as the reference while plotting the orientation from Vicon and IMU. You can also do a software synchronization using the time stamps. You can align the data from the two closest timestamps from IMU and Vicon data respectively or interpolate using <a href="https://en.wikipedia.org/wiki/Slerp">Slerp</a>. **You can use any third party code for alignment of timestamps.**


<a name='implementation1'></a>
### 2.4. Implementation

1. You will write a function that computes orientation only based on gyro data (using integration, assume that you know the initial orientation from Vicon). Check if that works well. Plot the results and verify. Add the plot to your report file. For references, watch the video <a href="https://www.youtube.com/watch?v=8hRoASoBEwY&list=PLZgpos4wVnCYhf5jsl2HcsCl_Pql6Kigk&index=3">here</a> and read the tutorial here <a href="https://nitinjsanket.github.io/tutorials/attitudeest/imu.html"></a>. Feel free to explore other resources to learn these concepts. Usage of ChatGPT is also encouraged as long as you do not blatantly plagiarize. 
2. You will write another function that computes orientation only based on accelerometer data (assume that the IMU is only rotating). Verify if that function works well before you try to integrate them into a single filter. Add the plot to your report file.For references, watch the video <a href="https://www.youtube.com/watch?v=8hRoASoBEwY&list=PLZgpos4wVnCYhf5jsl2HcsCl_Pql6Kigk&index=3">here</a> and read the tutorial here <a href="https://nitinjsanket.github.io/tutorials/attitudeest/imu.html"></a>. Feel free to explore other resources to learn these concepts. Usage of ChatGPT is also encouraged as long as you do not blatantly plagiarize.
3. You will write a third function that uses simple complementary filter to fuse estimates from both the gyroscope and accelerometer. Make sure this works well before you implement the next step. You should observe the estimates to be almost an average of the previous two estimates. Report the value of fusion factor $$\alpha$$ in your report. Also, add the plot to your report file.For references, watch the video <a href="https://www.youtube.com/watch?v=8hRoASoBEwY&list=PLZgpos4wVnCYhf5jsl2HcsCl_Pql6Kigk&index=3">here</a> and read the tutorial here <a href="https://nitinjsanket.github.io/tutorials/attitudeest/imu.html"></a>. Feel free to explore other resources to learn these concepts. Usage of ChatGPT is also encouraged as long as you do not blatantly plagiarize.


In the starter code, a function called `rotplot.py` is also included. Use this function to visualize the orientation of your output. To plot the orientation, you need to give a $$3 \times 3$$ rotation matrix as an input.

<a name='phase2'></a>
## 3. Phase 2: Waypoint Navigation and AprilTag Landing

<a name='prob2'></a>
### 3.1. Problem Statement 
In this phase, you will design and implement a navigation and landing system for a simulated quadrotor in an environment. The quadrotor is equipped with the ability to navigate to the provided 3D position waypoint. You are required to autonomously navigate through a sequence of predefined waypoints, perform AprilTag scanning after you reach every waypoint, and execute a landing maneuver on an AprilTag with the ID value of `4`, note that you still have to navigate through all waypoints for completion of the mission. For e.g., if you have 3 waypoints with Tag ID's (unknown before) as 1,4,2 (See Fig. 2). You will navigate to waypoint 1 and then continue to waypoint 2, then land on waypoint 2, takeoff again and continue to waypoint 3 for completion. 


<div class="fig fighighlight">
  <img src="/assets/2023/rbe595/p0/DroneExample.png" width="50%">
  <div class="figcaption">
    Figure 2: Sample drone scenario.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='codebase'></a>
### 3.2. Codebase

To setup the codebase, install Blender 3.6 from the <a href="https://www.blender.org/">official website</a>. Further install the following python libraries: `imath numpy opencv-python scipy pyquaternion`. The codebase and the `.blend` file are included in the project 0 package from Sec. <a href="#data1">2.2</a> in the `Phase2` folder. 

For running the code, please watch <a href="https://drive.google.com/file/d/1-M7xDqk84THtnoylcP2t6DEcSeHWf1hJ/view?usp=sharing">this video</a>, also attached below from your awesome TA Manoj. If you are super new to Blender, you can also check out <a href="https://drive.google.com/drive/folders/1Shr6cQDLWiov53f1tXZArhBBfoPczNTp">this</a> awesome Blender tutorial by Ramana which was made for the Computer Vision class. 

<iframe src="https://drive.google.com/file/d/1-M7xDqk84THtnoylcP2t6DEcSeHWf1hJ/preview" width="640" height="480" allow="autoplay"></iframe>


<a name='implementation2'></a>
### 3.3. Implementation

You need to implement the following:
- A state machine to monitor and supply the next waypoint. Note that you are given a template starter code in `Phase2\src\usercode.py` with the template class `state_machine`. You are required to fill the `step()` function of this class, where you need to monitor if you have reached the current desired waypoint (you will need to check the Euclidean distance between the current position given as `currpos` in your `step()` function and the desired waypoint) and switch to the landing/next waypoint based on the April Tag ID. To run the code you'll run the `Phase2\src\main.py` and **NOT** `Phase2\src\usercode.py`.
- AprilTag detection for next task to be performed. Your quadrotor is equipped with a down-facing camera and you are provided with a function `fetchLatestImage()` that returns the current camera frame. You can use utilize the <a href="https://pypi.org/project/apriltag/">`apriltag`</a> library to detect april tags, note that you are given tags from the `36h11` tag family. Once you reach a waypoint, fetch the latest camera frame, detect the April Tag ID, if it is `4`, invoke a land command. To land, you have to generate a waypoint with the same `XY` location and `Z` location of 0.1m. Once you have landed, you can takeoff again, i.e., maintain same `XY` location and make the `Z` location 2m. Then continue onto the next waypoint if any. If there are no more waypoints left, terminate your code and dance (in real-life)! Note that you do not need the images to test your waypoint switching logic but you'll need the image to know when to land. So you can test your state switching code without landing without the need to render images, this will make your debugging fast.


<a name='sub'></a>

## 4. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 4.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p0.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``. For our example, the submission file should be named ``abc_p0.zip``. The file **must have the following directory structure**. The file to run for the phase 1 of the project should be called ``YourDirectoryID_p1a/Code/Wrapper.py`` and for phase 2 it should be `main.py` as shown in the file structure below. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your Wrapper codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>100MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p0.zip
├── Phase1
|    └── Code
|        ├── Wrapper.py
|        ├── Any subfolders you want along with files
|        └── Report.pdf
|
├── Phase2
|    ├── log
|    ├── src
|    |    ├── usercode.py
|    |    ├── main.py
|    |    └── Other supporting files
|    ├── models
|    ├── outputs
|    ├── indoor.blend   
|    ├── main.blend    
|    └── Video.mp4   
└── README.md
```


<a name='report'></a>

### 4.2. Report for Phase 1

For each section of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Link to the `rotplot` videos comparing attitude estimation using Gyro Integration, Accelerometer Estimation, Madgwick Filter and Vicon. Sample video can be seen [here](https://www.youtube.com/watch?feature=player_embedded&v=iCe3o-9moUM).
- Plots for all the train and test sets. In each plot have the angles estimated from gyro only, accelerometer only, madgwick filter and Vicon along with proper legend.  
- A sample report for a similar project is given in the `.zip` file given to you with the name `SampleReport.pdf`. Treat this report as the benchmark or gold standard which we'll compare your reports to for grading.

<a name='video'></a>

### 4.3. Video for Phase 2

The video should be a screen capture of your code in action from both an oblique and top view as shown in the <a href="https://drive.google.com/file/d/1M8eFTep7xnmnh_rSPhul49IDT_Kr8jhr/view?usp=sharing">video below</a>. Note that a screen capture is not a video recorded from your phone. 

<iframe src="https://drive.google.com/file/d/1M8eFTep7xnmnh_rSPhul49IDT_Kr8jhr/preview" width="640" height="480" allow="autoplay"></iframe>

<a name='funcs'></a>

## 5. Allowed and Disallowed functions

<b> Allowed:</b>

- Any functions regarding reading, writing and displaying/plotting images in `cv2`, `matplotlib`
- Basic math utilities including convolution operations in `numpy` and `math`
- Any functions for pretty plots
- Quaternion libraries
- Any library that perform transformation between various representations of attitude
- Any code for alignment of timestamps
- Usage of ChatGPT (or any other LLM) is allowed as long as you include the prompts used in your report and blatantly do not plagiarize from ChatGPT (includes copy pasting entire code)

<b> Disallowed:</b>

- Any function that implements in-part or full Gyroscope integration, accelerometer attitude estimation, Complementary filter or Madgwick filter including usage of LLMs such as ChatGPT to write code for this


If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2023/rbe595). 

<a name='coll'></a>

## 6. Collaboration Policy

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE595-F02-ST Fall 2023 website](https://nitinjsanket.github.io/teaching/rbe595/fall2023.html).

<a name='ack'></a>

## 7. Acknowledgements

This data for this fun project was obtained by the ESE 650: Learning In Robotics course at the University of Pennsylvania.

