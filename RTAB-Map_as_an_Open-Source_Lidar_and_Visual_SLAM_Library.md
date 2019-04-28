# RTAB-Map as an Open-Source Lidar and Visual SLAM Library
RTAB-Map as an Open-Source Lidar and Visual SLAM Library for Large-Scale and Long-Term Online Operation

> RTAB-Map : Real-Time Appearance-Based Mapping
>
> Real-time : 정해진 시간 안에 수행

## Abstract

RTAB-Map started as an appearance-based loop closure detection approach with memory management to deal with large-scale and long-term online operation. It then grew to implement ***Simultaneous Localization and Mapping (SLAM) on various robots and mobile platforms.***

we decided to extend RTAB-Map to support both visual and lidar SLAM, providing in one package a tool allowing users to implement and compare a variety of 3D and 2D solutions for a wide range of applications with different robots and sensors.

This paper presents this extended version of RTAB-Map and its use in comparing, both quantitatively and qualitatively, a large selection of popular real-world datasets, outlining strengths and limitations of visual and lidar SLAM configurations from a practical perspective for autonomous navigation applications.



## Conclusion

This paper presents the extended version RTAB-Map, which provides a full integration with ROS to handle
robot's tf, to synchronize RGB-D, stereo, laser scan and point cloud topics, and the ability to generate occupancy grids for all sensors.

> tf : Transform Library

As a result, ***RTAB-Map is now a multi-purpose graph-based SLAM approach that can be used out-of-the-box by novice SLAM users and for prototyping on robot platforms with different sensor configurations and processing capabilities.***

RTAB-Map's flexibility is demonstrated in this paper by making meaningful comparisons between visual and lidar-based SLAM configurations, allowing to analyze which robot sensor configurations, ***is best for indoor autonomous navigation.*** 

RTAB-Map is currently one of the top ROS packages actively used by the community, ***for low-cost SLAM with RGB-D and stereo cameras.***



## Introduction

Initiated in 2009 and released as an open source library in 2013, RTAB-Map has since be extended to a complete graph-based SLAM approach to be used in various setups and applications. As a result, RTAB-Map has evolved into a cross-platform standalone C++ library and a ROS package, driven by practical requirements such as:

- Online processing: output of the SLAM module should be bounded to a maximum delay after
  receiving sensor data
- Robust and low-drift odometry: while loop closure detection can correct most of the odometry drift,
  in real-world scenarios the robot often cannot properly localize itself on the map, either because it is
  exploring new areas or that there is a lack of discriminative features in the environment.

> odometry : 단어 그래도 주행기록계라는 의미로서 엔코더를 통한 회전수화 IMU(관성 측정 장비)로 기울기 등을 측정함으로서 움직이고 있는 사물의 위치를 측정하는 방법.
>
> drift : 이동

During that time, odometry drift should be minimized so that accurate autonomous navigation is still possible until localization can occur, to avoid incorrectly overwriting mapped areas (e.g., incorrectly adding
obstacles in the entrance of a room, making it a closed area for instance).

***Using a mix of proprioceptive (e.g., wheel encoders, inertial measurement units (IMU)) and exteroceptive(external state) sensors would increase robustness to odometry estimation***

- Robust localization : the SLAM approach must be able to recognize when it is revisiting past locations
  (for loop closure detection) to correct the map.
- Practical map generation and exploitation : most popular navigation approaches are based on occupancy grid, and therefore it is beneficial to develop SLAM approaches that can provide 3D or 2D
  occupancy grid out-of-the-box for easy integration.
- Multi-session mapping (*a.k.a. kidnapped robot problem or initial state problem*): ***when turned on, a
  robot does not know its relative position to a previously created map,*** making it impossible to plan
  a path to a previously visited location. To avoid having the robot restart the mapping process to
  zero or localize itself in a previously-built map before initiating mapping, multi-session mapping
  allows the SLAM approach to initialize a new map with its own referential on startup, and when a
  previously visited location is encountered, a transformation between the two maps can be computed.

