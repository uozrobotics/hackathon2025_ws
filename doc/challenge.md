# Scenario of the challenge

The aim of this challenge is to develop a robot control program to perform an agricultural task
autonomously.
It is composed of the following steps:
* follow a path provided by the organization team,
* perform an agricultural task (weeding) in a crops field,
* avoid static and mobile obstacles (objects, humans, vehicles, ditches),
* indoor robot navigation (degraded GNSS signal inside buildings),
* robot control and stability on sloping terrain.
* Implement management

During the preliminary phase, you will use a simulation environment based on _Gazebo Classic_
simulator.
This environment corresponds to a farm containing a crop field, a robot you control and various
elements.
The robot is equipped with a hoeing implement to treat the plot and a set of sensors to perceive the
environment and localize the robot.
All the elements are interfaced using ROS2 topics, services and actions.
The description of this interface [is available here](robot_interface.md).

The final phase of the competition, to be held at FIRA 2025, will replace the Gazebo simulator with
4DV simulator.
The latter will run on a dedicated machine and provide more realistic sensor information.
However, the robot interface will stay the same.

You don't have to develop a solution capable of solving every challenge.
The difficulty level will increase progressively.


## Details of the different steps

### Follow a path

We provide a reference path that the robot have to follow throughout the challenge.
This path passes through the farm plot, a building and a sloping area.
You don't need to follow this path precisely, but you do need to stay within an interval around it.
It is therefore possible to calculate more optimized trajectory to save time and improve your score.
However, if the trajectory leaves the interval, this will result in a penalty on the score.

The path does not follow the crop fields.
As a result, following perfectly the absolute path will lead to crushing plants of interest.
You can generate a more precise path using the survey file provided.
For more information about this file, please refer to [this documentation](plots_surveying.md).

We also provide some ROS nodes:
* `robot_to_world_localisation` to fuse odometry, GNSS and IMU using a Kalman filter
* `path_matching` to compute the lateral and angular deviation to the path
* `path_following` to compute the speed and steering angles to send to the robot

You can modify these programs or use your own solutions.
If you rewrite `path_matching`, you will need to load the provided path file yourself.
This file is written using the [TIARA trajectory
format](https://github.com/Romea/romea-ros-path-tools/blob/main/tiara_format.md).
Some tools are provided in the [`romea_path_tools`](https://github.com/Romea/romea-ros-path-tools)
package to visualize, convert or generate paths in this format using its python library.


### Weeding the agricultural plot

The robot's path crosses a field containing various plants (beans, corn and wheat).
You need to use the weeder to treat the entire plot.
However, you will need to have a good perception of the crop rows to avoid driving over them because
the lines are not straight.
There is also an obstacle in the ground which requires to raise the implement of the robot.
Collisions with crops and implement will result in penalties to the score. 

The localization of the plots is known before starting the simulation by reading [the plots
surveying file](plots_surveying.md).
It allows to know when the implement should be raised or lowered.
It is mandatory to follow the rows in the order specified in the plots surveying file.


### Avoiding obstacles

Obstacles are placed all along the trajectory (except in the field and the sloping area).
Some are static and prevent the robot from following its path.
Some are mobile and move cross the robot path.
There are also moving obstacles that can be represented by humans or other robots.
Thus, you need to use the exteroceptive sensors of the robot and compute an avoidance path in real
time.
The sensors are described in the [robot interface documentation](robot_interface.md).
Collisions with obstacles or any element of the environment will result in penalties to the score.


### Indoor localization

A part of the robot's trajectory passes inside a building.
To best represent reality, we simulate a loss of the GNSS signal when the robot enters the building
by causing a random offset in the estimated position.
For the final phase, the 4DV simulator performs a more realistic GNSS simulation by simulating the
signal bouncing off the walls of the buildings.
You will therefore need to take this degradation into account and try to maintain good localization
by relying on the robot's other sensors.

If you have not implemented this part of the challenge, it is possible to disable it (i.e. use the `main` branch of romea_gps instead of `fira_hackathon2025` in https://github.com/FiraHackathon/hackathon2025_ws/blob/main/docker/repositories#L68), but it will
induce penalties on the score.


### Robot control on sloping terrain

Another part of the trajectory passes through a sloped area.
The goal of this part of the challenge is to control the robot so that it stays as close as possible
to the reference trajectory, despite the slippery terrain.
The score will be calculated based on the measurement of the lateral deviation, as measured by the
`path_matching` node.


## Score calculation

The score calculation is based on:
* the total time to reach the end of the path
* the distance of the robot to the path (except in the crops field)
* the surface covered by the weeder in the field

There is also different source of penalties:
* crushing crops
* colliding with obstacles
* leaving the area around the reference path
