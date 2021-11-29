---
title:  "Space Tracker"
layout: single
date:   2021-11-29 19:06:54
author_profile: true
read_time: true
show_date: true
related: true
header:
  teaser: "https://www.shell.com/inside-energy/smooth-moves-in-space/_jcr_content/par/pageHeader/image.img.960.jpeg/1458555814845/satellite-in-the-rays-of-light.jpeg?imformat=chrome&imwidth=1280"
  image: "https://www.shell.com/inside-energy/smooth-moves-in-space/_jcr_content/par/pageHeader/image.img.960.jpeg/1458555814845/satellite-in-the-rays-of-light.jpeg?imformat=chrome&imwidth=1280"
  og_image: "https://www.shell.com/inside-energy/smooth-moves-in-space/_jcr_content/par/pageHeader/image.img.960.jpeg/1458555814845/satellite-in-the-rays-of-light.jpeg?imformat=chrome&imwidth=1280"
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
Tracking an object has been one of the problems studied for the past several decades.
Usually the "tracker" - the process that performs the tracking, is part of a large system with several sensors.
Such systems can be used for various purposes in both the security and civilian spheres.
Our exposure and desire to create a system of detecting and tracking airborne objects was as part of the "Space Engineering" course of Professor Boaz Ben Moshe, Head of the Department of Computer Science.

