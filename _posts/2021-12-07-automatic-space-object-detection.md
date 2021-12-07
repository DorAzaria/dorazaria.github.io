---
title:  "Automatic Space Object Detection and Tracking using the NexStar 8SE Telescope"
layout: single
date:   2021-12-07 19:06:54
author_profile: true
read_time: true
show_date: true
related: true
header:
  teaser: "https://images.immediate.co.uk/production/volatile/sites/25/2019/05/Finderscope-0372e80-e1620635277388.jpg?quality=90&resize=620,413"
  image: "https://images.immediate.co.uk/production/volatile/sites/25/2019/05/Finderscope-0372e80-e1620635277388.jpg?quality=90&resize=620,413"
  og_image: "https://images.immediate.co.uk/production/volatile/sites/25/2019/05/Finderscope-0372e80-e1620635277388.jpg?quality=90&resize=620,413"
categories:
  - Space Science
tags:
  - Space
  - Science
  - Python
  - OpenCV
  - Algorithms
comments: true
share: true
related: true
toc: true
toc_sticky: true
usemathjax: true
---
Development of an optical ground station based on a robotic telescope: the idea is to become familiar with the telescope's capabilities to perform experiments of tracking aircraft + space station

[Github project](https://github.com/DorAzaria/SpaceTracker)

[About the code](https://dorazaria.github.io/space%20science/space-tracker/)

## ABSTRACT 
The purpose of the system that monitors airborne targets is to track the movement of objects in the airspace or space, in an area of interest, and thus provide a platform for decision-making to the people or systems responsible for airborne movement in that area.
 As well as to observe the same goals also for private bodies.
The tracking system has to face a number of challenges:
When tracking in daylight there are additional objects and objects in the frame.
When the photo is in motion then you need to estimate the position of the object each time the frame itself moves.
When doing tracking at night and there are several identical flying targets we will want to choose one of which we will want to follow.
In other words, the challenges are - the ability to detect and track the targets in a disturbing environment, accurate examination of the trajectory, correct association of the measurements with the targets when there are multiple discoveries, etc.
Therefore such a system has to deal with limitations and constraints of response times.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image14.png?raw=true)

Typically, a system consists of several stages, each of which is responsible for part of the task:
***In the first stage*** - our goal is to detect with the camera targets in the sky day and night, when these targets are in motion and at a relatively large distance.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image4.png?raw=true)

***In the second stage*** - when our camera manages to identify these targets, and locates their location, it marks the targets for us and then you can press any button (can be anything at the moment with us it is a space on the keyboard) and then choose the target we want to follow, and from that moment begin to follow After the object.
In the project, we focused on following at night - a space station, flashing planes and daylight after balloons and a glider.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image9.png?raw=true)

***In the third stage*** - which is the main task - to follow the target with the telescope when the camera detects and tracks the target and transmits to the control computer the position of the object in the frame and the algorithm moves the telescope together with the camera so that the moving object is in the center.
The camera is attached to a telescope above it.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image11.png?raw=true)

***Using the camera***:
The camera has a wider viewing angle than the telescope so when the target moves it detects and locates the target we want to track and then the tracking operation begins and then it will cause the telescope to focus on the target - after all the telescope
Compared to the camera which sees at a greater distance and therefore can track much farther targets, but the telescope's limitation is the angle of view that follows like a laser so we use a camera that has a wider angle of view.
The main point of our project is to track with the telescope targets as they move away and to be able to identify them in motion and track them.
With the help of our project it will be possible to simulate a ground station that will track a target from the moment it is blown up until it is at a very large distance in the sky, or track planes / space station when the software differentiates between ground and sky and daylight and darkness.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image8.png?raw=true)

## INTRODUCTION
***Tracking goals has been one of the most studied issues for the past several decades.***
Usually the "tracker" (the process that performs the tracking, is part of a large system with several sensors) sometimes of different types.
Such systems can be used for various purposes in both the security and civilian spheres.

***Example of a system from the security field***: a missile defense system. The purpose of this type of system would be to scan the area and try to identify possible targets.
Once the bone has been identified it is necessary to monitor its movement in order to decide whether it poses a threat, i.e. whether it is moving to the protected area or not. If you decide to intercept the threatening object then launch an interceptor missile. This missile needs to change its direction according to the target movements and therefore is currently required to track two objects:

***One - the original goal***:
 it is necessary to constantly estimate its future location, on the other hand it is necessary to monitor the interception missile as well: it is necessary to know its current location in order to estimate the optimal motion vector and give command to the engines. Tracking accuracy on the one hand, and speed of response to changes on the other hand, are the conditions for the success of the interception, when it is clear that failure in the interception mission can come at a very high price.
 
