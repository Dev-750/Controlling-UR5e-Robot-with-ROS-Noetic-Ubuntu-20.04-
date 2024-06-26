# Controlling-UR5e-Robot-with-ROS-Noetic-Ubuntu-20.04-


## 1. Installing ROS Noetic

Follow the Installation Procedure on the ROS Wiki page [here](https://wiki.ros.org/noetic/Installation/Ubuntu). The steps are very straigth forward.

## 2. Installing and Activating MoveIt Package

To run MoveIt, you must first install Catkin and creat a Catkin workspace. After downloading MoveIt, you will need to build the Catkin Workspace with `catkin build`. DO NOT USE `catkin cmake` as this compiles differently from "catkin build". Most issues arrise in the building of the workspace, so take your time and be methodical.

After you have installed the MoveIt package, you will need to configure the Plugin for RViz.

Intructions can be found in the [MoveIt 1 - Noetic Documentation](https://moveit.github.io/moveit_tutorials/doc/getting_started/getting_started.html)

## 3. Download and Run UR5e Model

Follow [steps to install](https://wiki.ros.org/universal_robots) the Universal Robots Package. Make sure that you enter "noetic" in place of the "$ROS_DISTRO" argument for each command that contains it. DO NOT use `cd/path/to/catkin_ws/src`. Here we already made a workspace for moveit, so we will install the package in `~/ws_moveit/src`. The same goes for the `rosdep` commands: use `~/ws_moveit`.

We can now see a folder in `~/ws_moveit/src` labeled "universal_robot". From here, we can navigate to the subfolder "ur5e_moveit_config". In the "launch" folder there are multiple launch files to choose from. We can launch the demo file by entering the terminal and entering 
`roslaunch ur5e_moveit_config demo.launch`. This will open the UR5e model in RViz.

**Command Syntax:** `roslaunch <package_name> <file_name.launch>`


## 4. Installing Drivers onto UR5e Robot



## 5. Robot Programming



## Resources

- [ROS Noetic Wiki (Ubuntu)](https://wiki.ros.org/noetic/Installation/Ubuntu)
- [MoveIt Installation Tutorial (Noetic)](https://moveit.github.io/moveit_tutorials/doc/getting_started/getting_started.html)
- [Universal Robots Package Install](https://wiki.ros.org/universal_robots)
- [UR ROS Drivers](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver)
- [UR Driver Setup Video](https://www.youtube.com/watch?time_continue=106&v=BS6pFmr7_lA&embeds_referring_euri=https%3A%2F%2Fwww.bing.com%2F&embeds_referring_origin=https%3A%2F%2Fwww.bing.com&source_ve_path=MjM4NTE&feature=emb_title)
- [The Construct: ROS Academy](https://www.theconstruct.ai/robotigniteacademy_learnros/ros-courses-library/)
