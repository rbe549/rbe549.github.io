---
layout: page
mathjax: true
title: EinsteinVision
permalink: /spring2025/proj/p3/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Data](#data)
- [4. Phase 1: Basic Features](#ckpt1)
- [5. Phase 2: Advanced Features](#ckpt2)
- [6. Phase 3: Bells and Whistles](#ckpt3)
- [7. Extra Credit: Cherry on Top](#extra)
- [8. Hints, Tips and Tricks](#hints)
- [9. Submission Guidelines](#sub)
  - [9.1. Phase 1 Submission](#chkpt1sub)
  - [9.2. Final File tree and naming](#files)
  - [9.3. Final Report](#report)
  - [9.4. Final Presentation](#present)
- [10. Allowed and Disallowed functions](#funcs)
- [11. Collaboration Policy](#coll)
- [12. Acknowledgments](#ack)


<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, March 30, 2025 for Phase 1, 11:59:59 PM, April 08, 2025 for Phase 2, 11:59:59 PM, April 15, 2025 for Phase 3 (and extra credit).** This project is to be done in groups of 2 and has a 10 min presentation. To summarize, various Phases are due at different dates and are given below:
- Phase 1 is due on **11:59:59 PM, March 30, 2025** 
- Phase 2 are due on **11:59:59 PM, April 08, 2025.**
- Phase 3 (and extra credit)  are due on **11:59:59 PM, April 15, 2025.**

<a name='prob'></a>
## 2. Problem Statement 
A key component of any product which a human has to interact with are beautiful visualizations. This is the first step for a human to build trust with an autonomous machine. For example, think of your first smartphone's UI and compare it to how it looks now. This not only makes things easier to use but also more efficient and intuitive. In summary, a good visualization system takes in sensory information and provide intuitive insights into how the robot is thinking. Visualizations are not only important for debugging the issues in the software but they are also a basic Human Robot Interaction (HRI) problem as we mentioned before. One such classic visualization is the infamous `rviz` as shown in Fig. 1.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/rviz.png" width="100%">
  <div class="figcaption">
    Fig 1: RViz Visualization of sensor data on a self-driving car.
  </div>
  <div style="clear:both;"></div>
</div>

Another example of the visualization looks in a Tesla car is shown in Fig. 2.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavisionrudimentary.png" width="100%">
  <div class="figcaption">
    Fig 2: Tesla's backend visualization in autonomous mode.
  </div>
  <div style="clear:both;"></div>
</div>


Both rviz and Tesla's earlier dashboards fail at the HRI problem, rendering (pun intended) it useless for common usage.
 
Tesla's latest visualization (Fig. 3) is a really good example of visualization which provides a lot of intuitive insights about information it perceives. 

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavision.png" width="100%">
  <div class="figcaption">
    Fig 3: Tesla's frontend visualization in autonomous mode.
  </div>
  <div style="clear:both;"></div>
</div>


In this project, you will build an visualization inspired and bettered version of Tesla's dashboard (see Vid. 1 and Fig. 3) and hence the name 😉, wherein you are provided with a set of videos recorded from cameras of a 2023 Tesla Model S (shown in Vid. 1). You are required to output a rendered video of your visualizations: atleast showing the view in front of the car and your car, you can also show everything around the car. You are free to use any approach (deep learning based or classical) available in the world and render the visualizations using <a href="https://www.blender.org/">Blender</a>. This project is open-ended and hence there are multiple right solutions. **You are graded for your creativity in approaching the problem, the effectiveness and prettiness of the visualizations.** 

<div class="fig fighighlight">
  <div class="figcaption">
<iframe width="560" height="315" src="https://www.youtube.com/embed/2q96i2vf8oA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><br>
    Video 1: Video showing Tesla's FSD Visualization.
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslamodels.jpg" width="100%">
  <div class="figcaption">
    Fig 4: 2023 Tesla Model S used to capture data.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='data'></a>
## 3. Data
Please download the data package from <a href='https://app.box.com/s/zjys9xcefyqfj2oxkwsm5irgz6g3hp1l'>here</a>. Also please watch this awesome Blender tutorial by Ramana. You can also download the tutorial from <a href="https://drive.google.com/drive/folders/1Shr6cQDLWiov53f1tXZArhBBfoPczNTp">here</a>.

<iframe src="https://drive.google.com/file/d/1zWn7Pkytcg2ElhWZYySJnPGk7D5Vr8Ic/preview" width="640" height="480" allow="autoplay"></iframe>

The data given to you contains the following:
- Assets (Blender models in the ``Assets`` folder) for various things like Cars: Sedan, SUV, Pickup truck, Bicycle, motorcycle, Truck, Traffic signal, Stop Sign, Traffic Cone, Traffic Pole, Speed Sign and Pedestrian. We also include texture images for stop sign and a blank speed sign (add your own speed as text here).
- Videos (Undistorted and Raw in the ``Sequences`` folder) for 13 sequences under various conditions with what scenarios are encountered in the respective markdown files in each folder. For each sequence, the content of the video are listed in a `contents.md` file.
- Calibration Videos (in the ``Calib`` folder) used to calibrate the cameras.


<a name='ckpt1'></a>
## 4. Phase 1: Basic Features
In this Phase, you are required to implement basic features which are absolutely essential for a self-driving car. We will describe these features next:

1. **Lanes:** Identify and show the different kinds of lanes on the road, they could be dashed, solid and/or of different color (white and yellow). Each lane has significance and are essential to identify (See Fig. 5).
2. **Vehicles:** Here, identify all cars (but you do not need to classify as different types) and represent them as a car shape (See Fig. 5, all your cars will look the same).
<!-- 3. **Pedestrians and their pose:**  You need identify and locate pedestrians and their poses and display them in the scene (See Figs. 5 and 6, all your pedestrians will look the same, i.e., they will face the same way). -->
3. **Pedestrians:**  You need identify and locate pedestrians and display them in the scene (See Figs. 5, all your pedestrians will look the same, i.e., they will face the same way).
4. **Traffic lights:** Indicate the traffic signals and it’s color (See Fig. 7, note that you do not need to classify arrows in the traffic signals here).
5. **Road signs:** There are sign boards on the road you need identify and represent. In this Phase, you need to primarily indicate stop signs (Fig. 6) . The models and texture images are given separately and you need to apply the textures appropriately. 


<!-- and speed limit signs (Fig. 8) -->
<!-- The speed limit texture is blank and hence you need to add the numbers appropriately.  -->

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslalanes.jpg" width="100%">
  <div class="figcaption">
    Fig 5: A sample showing pedestrians, different vehicles, double yellow solid lines, traffic lights, stop sign and arrows on the road.
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaped.jpg" width="100%">
  <div class="figcaption">
    Fig 6: A sample showing different vehicles, different lanes (white solid lines, white dashed lines and double yellow solid lines).
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslatrafficlights.png" width="100%">
  <div class="figcaption">
    Fig 7: Left: A visualization during a traffic stop, Right: Zoomed in view of traffic lights showing more details such as arrows and different colors (when applicable).
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaspeedsign.png" width="50%">
  <div class="figcaption">
    Fig 8: A visualization showing the speed limit sign highlighted in red.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='ckpt2'></a>
## 5. Phase 2: Advanced Features
In this Phase, you will build on top of the previous one by enhancing and adding more features. Here, we add more granularity to the vision system which can aid in algorithmic decisions in navigation modules.  


1. **Vehicles:** Here, you need to classify (identify different vehicles) and subclassify them (identify different kinds of a type of vehicle). You also need to identify the orientation of the vehicles and display them. Note that you have to display these detections as the respective 3D model in your renders (See Fig. 9). More particularly, 
  - Cars: Sedan, SUV, hatchback, Pickup trucks
  - Trucks
  - Bicycle
  - Motorcycle
2. **Traffic lights:** Additionally to the previous Phase, classify arrows on the traffic lights here (Fig. 7).
3. **Road signs:** Along with the previously mentioned stop signs, you should also indicate road signs on the ground such as arrows (See Fig. 6) and speed limit signs (Fig. 8). The speed limit texture is blank and hence you need to add the numbers appropriately.
4. **Objects:** You also need to indicate additional objects like dustbins, traffic poles, traffic cones and traffic cylinders as their respective 3D models in the renders (See Fig. 10). 
5. **Pedestrian pose:** You need to identify pedestrian pose each frame instead of just classifying them and display them (See Fig. 6).


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavehicles.png" width="100%">
  <div class="figcaption">
    Fig 9: Various scenarios showing different vehicles such as cars, trucks, motorcycles, etc.
  </div>
  <div style="clear:both;"></div>
</div>

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaobj.png" width="100%">
  <div class="figcaption">
    Fig 10: Various scenarios showing different objects such as dustbins, traffic poles, traffic cones.
  </div>
  <div style="clear:both;"></div>
</div>



<a name='ckpt3'></a>
## 6. Phase 3: Bells and Whistles

In this Phase, we try to add further cognitive abilities for better decision making in our planning stage.

1. **Break lights and indicators of the other vehicles:** Identify and display the vehicle break lights and indicator signals (See Fig. 11). This helps the navigation module in making better lane changing decisions. 
2. **Parked and Moving Vehicles:** Distinguish between parked and moving vehicles and display (make it subtle but identifiable). For the moving cars, you also need to identify their moving direction and display them (with an arrow or anything else that you like). Models that can directly output object motions with video input, like <a href='https://github.com/gengshan-y/rigidmask'>rigidmask</a>, are not allowed. You can consider using a network that can output optical flow, such as <a href='https://github.com/princeton-vl/RAFT'>RAFT</a>, and calculate <a href='https://docs.opencv.org/4.x/d9/d0c/group__calib3d.html#gacbba2ee98258ca81d352a31faa15a021'>Sampson distance</a> (See Fig. 12).


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaindicator.jpg" width="100%">
  <div class="figcaption">
    Fig 11: Visualization showing brake lights and indicators.
  </div>
  <div style="clear:both;"></div>
</div>

<div class="fig fighighlight">
  <img src="/assets/2024/p3/video5_frame_195_296.jpg" width="100%">
  <div class="figcaption">
    Fig 12: Top row: Input image frames. Bottom row (left to right): Example optical flow output and sampson distance (brighter has higher Sampson distance value).
  </div>
  <div style="clear:both;"></div>
</div>



<a name='extra'></a>
## 7. Extra Credit: Cherry on Top
Implementing the extra credit can give you up to 25% of bonus score. Like a cherry on the top, you need to identify and indicate:

1. **Speed bumps:** (See Fig. 13) This accounts for 10% of bonus score. No asset is given for this. Feel free to make your own.
2. **Collision prediction of pedestrians or other vehicles as red highlight:**  (See Fig. 14) This accounts for 15% of bonus score. This is as simple as changing the material color when the crash is detected.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaspeedbump.jpg" width="100%">
  <div class="figcaption">
    Fig 13: Visualization of speed bumps.
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslapedcoll.jpg" width="100%">
  <div class="figcaption">
    Fig 14: Visualization of collision of the pedestrian (left) and car (right).
  </div>
  <div style="clear:both;"></div>
</div>


<a name='hints'></a>

## 8. Hints, Tips and Tricks
This project involves a lot of concepts from various aspects of computer vision and robotics. Here are a few tips that can get you started:

- Refer to websites like <a href="https://paperswithcode.com/">Papers with code</a> to obtain a collated list sorted by accuracy for research papers of computer vision tasks.
- Ensure that object detection/recognition/segmentation models were trained on North American datasets as the data you are given is from the United States.
- Do not worry about inference speed or the number of models used (feel free to use multiple to boost accuracy of a single task) for this project.
- Try to refrain from training your own models unless absolutely essential. The goal here is also to learn how to design a system with pre-trained models and making the algorithm work in context of any problem.
- Sometimes, a very trivial method (such as color thresholding) could be used to solve the problem. Think creatively and do not be bound by how other people solve the problem.
- Here are a few keywords that can aid in finding methods to help solve the problems:
  - **Monocular depth estimation:** Can be utilized to obtain relative scale of objects.
  - **Absolute scale estimation using road markers:** You can use sizes of "known" objects on the roads such as signboards, other cars and so on to normalize scale across frames.
  - **Object Detection and Classification:** Can aid in detecting and sub-classifying objects.
  - **Pose estimation:** Depending on the scenario, you can use this to estimate pose of pedestrians, other objects and/or the motion of your camera.
  - **Optical Flow:** This is how pixels have moved between two image frames. Can help in classifying motion.
- Refer to the project reports in <a href="https://pear.wpi.edu/teaching/rbe549/spring2023studentoutputs.html#p3">2023</a>. Many models has been tested and could be good starts. Some example output are shown below.

<div class="fig fighighlight">
  <img src="/assets/2024/p3/BlenderResult_PedestrianPose.png" width="100%">
  <div class="figcaption">
    Fig 15: Example of vehicle, lane, and pedestrian pose detection.
  </div>
  <div style="clear:both;"></div>
</div>

<div class="fig fighighlight">
  <img src="/assets/2024/p3/BlenderResult_TrafficLight.png" width="100%">
  <div class="figcaption">
    Fig 16: Example of traffic light detection.
  </div>
  <div style="clear:both;"></div>
</div>

<div class="fig fighighlight">
  <img src="/assets/2024/p3/speedbump_detection.png" width="100%">
  <div class="figcaption">
    Fig 17: Example of showing speed bump.
  </div>
  <div style="clear:both;"></div>
</div>


<a name='sub'></a>

## 9. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

<a name='chkpt1sub'></a>

## 9.1. Phase 1 Submission

You're required to submit two sets of things all zipped into a folder called ``GroupGROUPNUM_p3ph1.zip`` on Canvas (The `GROUPNUM` can be found on Canvas) and have a meeting with the course instructors as described below.
1. Rendered images of various cases as ``png/jpg`` images. Feel free to have as many images as you want to show the cases we requested for in Phase 1 (Lanes, Vehicles, Pedestrians, Traffic lights, stop sign). 
2. A small ``.md`` file called ``References.md`` with the packages you used for your implementation along with one sentence of how you used it.
3. Meet the instructors on **(TBD) in UH250E** to discuss your progress and approach.


Note that, you **CAN** use late days for the submission but the instructor meeting time is fixed.


<a name='files'></a>

## 9.1. Phase 2 Submission
You're required to submit two sets of things all zipped into a folder called ``GroupGROUPNUM_p3ph2.zip`` on Canvas (The `GROUPNUM` can be found on Canvas).
1. Some example videos to show the detection performance of the features that are required in Phase 2. (Different videos, Different pose pedestrians, Objects, Road signs, Traffic lights with arrow)
2. A small ``.md`` file called ``References.md`` with the packages you used for your implementation along with one sentence of how you used it.

Note that, you **CAN** use late days for the submission.

### 9.3. Final File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``GroupGROUPNUM_p3.zip``. If your group number is ``4``, then the submission file should be named ``Group4_p2.zip``. The `GROUPNUM` can be found on Canvas. The file **must have the following directory structure**. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>500MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p3.zip
|   ├── Code 
|   |   └── Any subfolders you want along with files
|   └── Videos
|       ├── OutputVisualizationVideoSeq1.mp4
|       ├── ....
|       └── OutputVisualizationVideoSeq13.mp4
├── Report.pdf
├── Presentation.pdf or Presentation.pptx
└── README.md

```

The ``OutputVisualizationVideoSeq1.mp4`` is the output of your rendered visualization for ``Seq1`` (or ``scene1``). You'll have 13 such videos, one for each sequence.

<a name='report'></a>

### 9.3. Final Report

For each section/Phase of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper.
- Present sample visualization examples for each of the cases in every single Phase.
- Talk about the approach you took (as detailed as possible) to solve the problem with appropriate citations and explanations.
- Talk about the problems or corner cases where your approach would not work.
- Talk about how you would make your approach better.
- Talk about challenges you faced during in project and feedback on what can be improved further.


<a name='present'></a>

### 9.4. Final Presentation

You are required to do an in-person presentation for 5 mins (all team members MUST present) during the time decided (look out for a post on timings on <a href="https://piazza.com/wpi/spring2025/rbecs549">Piazza</a>) explaining your approach, the results as video. Explain what all problems you tackled during this project and how you overcame them. Further, talk about non-obvious observations, corner cases and failure modes along with potential ways to solve them. Also, give an in-depth analysis of your proposed approach. The presentation has to be professional of a conference quality presented to a wide range of audience ranging from a lay-person to an expert in the field. 

<a name='funcs'></a>

## 10. Allowed and Disallowed functions

<b> Allowed:

- Absolutely anything in the world!

<b> Disallowed:
<!-- - Absolutely nothing in the world! -->
- For Phase 3, Models that can directly output object motions with video input are not allowed.

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/spring2025/rbecs549).  


<a name='coll'></a>

## 11. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE/CS549 Spring 2025 website](https://pear.wpi.edu/teaching/rbe549/spring2025.html).

<a name='ack'></a>

## 12. Acknowledgments

The beautiful visualizations are from <a href="https://www.tesla.com/">Tesla's</a> products and a lot of the images are adapted from <a href="https://www.notateslaapp.com/tesla-reference/636/all-tesla-fsd-visualizations-and-what-they-mean">here</a>.




