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

During the preliminary phase, you will use a simulation environment based on classical Gazebo
simulator.
This environment corresponds to a farm containing a crop field, a robot you control and various
elements.
The robot is equipped with a hoeing implement to treat the plot and a set of sensors to perceive the
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
The trajectory does not exactly fit the crop rows in the plot. As a result, following perfectly the absolute path will lead to crushing plants of interest
We also provide some ROS nodes:
* `robot_to_world_localisation` to fuse odometry, GNSS and IMU using a Kalman filter
* `path_matching` to compute the lateral and angular deviation to the path
* `path_following` to compute the speed and steering angles to send to the robot

You can modify these programs or use your own solutions.
If you rewrite `path_matching`, you will need to load the provided path file yourself.
This file is written using the [TIARA trajectory
format](https://github.com/Romea/romea-ros-path-tools/blob/main/doc/tiara_format.md).
Some tools are provided in the [`romea_path_tools`](https://github.com/Romea/romea-ros-path-tools)
package to visualize, convert or generate paths in this format using its python library.


### Weeding the agricultural plot

The robot's path crosses a field containing various plants (beans, corn and wheat).
You need to use the weeder to treat the entire plot.
However, you'll need to have a good perception of the crop rows to avoid driving over them because
the lines are not straight.
There is also an obstacle in the ground which requires to lift the implement of the robot.
Collisions with crops and implement will result in penalties to the score. 


### Avoiding obstacles

Obstacles are placed all along the trajectory (except in the field).
Some are static and prevent the robot from following its path.
Some are mobile and move cross the robot path.
There are also moving obstacles that can be represented by humans or other robots.
Thus, you need to use the exteroceptive sensors of the robot and compute an avoidance path in real
time.
The sensors are described in the [robot interface documentation](doc/robot_interface.md).
Collisions with obstacles or any element of the environment will result in penalties to the score.


### Indoor localization

A part of the robot's trajectory passes inside a building.
To best represent reality, we simulate a loss of the GNSS signal when the robot enters the building
by causing a random offset in the estimated position.
For the final phase, the 4DV simulator performs a more realistic GNSS simulation by simulating the
signal bouncing off the walls of the buildings.
You will therefore need to take this degradation into account and try to maintain good localization
by relying on the robot's other sensors.


### Robot control on sloping terrain

Another part of the trajectory passes through a sloped area.
The goal of this part of the challenge is to control the robot so that it stays as close as possible
to the reference trajectory, despite the slippery terrain.
The score will be calculated based on the measurement of the lateral deviation, as measured by the
`path_matching` node.