![Updated](https://img.shields.io/badge/Updated-2021-green)

## Description



| | |
| --- | --- |
| We have developed a system for detecting and tracking objects such as satellites, drones and meteorological balloons using the NexStar 8SE Telescope.<br>The system is using smart tracking and detecting algorithms via OpenCV and also with some original detection algorithms.<br>The telescope is connected to a computer that controls the telescope to track the object.<br>The algorithms were performed and tested in daylight videos and also night-time videos and real-time videos. | <img width="600" height="300" src="https://github.com/DorAzaria/SpaceTracker/blob/master/README/exp.jpg?raw=true">|
|For the NexStar 8SE Telescope, an API has been given to us by Semyon Pikolov. <br> The user can choose whether to track the object if any detected. <br>If the object is being tracked, the program returns an X, Y point that requires the telescope to move toward the point.| <img width="200" height="200" src="https://sep.yimg.com/ay/yhst-8480297768913/celestron-nexstar-8se-computerized-telescope-11069-4.gif">|

## About the code
For more information, you can check the documentation in the code.

* **main.py** - init the program and choose the right port connection to the telesctope and also can control the speed of telescope's movement.

* **object_tracing.py** - This class manages the whole detecting and tracking algorithm.<br>
    It initially decides whether it is a night / space mode or a day mode.<br>
    After the decision, it sends each frame to the appropriate mode (2 different classes).<br>
    Those classes are detecting objects and when the user decides to track any of the suggest,
    it returns the (X,Y) position of the object, this way, the telescope can also track it.<br>
    This class is also responsible to display the graphics and information on the screen.<br>
    Using history of positions, it can suggest by arrows which direction the object moved if it lost.<br>
    And also zooming in to the tracked object if it fully focused by the telescope, this way it can help
    show small objects in space.<br>
    
 * **day_detection.py** - This class manages the detecting and tracking algorithm in day-light.<br>
    Using smart algorithms of OpenCV and some original methods.<br>
    In each frame, it decides if it is a ground or sky mode, it way
    it can priority the detection by the number of objects in the frame and
    then can work much better and right.<br>
    
* **night_detection.py** - This class manages the detecting and tracking algorithm in night-sky/space.<br>
    Using smart algorithms of OpenCV and some original methods.<br>

* **Telecontrol.py** - This API is used to controll the Nexstar 8SE Telescope from the ground using Python. <br>The main class, all function that enables the telescope control can be found here.

### Algorithms

* **MOG2** - Gaussian Mixture-based Background Segmentation
* **CSRT** - Discriminative Correlation Filter with Channel and 
           Spatial Reliability
           
 **About the main algorithm for sky mode:** <br>
 ```java

 Using background subtraction algorithm of Gaussian Mixture-based Background Segmentation, 
 with a very short history it can detect moving objects.
It generates a BnW frame, the whiter, the faster.
We used another algorithm to detect the whiter object and then analyse it for correctness.
After detecting, we used some limited-data-structures that contains:
1) 'stat' - the position of an object in each frame.
            If the new position of the detected object is around the mean probability of 'stat' list
            then it probably an object that moves logically.
2) 'avg_contours' - the number of total objects we can get in each frame.
                    By probabilistic calculation of variance and mean this list can decide 
                    if the picture is 'loud' by many objects (it happen mostly near the ground).
                    So if the variance and mean is big, the position of the detected object
                    can't go far from the last position it was because there's too many objects.
                    If the variance and mean is small, we can look for a far position of the object 
                    comparing to its last position.
           
When the program detect an object, the user can press specific keys to control the state.
Using smart tracker algorithm of CSRT -(Discriminative Correlation Filter with Channel 
and Spatial Reliability) which is specializing in fast-moving objects.
           
The user can press:
    - 'space' to track a detected object.
    - 'c' to cancel the tracking and move to detection scenario.
    - 'z' to zoom-in to the object.
                
The transition from sky to ground (and vice-versa) is safe while we track an object even though the two  
algorithms are completely different, it is a safe-change between algorithms without losing the object.
```

 **About the main algorithm for ground mode (color detection):** <br>
 ```java
Given a color by the user (RED, YELLOW or ORANGE).
The algorithm detects the object by color.
Using color range of the given color, it detect the most 
stronger color and relevant-sized object.
            
Using HSV color picture we search in each frame the correct color we wish to detect
and then using mask we find the right contours (shapes).
            
When the program detect an object, the user can press specific keys to control the state.
Using smart tracker algorithm of CSRT -(Discriminative Correlation Filter with Channel and 
Spatial Reliability) which is specializing in fast-moving objects.
 ```
 
  **About the main algorithm for ground mode:** <br>
 ```java
Using MOG2 algorithm we can detect the moving objects.
Then we picked the fastest one (which is presented by the strongest white color).
The detect object is displayed to the screen and the user can chose to track it or not.
 ```

  **About the statisticallyTarget method:** <br>
 ```java
This method suggest the size of radius of detecting-search
using the mean of the history list of the number of moving objects we found
in each frame.
If the number is big, then look somewhere near because we can't predict where the object
has lost if there is too many objects around.

If the number is small, then it can search by a big radius for lost the object.

:return an integer value (the radius).
 ```
 
**About the day or night auto-decision method :** <br>
```java
This method decides using color range if the frame is at ground or sky mode.
        
:return True if it's sky mode.
```

## Getting Started

**1.** Clone this repository. <br>
**2.** Connect the telescope with USB cable connetion. <br>
**3.** Change to `telescopeEnabled=True` in the SpaceTracker constructor.<br>
**4.** Insert the port-code to the SpaceTracker constructor in the main.py class.<br>
**5.** Change to `video_path=1`.<br>
**6.** Run main.py .<br>
**7.** If you want to detect ground-objects by color, then insert in the ObjectTracking declerationg True and the name of the color. <br>For example `ObjectTracking(True, 'RED')`.<br>

**Running the program**
```
python main.py
```



### Dependencies

* Python, OpenCV, numPy.
* OS - Windows 10


## Help
For any advise for common problems or issues please PM.


## Authors

[@DorAzaria](https://www.linkedin.com/in/dor-azaria/) <br>
[@InbalCohen](https://github.com/inbalcohen2) <br>
[@SagieHod](https://github.com/sagiehod) <br>

## Acknowledgments

* [OpenCV](https://opencv.org/)
* Professor Boaz Ben Moshe, Head of the Department of Computer Science.
* Arkady Gorodishtser
* Semyon Pikolov


#### Satellites, planes and spacecrafts: <br>

 <img src="https://github.com/DorAzaria/SpaceTracker/blob/master/README/ISS.gif?raw=true">
 
 <img src="https://github.com/DorAzaria/SpaceTracker/blob/master/README/night.gif?raw=true"> 

#### Drone Tracking: <br>

<img  src="https://github.com/DorAzaria/SpaceTracker/blob/master/README/balloon.gif?raw=true">

