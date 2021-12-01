---
title:  "Starlink Satellite Orbital Simulation  using Systems Tool Kit (STK)"
layout: single
date:   2021-12-01 11:06:54
author_profile: true
read_time: true
show_date: true
related: true
header:
  teaser: "https://thumbor.forbes.com/thumbor/960x0/https%3A%2F%2Fblogs-images.forbes.com%2Fjonathanocallaghan%2Ffiles%2F2019%2F05%2FStarlink-main-1200x696.jpg"
  image: "https://thumbor.forbes.com/thumbor/960x0/https%3A%2F%2Fblogs-images.forbes.com%2Fjonathanocallaghan%2Ffiles%2F2019%2F05%2FStarlink-main-1200x696.jpg"
  og_image: "https://thumbor.forbes.com/thumbor/960x0/https%3A%2F%2Fblogs-images.forbes.com%2Fjonathanocallaghan%2Ffiles%2F2019%2F05%2FStarlink-main-1200x696.jpg"
categories:
  - Space Science
tags:
  - Space
  - Science
comments: true
share: true
related: true
toc: true
toc_sticky: true
usemathjax: true
---

This project is intended to simplify a complex and physical problem of calculating Starlink satellite orbits by a representation of dynamic graphs and simple data charts.

We will make a data report and a graph of results showing the connection time between Starlink satellites and the ground-station (at Ariel University).
If the distance between the satellite and the ground station is within the reception range then communication can take place.


The final product will be a report and data graph that shows the entry and exit times to the contact space given some dates.
The problen was solved by using STK software with an illustration solution for the project theme.


## Starlink
If it's rich, smart and out of this world then it's probably Elon Musk.
Starlink is a satellite internet constellation operated by SpaceX  providing satellite Internet access to most of the Earth.
The goal of Starlink is to develop a series of 42,000 satellites, which will provide a new Internet communications system from space.
Starlink's satellites will at the end of the process contain thousands of small satellites that will orbit the Earth in the LEO orbit, work in conjunction with terrestrial transmitters, and provide broadband Internet connection.
In the future, with the help of the Starlink project, a satellite telephone network will also be established.

#### Benefits of Starlink
* Download speeds of 1Gb/s (once all satellites are launched).
* Greatly reduces the distance signals needed for network traffic (low delay).
* Allows internet access for poor countries.


## Starlink VS 5G


> Americans want better home internet options. We've seen that in survey after survey. A pair of recent surveys show with more details that people don't think they're going to be able to get those better options from 5G—but respondents think they'll get them from Elon Musk's satellite ISP, Starlink.
  
> <cite><a href="https://www.pcmag.com/news/surveys-more-americans-want-starlink-than-5g-home-internet">Sascha Segan from PCmag</a></cite>


![](https://i.pcmag.com/imagery/articles/019uYAoeKueqrCTZhTRqAAl-5.fit_lim.size_1600x900.v1613064798.png)


> So who's going to step up with an alternative to the currently dominant cable systems? A survey from AllConnect.com (a site that compares broadband plans) has relatively bleak news for promoters of 5G home internet. Only 17.77% of Americans have or want 5G home internet as an alternative to their current service, according to the survey. That number is far short of the 51% who want to switch to the satellite-based Starlink service, as was found in an earlier survey from Reviews.org.
(From PCmag).
  
> <cite><a href="https://www.pcmag.com/news/surveys-more-americans-want-starlink-than-5g-home-internet">Sascha Segan from PCmag</a></cite>


![](https://i.pcmag.com/imagery/articles/019uYAoeKueqrCTZhTRqAAl-3.fit_lim.size_960x.png)

## Back to the problem
How will we process the entry times of Starlink satellites into the reception area with the Ariel University area (ground station) in a fast, efficient and accessible way?
Orbit calculations can be very difficult, especially when working with formulas and real numbers.
The solution - STK.

## The solution
Systems Tool Kit (commonly known as STK for short) is a software package developed by the company (Analytical Graphics Inc) (American AGI) that enables the design and execution of complex dynamic simulations of real-world events.
The software has become a major tool for planning and managing space missions, and is used by many space agencies, including NASA, the European, Russian, French and other space agencies.
The software is a major tool for planning space missions in many companies involved in building satellites, spacecraft and launchers.

![](https://downloads.agi.com/u/images/stk-logo.png)

In STK software it is possible to create static and dynamic objects that represent communication bodies in space and on the ground.
We can create a Sensor object (Ground station) and define its abilities, including the angle of reception, the range of width, height or any desired direction.
Each satellite that passes within the sensor's reception range will be detected, thus documenting future, past and present cases of satellites passing within Ariel's reception range and then easiliy extracting a detailed report from it in a few seconds.


## Two Line Element (TLE)
We are in big trouble: how do we create 1600 (and more) satellite objects in the software?

Using TLE (Two Line Element - a data format that encodes a list of orbital elements of objects orbiting the Earth at a given point in time, called the Epoch.
Using an appropriate prediction formula, the situation (location and velocity) at any point in the past or future can be accurately estimated.
TLEs can describe the orbits only of objects orbiting the earth.
 
With the help of the CelesTrak website that provides reliable TLE files (directly from SpaceX) it is possible to download an updated file of all Starlink satellites.

![](http://kmenezes.github.io/ARO-Tracking-Software/docs/TLE.jpg)


## The final report 
Starlink satellite entry and exit report.
Just a short part from the file:


```

Loaded 1382 satellites
Sensor Location -> latitude: 32.388018 , longitude: 34.991058 
Time -> from 2021/4/24 to 2021/4/26

STARLINK-24
--------------------------------------
Access          Start Time                    Stop Time           Duration (sec)
  1       2021 Apr 24 01:33:40        2021 Apr 24 01:36:46          186.0
  2       2021 Apr 24 09:55:16        2021 Apr 24 09:56:24          68.0
  3       2021 Apr 25 01:18:36        2021 Apr 25 01:21:18          162.0

STARLINK-61
--------------------------------------
Access          Start Time                    Stop Time           Duration (sec)
  1       2021 Apr 24 00:20:24        2021 Apr 24 00:21:51          87.0
  2       2021 Apr 24 07:00:42        2021 Apr 24 07:02:12          90.0
  3       2021 Apr 25 00:00:01        2021 Apr 25 00:01:01          60.0
  4       2021 Apr 25 06:39:56        2021 Apr 25 06:41:32          96.0
  5       2021 Apr 25 23:39:33        2021 Apr 25 23:40:09          36.0

STARLINK-71
--------------------------------------
Access          Start Time                    Stop Time           Duration (sec)
  1       2021 Apr 24 01:02:05        2021 Apr 24 01:05:28          203.0
  2       2021 Apr 24 09:22:01        2021 Apr 24 09:25:40          219.0
  3       2021 Apr 25 00:44:53        2021 Apr 25 00:48:22          209.0
  4       2021 Apr 25 09:04:53        2021 Apr 25 09:08:31          218.0

STARLINK-43
--------------------------------------
Access          Start Time                    Stop Time           Duration (sec)
  1       2021 Apr 24 02:41:00        2021 Apr 24 02:44:08          188.0
  2       2021 Apr 24 17:49:46        2021 Apr 24 17:51:45          119.0
  3       2021 Apr 25 02:01:30        2021 Apr 25 02:04:09          159.0

```



<iframe width="560" height="315" src="https://www.youtube.com/embed/u1sc3__h348" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