***The problematic nature of this process begins with sensors***: each type of sensor has performance limitations that are reflected in the detection range, measurement accuracy, data transfer rate, false alarms, etc.
The problem becomes more difficult when it comes to several sensors, and especially when these sensors are of different types - it is necessary to fuse information coming from the sensors and process them together.

***There is a dilemma for the tracker***: too few reports from the sensors will result in inaccuracy in tracking, while the multiplicity of reports will cause information to overflow.

***The problem becomes more complicated when it comes to a number of threats***: there are a number of objects that threaten the protected area and a number of missiles are needed to intercept them. In this case the complexity of the whole process increases, there are more reports, more goals, more calculations and there is still a demand for fast enough response times on the one hand, and a sufficiently accurate position evaluation on the other.

## PROJECT’S PURPOSE
Our exposure and desire to create a system of identifying and tracking airborne targets was as part of the "Space Engineering" course of Professor Boaz Ben Moshe, Head of the Department of Computer Science.
One of the projects of the Department of Computer Science at Ariel University was to build a satellite and launch it into space.
And after it succeeded, the department wanted to track the next satellite through the telescope we have at the university, and learn through observation on what can be improved / preserved in the construction of the satellite and for that it was necessary to build a tracking system for closer targets in order to actually identify and track targets.
 The smaller telescope, and then transfer it to the larger telescope which is with the same api of the small telescope.
In one of the experiments that took place at the university, balloons blew up and flew a glider, but the tracking algorithms they had, could not track these targets and there were all sorts of problems, and then we realized the complexity of tracking a satellite / distant target in the sky with a telescope.
So we decided that in order to advance the project of our Department of Computer Science to track satellite and space station, we decided to develop a type of optical ground station based on a robotic telescope - a tracking system that deals with different challenges and can detect different objects in motion and can choose one of the objects we want. Follow.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image2.png?raw=true)

***Figure 1***: Example of a tracking system with satellite sensors for STSS missile detection and interception
This illustration, taken from http://missiledefense.wordpress.com, illustrates an extensive and controlled tracking system. Example of a system from the civilian field: an air control system. The purpose of this system is to give an up-to-date and accurate picture of the sky in the area of control responsibility.
The skies of the whole world are divided into different control areas. It is the responsibility of the controller to know each aircraft moving in its area of ​​responsibility, to make sure that it is progressing according to the flight plans, to detect anomalies and to manage the faults that arise. All aircraft are supposed to move in the same lanes, altitudes and speeds defined for them by the controller.
The system should provide the visitor with a picture of the sky, ie the following data: location of the aircraft, its nature (identification data), speed and altitude. From the above data it is possible, on the one hand, to deduce

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image19.jpg?raw=true)

***Figure 2***: Advanced Safari Satellite app reform
In this figure taken from the marked link it appears that there is satellite tracking and it includes a very extended database as well as augmented reality and virtual reality modes with voice commands.
Using green dots to represent each satellite, the orbit status of the app, shown here, easily illustrates the difference between the Earth's low satellite orbits (LEO), such as the International Space Station (inlay), and the much more distant geosynchronous satellites (curved chain of Satellite symbols) used for television broadcasts and other applications. When you select a single satellite, its name and trajectory continue.
There are many other types of tracking that today are used by different bodies for different purposes, since this is a topic that has been very researched and developed over the years.

Our project focuses on this type of tracking because we simulate a type of optical ground station that will track different objects through a robotic telescope, identify different targets and objects while moving in the sky and select the target we want to track.
In addition, we can track these targets from the moment they are blown / flown because our algorithm differentiates between ground and sky.

## PROJECT STRUCTURE
1. First, we launched balloons so we could start creating software that would track daylight targets.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image13.png?raw=true)

2. Next, we took videos of a flashing plane and a relevant ground station so we could tailor the solution to tracking targets at night - like a flashing plane, or space station - which is the main challenge of our project.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image5.png?raw=true)

3. Creating classes for different situations, writing the algorithms, getting to know the openCV library and dealing with identifying the goals and then tracking a particular goal is far away.
4. After writing these algorithms we divided the algorithms into cases:
    a) Daylight tracking:
    * Case A- When the object is in ground position.
    * Case B-When the object is in the sky position.

    b) Tracking at night:
    * The algorithm recognizes the different states and accordingly adapts itself to the same state.
    
    ![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image1.png?raw=true)

5. Familiarity with the structure of the telescope - and the manner in which the two degrees of freedom of the telescope behave.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image7.png?raw=true)

6. Given the control code of the telescope and it has a basic tracking algorithm but does not provide the tracking service so we improved this algorithm to get tracking.

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image6.png?raw=true)

7. After the algorithms work on videos - we saw that the control system works in real time:
We did a number of experiments 
    1) In daylight - we inflated balloons and followed them with the telescope,
    2) We flew a glider that could simulate a very distant target in the sky and we saw that the telescope was following it as well.
    3) At night we tried to detect the space station as it was above us in real time, we followed flashing planes.

