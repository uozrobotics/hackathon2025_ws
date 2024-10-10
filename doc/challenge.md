# Scenario of the challenge

The aim of this challenge is to develop a robot control program to perform an agricultural task
autonomously.
It is composed of the following steps:
* follow a path provided by the organization team,
* perform an agricultural task (weeding) in a crops field,
* avoid static and mobile obstacles (object, humans, vehicles, small passage),
* indoor robot control (degraded GNSS signal inside buildings),
* robot control on sloping terrain.

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

You don't have to develop a solution capable of solving every challenge.
In fact, you can pilot the robot manually whenever you like.
However, there will be a penalty when calculating the score.


## Details of the different steps

### Follow a path

We provide the path that the robot have to follow throughout the challenge.
This path passes through the farm plot, a building and a sloping area.
The trajectory is not perfect, however, as it does not follow the rows of plants in the plot
exactly.
We also provide some ROS nodes:
* `robot_to_world_localisation` to fuse odometry, GNSS and IMU using a Kalman filter
* `path_matching` to compute the lateral and angular deviation to the path
* `path_following` to compute the speed and steering angles to send to the robot

You can modify these programs or use your own solutions.
If you rewrite `path_matching`, you will need to load the provided path file yourself.
This file is written using the [TIARA trajectory
format](https://github.com/Romea/romea-ros-path-tools/blob/main/doc/tiara_format.md).
Some tools are provided in the [`romea_path_tools`](https://github.com/Romea/romea-ros-path-tools)
package to visualize, convert or easily generate paths in this format using its python library.


### Weeding the agricultural plot

The robot's path crosses a field containing various plants (beans, corn and wheat).
You need to use the weeder to treat the entire plot.
However, you'll need to have a good perception of the crop rows to avoid driving over them because
the lines are not straight.
There is also an obstacle in the ground which requires to lift the implement of the robot.
Collisions with crops and implement will cause a malus in the score. 

### Avoiding obstacles

### Indoor localization

### Robot control on slopping terrain
