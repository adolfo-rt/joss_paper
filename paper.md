---
title: 'ros_control: A generic and simple control framework for ROS'
tags:
  - ros
  - control
  - framework
  - robotics
  - robot control
  - manipulation
  - navigation
  - mobile robotics
  - hardware abstraction layer  
authors:
 - name: Sachin Chitta
   orcid: 0000-0003-4859-6096
   affiliation: 9, 11
 - name: Eitan Marder-Eppstein
   orcid: 0000-0003-0940-7117
   affiliation: 1
 - name: Wim Meeussen
   orcid: 0000-0003-1905-4001
   affiliation: 1
 - name: Vijay Pradeep
   orcid: 0000-0002-8280-7275
   affiliation: 1
 - name: Adolfo Rodríguez Tsouroukdissian
   orcid: 0000-0002-3075-1329
   affiliation: 2
 - name: Jonathan Bohren
   orcid: 0000-0003-4568-6352
   affiliation: 8,10
 - name: David Coleman
   orcid: 0000-0001-6112-1795
   affiliation: 3, 11
 - name: Bence Magyar
   orcid: 0000-0001-8455-8674
   affiliation: 4, 2
 - name: Gennaro Raiola
   orcid: 0000-0003-1481-1106
   affiliation: 5, 2
 - name: Mathias Lüdtke
   orcid: 0000-0002-1835-3854
   affiliation: 6
 - name: Enrique Fernandez Perdomo
   orcid: 0000-0002-4881-0497
   affiliation: 7, 2
affiliations:
 - name: hiDOF, Inc. (at the time of this work)
   index: 1
 - name: PAL Robotics (at the time of this work)
   index: 2
 - name: PickNik Consulting
   index: 3
 - name: Heriot-Watt University, Edinburgh Centre for Robotics
   index: 4
 - name: Department of Advanced Robotics, Istituto Italiano di Tecnologia (IIT)
   index: 5
 - name: Fraunhofer IPA
   index: 6
 - name: Clearpath Robotics
   index: 7
 - name: Honeybee Robotics
   index: 8
 - name: Kinema Systems Inc.
   index: 9
 - name: John Hopkins University
   index: 10
 - name: Willow Garage Inc. (at the time of this work)
   index: 11
date: 31 October 2017
bibliography: paper.bib
---
# Summary

In recent years the Robotics Operating System [@quigley2009ros] (ROS) has become the 'de facto' standard framework for the development of software in robotics. The idea of `ros_control` originates from the PR2 robot's `pr2_controller_manager` framework with some ideas borrowed from OROCOS [@bruyninckx2001open]. The `ros_control` framework provides the capability to implement and manage robot controllers with a focus on both _real-time performance_ and _sharing of controllers_ in a robot-agnostic way. 
The clear, modular design of `ros_control` makes it ideal for both research and industrial use and has indeed seen many such applications to date. `ros_control` is out-of-the-box compatible with 3rd party software such as `MoveIt!` [@moveit],  the `ROS navigation stack`, `Gazebo`[@koenig2004design] and others.

The backbone of the framework is the Hardware Abstraction Layer, which serves as a bridge to different simulated and real robots. This abstraction is provided by the `hardware_interface::RobotHW` class; specific robot implementations have to inherit from this class.  Instances of this class are then used to interface the robot hardware (including some low-level sensors) to higher-level controllers. This allows higher-level controllers to be hardware-agnostic and easily shareable.

![ROS Control overview](images/ros_control_overview.png) 

There is a possibility for composing already implemented `RobotHW` instances through the `CombinedRobotHW` class. The latter is ideal for constructing control systems for robots where parts come from different suppliers, each supplying their own specific `RobotHW` instance. The rest of the `hardware_interface` package defines read-only or read-write typed joint and actuator interfaces for abstracting hardware away. Through these typed interfaces this abstraction enables easy introspection and increases maintainability.

The `controller_manager` is responsible for managing the lifecycle of controllers, and hardware resources through the interfaces and handling resource conflicts between controllers. It provides a standard `ROS service`-based interface for controller lifecycle management and queries.

![Overview](images/overview.png)

Furthermore, `ros_control` ships software libraries addressing real-time ROS communication, transmissions and joint limits. The `realtime_tools` library adds utility classes handling ROS communications in a realtime-safe way. The `transmission_interface` package supplies classes implementing joint- and actuator-space conversions such as: simple reducer, four-bar linkage and differential transmissions. A declarative definition of transmissions is supported directly within the robot's URDF [@garage2009universal].  The `joint_limits_interface` package contains data structures for representing joint limits, methods to populate them through URDF or yaml files and methods to enforce these limits. `control_toolbox` offers components useful when writing controllers: a PID controller class, smoothers, sine-wave and noise generators. 

The repository `ros_controllers` holds several ready-made controllers supporting the most common use-cases for manipulators, mobile and humanoid robots, e.g. a `joint_trajectory_controller` is heavily used with position-controlled robots to interface with MoveIt!.

Finally, `control_msgs` provides ROS messages used in most controllers offered in `ros_controllers`.

`ros_control` was conceptualized by Sachin Chitta at Willow Garage Inc. and initial design and implementation was done by Sachin Chitta (then at Willow Garage), Wim Meussen, Vijay Pradeep and Eitan Marder-Epstein (then at HiDOF) before being released open-source.

`ros_control` is released as binary packages with each new version of ROS, source code is hosted at the [ros-controls](https://github.com/ros-controls) Github organization. Documentation on behaviour, interfaces, doxygen-generated pages and tutorials can be found at [ros_control](http://wiki.ros.org/ros_control) and [ros_controllers](http://wiki.ros.org/ros_controllers). For a thorough presentation we invite the interested reader to watch the talk given at ROSCon2014 [@rodriguez2014roscon].

# Robots using `ros_control`

Being a mature framework, `ros_control` is widely applied to both production and research platform robots. A few examples where the control system is implemented with `ros_control` are:
- PAL Robotics' humanoid, biped and mobile robots: REEM, REEM-C, PMB2, Tiago and Talos [@stasse2017talos] 
- NASA's humanoid and biped robots: Valkyrie & Robonaut [@radford2015valkyrie, @hart2014robot, @badger2016ros]
- Universal Robots' industrial arms: UR3, UR5 [@andersen2015optimizing]
- Clearpath Robotics' outdoor mobile robots: Grizzly, Husky, Jackal [@cpr2017roscontrol], and OTTO Motors' industrial indoor mobile robots: OTTO 1500, OTTO 100
- Shadow Robot's anthropomorphic, highly sensorized and precise Shadow Hand [@meier2016distinguishing]
- The quadruped robot HyQ [@semini11hyqdesign] at Istituto Italiano di Tecnologia
- The "Twil" robot at Federal University of Rio Grande do Sul [@lages2017parametric]

# References
