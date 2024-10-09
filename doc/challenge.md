# Scenario of the challenge

The aim of this challenge is to develop a robot control program to perform an agricultural task
autonomously.
It is composed of the following steps:
* follow a path provided by the organization team,
* perform an agricultural task (weeding) in a crops field,
* avoid static and mobile obstacles (object, humans, vehicles, small passage),
* control a robot indoor (degraded GNSS signal),
* precise robot control on slopping terrain.

During the preliminary phase, you will use a simulation environment based on Gazebo Classic
simulator.
This environment corresponds to a farm containing a crops field, a robot you control and various
elements.
The robot is equipped with a hoeing system to treat the plot and a set of sensors to perceive the
environment and localize the robot.
All the elements are interfaced using ROS2 topics, services and actions.
The description of this interface [is available here](doc/robot_interface.md).

The final phase of the competition, to be held at FIRA 2025, will replace the Gazebo simulator with
4DV simulator.
The latter will run on a dedicated machine and provide more realistic sensor information.
However, the robot interface will stay the same.


## Details of the different steps

### Follow a path

### Weeding the agricultural plot

### Avoiding obstacles

### Indoor localization

### Robot control on slopping terrain