## OBJECT’S TRACKING
***Defining the problem***-
In order to track the target it is required to have its collection of locations over time.
The resulting location is not an exact figure, but a probabilistic function.
This method manages to locate and track an object in the ground by passing an object search.
Used in case we do not want to use the color recognition mode.

***About the algorithm***:
Using the MOG2 algorithm we can identify the moving objects.
First to locate all the moving objects and mark them as optional targets:
We have a list that has 100 blank frames
mean- makes an average of the array and returns the value of the average of the whole array,
2 arrays - one array that stores the object tax in each frame and in order to know what the size of the search square will be in order to focus the telescope on the target we want to track.
 And the second array is also 100 and it keeps the position of the object of the last 100 frames, in order to guess whether the object I am focusing on is the same object we chose to focus on in the last 100 frames, if it is not averaged over the last positions
- The one that is the fastest will usually focus on it (about 2 seconds video)

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image3.png?raw=true)

Then we chose the fastest (represented by the strongest white color.
The discovery object is displayed on the screen and the user can choose to follow it or not.
When the program detects an object, the user can press specific keys to control the mode.
Using CSRT's smart tracker algorithm - (discriminating adapter filter with channel and spatial reliability) that specializes in fast moving objects.
The user can click on 'Space' to track an identified object.
 - 'c' to cancel the tracking and switch to a detection scenario.
 The transition from heaven to earth (and vice versa) is safe while we are tracking an object Although the two algorithms are completely different, it is a safe change between the algorithms without losing the object.

Once we have started tracking, you can see on the screen the movement of the telescope by the arrow since the movement of the telescope is only possible after sending position X, Y and the arrows move.
In addition we added a function that prints an exact date and time when we started tracking, when we lost the object, when we left the program and more. In order to get the exact details on the trace.

## ABOUT THE CODE
Explanation of the various departments and algorithms-
There are 2 main classes for the tracking system - algorithms and a telescope
Algorithm class is divided into 3 classes:

***Main class: object_tracking***
This class manages the entire detection and tracking algorithm.
It initially decides whether it is a night / space mode or a day mode (each mode is a class).
After the decision, it sends each frame to the appropriate mode (2 different classes).
These classes detect objects and when the user decides to follow each of the motions, he returns the position (X, Y) of the object, this way the telescope can also follow it.
This department is also responsible for displaying the graphics and information on the screen.
Using a history of locations, it can suggest using arrows which direction the object is moving if it is lost.
And also zooming in on the tracking object if it is fully focused by the telescope so that it can help show small objects in space.
And his specialty is tracking fast moving objects so we chose to use this algorithm that allows us to track flying objects in the sky that are moving fast.
We have different situations when - you can track a target when it is in ground mode by 2 options selected - by the color of the target and then it recognizes very quickly the object or option B according to the object that is in motion


***First: day_detection***
This department manages the daylight detection and tracking algorithm.
Using smart OpenCV algorithms and some original methods.
The first algorithm is the one that recognizes the objects in motion by subtraction and comparison and thus identifies all the objects in motion in the frame, in each frame it decides if it is a ground or sky mode, so it can give priority to identification by number of objects in the frame and then can work much more Good and true. This class has 3 different modes:
Sky mode - tracking an object in motion in the sky.
Ground status - tracking an object in motion in the ground and is divided into 2 identification methods according to user requirements:

***Color Recognition*** - Given a prominent color of the object that the user enters into the program, the algorithm will recognize the object by the color and then track it.
***Colorless Detection*** - The algorithm will identify the object in motion most relevant in the image using subtraction and comparison calculations and the probability ratio of objects.

***Second: night_detection***
This class manages the detection or tracking algorithm at night or in space.
Using smart OpenCV algorithms and some original methods.
The algorithm detects the objects in motion by subtraction and comparison and thus recognizes all the objects in motion in the frame.

***Algorithms***
* ***MOG2*** - Gaussian Mixture-based Background Segmentation
* ***CSRT*** - Discriminative Correlation Filter with Channel and Spatial Reliability


***Hardware requirement***
* Camera, Connects the telescope to the camera,
* Motorized telescope,
* Control computer,
* HDMI capture.

***Software requirements***
* Python: OpenCV

![](https://github.com/DorAzaria/dorazaria.github.io/blob/main/assets/images/space_telescope/image10.jpg?raw=true)

## REFERENCES
* The open university: Concluding work on "Tracking airborne targets using spatial data structures"
* Aircraft tracking system and autonomous transmission of distressed aircraft location
* Space station tracking map
* Tracking Targets - Code in Python

[Github project](https://github.com/DorAzaria/SpaceTracker)
[About the code](https://dorazaria.github.io/space%20science/space-tracker/)