With the diversity of available SLAM approaches, determining which one to use in relation to a specific
platform and application is a difficult task, mostly because of the absence of comparative analyses between
them. ***SLAM approaches are generally visual-based or lidar-based only, and are benchmarked often on datasets having only a camera or a lidar, but not both,***

***making difficult to have a meaningful comparison between them.***

since RTAB-Map evolved to handle these practical requirements, we decided to further extend RTAB-Map capabilities to compare visual and lidar SLAM configuration for autonomous robot navigation.

***RTAB-Map can be used to implement either a visual SLAM approach, a lidar SLAM approach or a mix of both, which makes it possible to compare different sensor configuration on a real robot.***



## 3. RTAB-Map Description

RTAB-Map is a graph-based SLAM approach that has been integrated in ROS as the rtabmap-ros package
since 2013.

The odometry is an external input to RTABMap, which means that SLAM can also be done using any kind of odometry to use what is appropriate for a given application and robot.

The structure of the map is a graph with nodes and links. After sensor synchronization, the Short-Term Memory(STM) module creates a node memorizing the odometry pose, sensor's raw data and additional information useful for next modules (e.g., visual words for Loop Closure and Proximity Detection, and local occupancy grid for Global Map Assembling).

Nodes are created at a fixed rate "Rtabmap/DetectionRate" set in milliseconds according to how much data created from nodes should overlap each other. ***For example, if the robot is moving fast and sensor range is small, the detection rate should be increased to make sure that data of successive nodes overlap, but setting it too high would unnecessary increase memory usage and computation time***

A link contains a rigid transformation between two nodes. There are three kind of links: ***Neighbor, Loop Closure and Proximity links.***

> rigid : 엄격한

- Neighbor links are added in the STM between consecutive nodes with odometry transformation.
- Loop Closure and Proximity links are added through loop closure detection or proximity detection, respectively.

***All the links are used as constraints for graph optimization.*** When there is a new loop closure or proximity link added to the graph, ***graph optimization propagates the computed error to the whole graph, to decrease odometry drift.*** With the graph optimized, OctoMap, Point Cloud and 2D Occupancy Grid outputs can be assembled and published to external modules.

> rigid :  엄격한

Without memory management, as the graph grows, processing time for modules like Loop Closure and Proximity Detection, Graph Optimization and Global Map Assembling can eventually exceed real-time constraints, i.e., processing time can become greater than the node acquisition cycle time.

Basically, ***RTAB-Map's memory is divided into a Working Memory (WM) and a Long-Term Memory (LTM).*** When a node is transferred to LTM, it is not available anymore for modules inside the WM. ***When RTAB-Map's update time exceeds the fixed time threshold "Rtabmap/TimeThr", some nodes in WM are transferred to LTM to limit the size of the WM and decrease the update time.*** Similarly to the fixed time threshold, ***there is also a memory threshold "Rtabmap/MemoryThr" that can be used to set the maximum number of nodes that WM can hold.***

To determine which nodes to transfer to LTM, a ***weighting mechanism identifies locations that are more important than others***, using heuristics such as the ***longer a location has been observed, the more important it is and therefore should be left in the WM.*** To do so, when creating a new node, STM initializes the node's weight to 0 and compares it visually (deriving a percentage of corresponding visual words) with the last node in the graph. 

***If they are similar (with the percentage of corresponding visual words over the similarity threshold "Mem/RehearsalSimilarity"), the weight of the new node is increased by one plus the weight of the last node.***

The weight of the last node is reset to 0, and the last node is discarded if the robot is not moving to avoid increasing uselessly the graph size. ***When the time or the memory thresholds are reached, the oldest of the smallest weighted nodes are transferred to LTM first.*** When a loop closure happens with a location in the WM, neighbor nodes of this location can be brought back from LTM to WM for more loop closure and proximity detections. As the robot is moving in a previously visited area, it can then remember the past locations incrementally to extend the current assembled map and localize using past locations.