# Controlling-UR5e-Robot-with-ROS-Noetic-Ubuntu-20.04-


## 1. Installing ROS Noetic

Follow the Installation Procedure on the ROS Wiki page [here](https://wiki.ros.org/noetic/Installation/Ubuntu). The steps are very straigth forward.

## 2. Installing and Activating MoveIt Package

To run MoveIt, you must first install Catkin and creat a Catkin workspace. After downloading MoveIt, you will need to build the Catkin Workspace with `catkin build`. DO NOT USE `catkin cmake` as this compiles differently from "catkin build". Most issues arrise in the building of the workspace, so take your time and be methodical.

After you have installed the MoveIt package, you will need to configure the Plugin for RViz.

Intructions can be found in the [MoveIt 1 - Noetic Documentation](https://moveit.github.io/moveit_tutorials/doc/getting_started/getting_started.html)

## 3. Download and Run UR5e Model

Follow [steps to install](https://wiki.ros.org/universal_robots) the Universal Robots Package. Make sure that you enter `noetic` in place of the `$ROS_DISTRO` argument for each command that contains it. DO NOT use `cd/path/to/catkin_ws/src`. Here we have already made a workspace for moveit, so we will install the package in `~/ws_moveit/src`, and the same goes for the `rosdep` commands: use `~/ws_moveit`.

We can now see a folder in `~/ws_moveit/src` labeled "universal_robot". From here, we can navigate to the subfolder "ur5e_moveit_config". In the "launch" folder there are multiple launch files to choose from. We can launch the demo file by entering the terminal and entering 
`roslaunch ur5e_moveit_config demo.launch`. This will open the UR5e model in RViz.

**Command Syntax:** `roslaunch <package_name> <file_name.launch>`


## 4. Installing URCap Driver onto UR5e Robot

**Note**: For installing this URCap a minimal **PolyScope version of 5.1** is necessary.

1. Save the **externalcontrol-x.x.x.urcap** which can be found [here](https://github.com/UniversalRobots/Universal_Robots_ExternalControl_URCap/releases) [^1] to a **USB stick**.

[^1]: *For the URCap used in this project, check the resources folder of the repository.* 

3. Plug in the USB to the **Control Box** and copy the file to the robots **program** folder.

4. On the welcome screen click on the hamburger menu in the top-right corner and select **Settings** to enter the robot's setup. There select **System** and then **URCaps** to enter the URCaps installation screen.

5. Click the little **plus** sign at the bottom to open the file selector. There you should see all urcap files stored inside the robot's programs folder or a plugged in USB drive. Select and open the **externalcontrol-x.x.x.urcap** file and click **open**. Your URCaps view should now show the **External Control** in the list of active URCaps and a notification to **restart** the robot. Do that now.
   
![External Control View](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/raw/master/ur_robot_driver/doc/initial_setup_images/es_05_urcaps_installed.png)
 
5. After the reboot you should find the **External Control** URCap inside the **Installation** section. For this, select the **Installation tab** and select **External Control** from the list.
    
6. Here you'll have to setup the **IP address** of the external PC which will be running the ROS driver. Note that the robot and the external PC have to be in the **same network**, ideally in a direct connection with each other to minimize network disturbances. The custom port should be left untouched for now.

![Installation section View](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/raw/master/ur_robot_driver/doc/initial_setup_images/es_07_installation_excontrol.png)

7. Create a new program in the **Program** tab and insert the **External Control** program node into the program tree
  
![External Control Program img](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/raw/master/ur_robot_driver/doc/initial_setup_images/es_11_program_view_excontrol.png)

8. If you click on the command tab again, you'll see the settings entered inside the Installation. Check that they are correct, then save the program. Your robot is now ready to be used together with this driver. [^2]

[^2]: *For additional references, see the [install_urcap_e_series.md](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/install_urcap_e_series.md)*
      <br>*For tool communication setup, see the [Tool Communication Setup Guide](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/setup_tool_communication.md)*

## 5. PC Config

### Extract Robot Calibration Info


## 6. Robot Programming



## Resources

- [ROS Noetic Wiki (Ubuntu)](https://wiki.ros.org/noetic/Installation/Ubuntu)
- [MoveIt Installation Tutorial (Noetic)](https://moveit.github.io/moveit_tutorials/doc/getting_started/getting_started.html)
- [Universal Robots Package Install](https://wiki.ros.org/universal_robots)
- [UR ROS Drivers](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver)
- [UR Driver Setup Video](https://www.youtube.com/watch?time_continue=106&v=BS6pFmr7_lA&embeds_referring_euri=https%3A%2F%2Fwww.bing.com%2F&embeds_referring_origin=https%3A%2F%2Fwww.bing.com&source_ve_path=MjM4NTE&feature=emb_title)
- [The Construct: ROS Academy](https://www.theconstruct.ai/robotigniteacademy_learnros/ros-courses-library/)
