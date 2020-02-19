# Autodrift-Grand-Prix-NUS

This repository contains sample code for competitors of the Autodrift Grand Prix NUS competition.

## Robot Operating System
The implementation in this repo requires the use of the Robot Operating System (ROS1). You can find out more about ROS from their website: http://wiki.ros.org/. While competitors are free to decide on their own choice of software and libraries, and the use of ROS is completely optional, it is nevertheless *highly recommended*.

As of the time of writing, there has yet to be a version of the ROS binary that can be installed on the Raspberry Pi 4 using `apt-get`, so you will need to install ROS from source. A link to a guide containing the instructions is included in the `Useful Links` section below. Of course, if you have other ways of obtaining a version of ROS on the Raspberry Pi, that is fine as well.

## Packages
In the src/ folder, you will find 2 subfolders, the joy package and the drift_racer package. 

### Joy Package
The joy package is a clone of the official ROS joy package, for your convenience. This package will allow you to use the provided Logitech Controller to interface with your robot. Credit to the authors found on the official ROS page: Morgan Quigley, Brian Gerkey, Kevin Watts, Blaise Gassend.

### Drift Racer Package
The drift_racer package, on the other hand, is implemented by us and contains some of our code, for your reference. The `src` folder contains some basic source code that can serve as a starting point, while the `launch` folder contains a launch file that you can use with the ros command `roslaunch`. Do note that the original implementation contains several other scripts that have been omitted.

#### Controlling the Robot Manually
Only 2 nodes need to be implemented to control the robot manually. 

1. joystick_teleop.cpp: for interpreting the instructions provided by the `joy` package and converting it into instructions of type `Twist`. In this implementation, it also controls the state of the robot and arbitrates between autonomous navigation commands and manual controls.

2. actuators_controller.py: receives control messages from the joystick_teleop node and controls the actuators accordingly.

#### Autonomous Navigation
Because the Machine Learning library used requires Python 3, and ROS only supports Python 2, we need some form of communication between ROS and the Machine Learning library. In this implementation, this is achieved using a combination of `TCP socket programming` and `pickle`.

1. drive_agent.py: feeds the live image stream from the camera to the trained neural network, encodes the output of the neural network using `pickle` and sends it over a TCP connection.

2. nav_controller.py: sets up a TCP socket to receive the output of drive_agent, and controls the car appropriately. Note that some parts of the code has been deliberately omitted.

#### Launch File
A sample launch file, `drift_racer.launch` has been included, which will allow the robot to be controlled using the provided Logitech controller. You can use this as a starting point to creating your own launch files, which will surely contain more nodes than that shown.

## Useful Links
1. Install ROS on Raspberry Pi 4: https://www.seeedstudio.com/blog/2019/08/01/installing-ros-melodic-on-raspberry-pi-4-and-rplidar-a1m8/. Credits: Dmitry Maslov, who authored the instructions. Note that you will only need to complete up to step 3 to install ROS.

2. Official ROS Tutorial: http://wiki.ros.org/ROS/Tutorials. While having a good understanding of ROS will prove extremely beneficial, the most important topics to understand well are 3, 4, 5, 6, 8, 12, and 13 of the Beginner Level section.

3. Official ROS joy package page: http://wiki.ros.org/joy.

## Check Your Understanding of ROS
To be able to use ROS well, make sure you understand what following terminology/commands mean and how to use them:

Terminology:
1. nodes
2. topics
3. messages

Commands:
1. catkin_make
2. roscore
3. rosrun
4. roslaunch
5. rostopic
