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

### Wired Robot Connection Settings

In your PC's settings, navigate to **Network** and check your Wired settings. Make sure that under **IPv4** you set your Network and Address to the same one the robot is on.

![Wired Network Settings]()

### Extracting Robot Calibration Info

Each UR robot is calibrated inside the factory giving exact forward and inverse kinematics. To make use of this in ROS, you first have to extract the **calibration information** from the robot.
This node extracts calibration information directly from a robot, calculates the URDF correction and saves it into a .yaml file.

In the launch folder of the ur_calibration package is a helper script:

```
$ roslaunch ur_calibration calibration_correction.launch \
  robot_ip:=<robot_ip> target_filename:="${HOME}/my_robot_calibration.yaml"
```

For the parameter `robot_ip` insert the IP address on which the ROS pc can reach the robot, and `target_filename` provides an absolute path where the result will be saved to. 

Calibrations for all robots in your organization should be kept in a common package. 👇

### Creating a calibration / launch package for all local robots

When dealing with multiple robots in one organization it might make sense to store calibration data into a package dedicated to that purpose only. To do so, create a new package (if it doesn't already exist)

```
# Replace your actual catkin_ws folder
$ cd <catkin_ws>/src
$ catkin_create_pkg example_organization_ur_launch ur_robot_driver \
-D "Package containing calibrations and launch files for our UR robots."
# Create a skeleton package
$ mkdir -p example_organization_ur_launch/etc
$ mkdir -p example_organization_ur_launch/launch
```

We can use the new package to store the calibration data in that package. We recommend naming each robot individually, e.g. ur5e-1, ur5e-2, etc.

```
$ roslaunch ur_calibration calibration_correction.launch \
robot_ip:=<robot_ip> \
target_filename:="$(rospack find example_organization_ur_launch)/etc/ex-ur10-1_calibration.yaml"
```

Create a launchfile for each particular robot. We base it upon the respective launchfile in the driver:

```
# Replace your actual catkin_ws folder
$ cd <catkin_ws>/src/example_organization_ur_launch/launch
$ roscp ur_robot_driver ur10_bringup.launch ex-ur10-1.launch
```

Next, modify the parameter section of the new launchfile to match your actual calibration:

```
<!-- Note: Only the relevant lines are printed here-->
  <arg name="robot_ip" default="192.168.0.101"/> <!-- if there is a default IP scheme for your
  robots -->
  <arg name="kinematics_config" default="$(find example_organization_ur_launch)/etc/ex-ur10-1_calibration.yaml"/>
```

Then, anybody cloning this repository can startup the robot simply by launching

```
$ roslaunch example_organization_ur_launch ex-ur10-1.launch
```

## Starting the Driver

Once the driver is built and the externalcontrol URCap is installed on the robot, you are good to go ahead starting the driver.

To actually start the robot driver use one of the existing launch files

```
$ roslaunch ur_robot_driver <robot_type>_bringup.launch robot_ip:=192.168.56.101
```
\*Be sure to type "**ur5e**" inplace of `<robot_type>`.


Next, pass that calibration to the launch file:

```
$ roslaunch ur_robot_driver <robot_type>_bringup.launch robot_ip:=192.168.56.101 \
  kinematics_config:=$(rospack find ur_calibration)/etc/ur10_example_calibration.yaml
```

If the parameters in that file don't match the ones reported from the robot, the driver will output an error during startup, but will remain usable.

For more information on the launch file's parameters see its own documentation.

Once the robot driver is started, load the previously generated program on the robot panel that will start the External Control program node and execute it. From that moment on the robot is fully functional. You can make use of the Pause function or even Stop :stop_button: the program. Simply press the Play button :arrow_forward: again and the ROS driver will reconnect.

## 6. Robot Programming



## Resources

- [ROS Noetic Wiki (Ubuntu)](https://wiki.ros.org/noetic/Installation/Ubuntu)
- [MoveIt Installation Tutorial (Noetic)](https://moveit.github.io/moveit_tutorials/doc/getting_started/getting_started.html)
- [Universal Robots Package Install](https://wiki.ros.org/universal_robots)
- [UR ROS Drivers](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver)
- [UR Driver Setup Video](https://www.youtube.com/watch?time_continue=106&v=BS6pFmr7_lA&embeds_referring_euri=https%3A%2F%2Fwww.bing.com%2F&embeds_referring_origin=https%3A%2F%2Fwww.bing.com&source_ve_path=MjM4NTE&feature=emb_title)
- [UR Calibration](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_calibration/README.md)
- [The Construct: ROS Academy](https://www.theconstruct.ai/robotigniteacademy_learnros/ros-courses-library/)
