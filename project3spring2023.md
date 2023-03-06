---
layout: page
mathjax: true
title: EinsteinVision
permalink: /spring2023/proj/p3/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Problem Statement](#prob)
- [3. Data Collection](#data)
- [4. Checkpoint 1: Basic Features](#ckpt1)
- [5. Checkpoint 2: Advanced Features](#ckpt2)
- [6. Checkpoint 3: Bells and Whistles](#ckpt3)
- [7. Extra Credit: Cherry on Top](#extra)
- [8. Hints, Tips and Tricks](#hints)
- [9. Submission Guidelines](#sub)
  - [9.1. File tree and naming](#files)
  - [9.2. Report](#report)
  - [9.3. Presentation](#present)
- [10. Allowed and Disallowed functions](#funcs)
- [11. Collaboration Policy](#coll)
- [12
. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, April 01, 2023.** This project is to be done in groups of 2 and has a 10 min presentation. Various checkpoints are due at different dates and are given below:
- Checkpoint 1 is due on **11:59:59 PM, March 24, 2023.**
- Checkpoints 2 and 3 (and extra credit) are due on **11:59:59 PM, April 01, 2023.**

<a name='prob'></a>
## 2. Problem Statement 
A key component of any product which a human has to interact with are beautiful visualizations. This is the first step in a human building a trust with an autonomous machine. Think for example of your first smartphone and the UI it had versus how it looks now. This not only makes things easier to use but also more efficient and intuitive. To recap, our visualization takes in sensory information and provide insights into how the robot is thinking. Visualizations are not only important for debugging the issues in the software but they are also a basic Human Robot Interaction (HRI) problem as we mentioned before. One such classic visualization is the infamous `rviz` as shown in Fig. 1.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/rviz.png" width="100%">
  <div class="figcaption">
    Fig 1: RViz Visualization of sensor data on a self-driving car.
  </div>
  <div style="clear:both;"></div>
</div>

Another example of this looks in a Tesla car is shown in Fig. 2.

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavisionrudimentary.png" width="100%">
  <div class="figcaption">
    Fig 2: Tesla's backend visulization in autonomous mode.
  </div>
  <div style="clear:both;"></div>
</div>


Both rviz and Tesla's earlier dashboards fail at the HRI problem, rendering (pun intended) it useless for common usage.
 
Tesla's recent dashboard (Fig. 3) is a really good example of visualization which not only provides a lot of insights about information it perceives, it is intuitive and interactive. 

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslavision.png" width="100%">
  <div class="figcaption">
    Fig 3: Tesla's frontend visulization in autonomous mode.
  </div>
  <div style="clear:both;"></div>
</div>


In this project, you will build an visualization inspired and bettered version of Tesla's dashboard (see Vid. 1 and Fig. 3) and hence the name 😉, wherein you are provided with a set of videos recorded from cameras of a 2023 Tesla Model S (shown in Vid. 1). You are required to output a rendered video of your visualizations: atleast showing the view in front of the car and your car, you can also show everything around the car. You are free to use any approach (deep learning based or classical) available in the world and render the visualizations using <a href="https://www.blender.org/">Blender</a>. This project is open-ended and hence there are multiple right solutions. You are graded for your creativity in approaching the problem, the effectiveness and prettiness of the visualizations. 

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
## 3. Dataset
- Models
- Videos
- Calibration


<a name='ckpt1'></a>
## 4. Checkpoint 1: Basic Features
In this checkpoint, you are required to implement basic features which are absolutely essential for a self-driving car. We will describe these features next:

1. **Lanes:** Identify and show the different kinds of lanes on the road, they could be dashed, solid and/or of different color (white and yellow). Each lane has significance and are essential to identify (See Fig. 5).
2. **Vehicles:** Here, identify all cars (but you do not need to classify as different types) and represent them as a car shape (See Fig. 5, all your cars will look the same).
3. **Pedestrians:**  You need identify and locate pedestrians in the scene (See Figs. 5 and 6, all your pedestrians will look the same, i.e., they will face the same way).
4. **Traffic lights:** Indicate the traffic signals and it’s color (See Fig. 7, note that you do not need to classify arrows in the traffic signals here).
5. **Road signs:** There are sign boards on the road you need identify and represent. In this checkpoint, you need to primarily indicate stop signs (Fig. 6) and speed limit signs (Fig. 8). 


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
## 5. Checkpoint 2: Advanced Features
In this checkpoint, you will build on top of the previous one by enhancing and adding more features. Here, we add more granularity to the vision system which can aid in algorithmic decisions in navigation modules.  


1. **Vehicles:** Here, you need to classify (identify different vehicles) and subclassify them (identify different kinds of a type of vehicle). Note that you have to display these detections as the respective 3D model in your renders (See Fig. 9). More particularly, 
  - Cars: Sedan, SUV, hatchback, Pickup trucks
  - Trucks
  - Bicycle
  - Motorcycle
2. **Traffic lights:** Additionally to the previous checkpoint, classify arrows on the traffic lights here (Fig. 7).
3. **Road signs:** Along with the previously mentioned sign boards, you should also indicate road signs on the ground such as arrows (See Fig. 6).
4. **Objects:** You also need to indicate additional objects like dustbins, traffic poles, traffic cones and traffic cylinders as their respective 3D models in the renders (See Fig. 10). 


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
## 6. Checkpoint 3: Bells and Whistles

In this checkpoint, we try to add further cognitive abilities for better decision making in our planning stage.

1. **Break lights and indicators of the other vehicles:** Identify and display the vehicle break lights and indicator signals (See Fig. 11). This helps the navigation module in making better lane changing decisions. 
2. **Pedestrian pose:** You need to identify pedestrian pose each frame instead of just classifying them and display them (See Fig. 6).
3. **Parked and Moving Vehicles:** Distiguish between parked and moving vehicles and display (make it subtle but identifiable). 


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaindicator.jpg" width="100%">
  <div class="figcaption">
    Fig 11: Visualization showing brake lights and indicators.
  </div>
  <div style="clear:both;"></div>
</div>



<a name='extra'></a>
## 7. Extra Credit: Cherry on Top
Implementing the extra credit can give you upto 25% of bonus score. Like a cherry on the top, you need to identify and indicate:

1. **Speed bumps:** (See Fig. 12) This accounts for 10% of bonus score.
2. **Collision prediction of pedestrians or other vehicles as red highlight:**  (See Fig. 13) This accounts for 15% of bonus score

<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslaspeedbump.jpg" width="100%">
  <div class="figcaption">
    Fig 12: Visualization of speed bumps.
  </div>
  <div style="clear:both;"></div>
</div>


<div class="fig fighighlight">
  <img src="/assets/2023/p3/teslapedcoll.jpg" width="100%">
  <div class="figcaption">
    Fig 13: Visualization of collision of the pedestrian (left) and car (right).
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
- Sometimes, a very trivial method (such as color thresolding) could be used to solve the problem. Think creatively and do not be bound by how other people solve the problem.
- Here are a few keywords that can aid in finding methods to help solve the problems:
  - **Monocular depth estimation:** Can be utilized to obtain relative scale of objects.
  - **Absolute scale estimation using road markers:** You can use sizes of "known" objects on the roads such as signboards, other cars and so on to normalize scale across frames.
  - **Object Detection and Classification:** Can aid in detecting and sub-classifying objects.
  - **Pose estimation:** Depending on the scenario, you can use this to estimate pose of pedestrians, other objects and/or the motion of your camera.
  - **Optical Flow:** This is how pixels have moved between two image frames. Can help in classifying motion.


<a name='sub'></a>

## 9. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

<a name='files'></a>

### 9.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``YourDirectoryID_p3.zip``. If you email ID is ``abc@wpi.edu``, then your ``DirectoryID`` is ``abc``. For our example, the submission file should be named ``abc_p3.zip``. The file **must have the following directory structure**. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Please <b>DO NOT</b> include data in your submission. Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>100MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
YourDirectoryID_p3.zip
|   ├── Code 
|   |   └── Any subfolders you want along with files
|   └── Videos
|       └── OutputVisualizationVideos.mp4
├── Report.pdf
├── Presentation.pdf or Presentation.pptx
└── README.md

```

<a name='report'></a>

### 9.2. Report

For each section/checkpoint of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper.
- Present sample visualization examples for each of the cases in every single checkpoint.
- Talk about the approach you took (as detailed as possible) to solve the problem with appropriate citations and explanations.
- Talk about the problems or corner cases where your approach would not work.
- Talk about how you would make your approach better.
- Talk about challenges you faced during in project and feedback on what can be improved further.


<a name='present'></a>

### 9.3. Presentation

You are required to do an in-person presentation for 10mins (all team members MUST present) during the time decided (look out for a post on timings on <a href="https://piazza.com/wpi/spring2023/rbecs549">Piazza</a>) explaining your approach, the results as video. Explain what all problems you tackled during this project and how you overcame them. Further, talk about non-obvious observations, corner cases and failure modes along with potential ways to solve them. Also, give an in-depth analysis of your proposed approach. The presentation has to be professional of a conference quality presented to a wide range of audience ranging from a lay-person to an expert in the field. 

<a name='funcs'></a>

## 10. Allowed and Disallowed functions

<b> Allowed:

- Absolutely anything in the world!

<b> Disallowed:
- Absolutely nothing in the world!

If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/spring2023/rbecs549).  


<a name='coll'></a>

## 11. Collaboration Policy
<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE/CS549 Spring 2023 website](https://nitinjsanket.github.io/teaching/rbe549/spring2023.html).

<a name='ack'></a>

## 12. Acknowledgements

The beautiful visualizations are from <a href="https://www.tesla.com/">Tesla's</a> products and a lot of the images are adapted from <a href="https://www.notateslaapp.com/tesla-reference/636/all-tesla-fsd-visualizations-and-what-they-mean">here</a>.
