# KinectFusion: Real-Time Dense Surface Mapping and Tracking

# Abstract

We present a system for accurate real-time mapping of complex and arbitrary indoor scenes in variable lighting conditions, using only a moving low-cost depth camera and commodity graphics hardware. We fuse all of the depth data streamed from a Kinect sensor into a single global  implicit surface model of the observed scene in real-time.  

 We demonstrate the advantages of tracking against the growing full surface model compared with frame-to-frame tracking, obtaining tracking and mapping results in constant time within room sized scenes with limited drift and high accuracy.

## Conclusion

In this work we have taken a step towards bringing the ability to reconstruct and interact with a 3D environment to the masses. The key concepts in our real-time tracking and mapping system are

- Always up-to-date surface representation fusing all registered data from previous scans using the truncated signed distance function; 

> [Signed distance function](https://en.wikipedia.org/wiki/Signed_distance_function)
>
> -> when passed the coordinates of a point in space, ***return the shortest distance between that point and some surface.***

- Accurate and robust tracking of the camera pose by aligning all depth points with the complete scene model

- Fully parallel algorithms for both tracking and mapping

## 1. Introduction

Research on simultaneous localisation and mapping (SLAM) has focused more on real-time markerless tracking and live scene reconstruction based on the input of a single commodity sensor a monocular RGB camera. 

New depth cameras based either on time-of-flight (ToF) or structured light sensing offer dense measurements of depth in an integrated  device. 

***In this paper we present a detailed method with analysis of what we believe is the first system which permits real-time, dense volumetric reconstruction of complex room-sized scenes using a hand-held Kinect depth sensor.*** 

Users can simply pick up and move a Kinect device to generate a continuously updating,  smooth,  fully fused 3D surface reconstruction. 

> Is it possible by using our sensor?

***Using only depth data, the system continuously tracks the 6 degrees-of-freedom (6DOF) pose of the sensor using all of the live data available from the Kinect sensor rather than an abstracted feature subset, and integrates depth measurements  into a global dense volumetric model.***



## 2. Background

### 2.1 The Kinect Sensor

Kinect is a new and widely-available commodity sensor plat form that incorporates a structured light based depth sensor. In particular, the depth images contain numerous ‘holes’ where no structured-light depth reading was possible. This can be due to certain materials or scene structures which do not reflect infra-red (IR) light,very thin structures or surfaces at glancing incidence angles. 



### 2.2 Drift-Free SLAM for AR

Most SLAM algorithms must be capable of producing self-consistent scene maps and performing drift-free sensor trackingina sequential, real-time fashion. Early SFM algorithms capable of dealing with a large number of images had either tracked camera motion incrementally,  accumulating drift , or required off-line optimisation to close loop



### 2.3 Dense Tracking and Mapping by Scan Alignment

Alongside mapping and tracking work using passive cameras, a line of research has continued using active laser and depth imaging sensors in the fields of robotics and graphics. ***These methods have had at their core the alignment of multiple scans by minimising distance measures between all of the data in each rather than feature extraction and matching.***

Some SLAM algorithms have also made use of depth data alignment and ICP (often referred to in robotics as scan matching), originally in 2D using laser range-finder sensors, to produce effective robot localisation algorithms which can also autonomously map detailed space occupancy in large areas.

> ICP : iterative closest point 
>
> -> **Iterative closest point** (**ICP**) is an algorithm employed to minimize the difference between two clouds of points

 ICP is used to estimate relative robot motion between consecutive poses, which together with loop closure detection and correction can produce large scale metrically consistent maps.



## 2.4 Dense Scene Representations

Dense depth measurements from active sensors, once aligned, can be used to produce fused representations of space which are better than simply overlapping scans. Occupancy mapping has been popular in robotics, and represents space using a grid of cells, within each of which a probability of occupancy is accumulated via Bayesian updates every time a new range scan provides an in-formative observation.

Such non-parametric environment models provide vital free space information together with arbitrary genus surface representation with orientation information, important when physical interaction predictions  are required. 

A related non-parametric representation used in graphics is the signed distance function (SDF).

> [signed distance function](#conclusion)

  Given a SDF representation two main approaches to obtaining a view(rendering) of the surface exist,  and have been extensively studied within the graphics community. 
One option is to extract the connected surfaces using a marching cubes type algorithm, followed by a standard rasterising rendering pipeline. Alternatively the surface can be directly raycast, avoiding the need to visit areas of the function that are out-side the desired view frustum. This is attractive due to the scene complexity-independent nature of the algorithm, a factor that is becoming increasingly important as the requirement for real-time photo-realistic rendering increases.



## 2.5 Dense SLAM with Active Depth Sensing

They conclude that with substantial increases in computational power, it might be possible to instead perform live volumetric SDF fusion. 

With the advent of GPU hardware and our efficient implementation thereon we are able to achieve both these goals in our system. ***The main focus of our work is on reconstruction of larger scale scenes from higher speed camera motions***

This system, while impressive, was targeted at large scale building mapping; the tracking front end was not designed to provide real-time performance needed for useful augmented reality; and the dense modeling (based on a surface patch representation) provides a less refined reconstruction than can be achieved by using a full global fusion approach.