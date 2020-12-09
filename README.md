# sara_sensors_pkgs

## Dependencies
- System space packages
   ``` 
   sudo apt-get install ros-melodic-jackal*
   sudo apt-get install ros-melodic-velodyne*
   sudo apt-get install ros-melodic-pointcloud-to-laserscan
   sudo apt-get install ros-melodic-realsense2*
   sudo apt-get install ros-melodic-cartographer-ros
   sudo apt-get install ros-melodic-cartographer-ros-msgs
   ```
- catkin_ws packages
   ``` 
   cd ~/catkin_ws/src 
   git clone https://github.com/westpoint-robotics/sara_sensors_pkgs.git
   cd sara_sensors_pkgs
   git clone https://github.com/jackal/jackal_cartographer_navigation
   ```

## The SARA Sensors Package

As of 8 Dec, 2020, these packages are complete for use with the Clearpath Jackal as simulated in Gazebo.  The packages will let you use a simulated Jackal with the SARA sensor array mounted on it to perform Google Cartographer.  Within the jackal_description package inside of the sara_sensors_pkgs, there is a costomized versino of the jackal.urdf.xacro which incorporates the SARA sensors modular frame (a modular frame constructed with 8020 for mounting a set of sensors in a particular configuration for SLAM, stereo vision, etc.)  If you want to run a basic demo without concerning yourself with Cartographer, you can run the following:

` roslaunch sara_description sara_sensors_demos.launch which_robot:=jackal `

In future updates, the "which_robot" parameter will allow you to indirectly launch a husky, jackal, or gvrbot gazebo simulation.  It also is used to determine whether to use the wider or narrower version of the 8020 frame (sensor placement within the frame is always the same), and what the origin of the frame is relative to the robot in question.  For the GVR-Bot, the frame is currently centered, instead of at the front of the robot as in the case of the husky and jackal.  This may be modified in line 45 of the sara.urdf.xacro

```
 <xacro:if value="${version == 'gvrbot'}">
   <origin xyz="0.0 0.0 0.1675" rpy="0 0 0" />
 </xacro:if> 
```
 
When you are running the sara_sensors_demos luanch files, you can then also run RViz using
```
roslaunch <current robot>_viz view_robot.launch
```

You can also use RViz alone with:
```
roslaunch <current robot>_viz view_model.launch
```
## Cartographer
To run Google Cartographer with the Jackal using the SARA sensor array, use the command below.  There is a parameter called, "which_scan" that defaults to "ouster" and can be set to "ouster" or "realsense" and will use the pointcloud published by the associated sensor to generate the laser scan topic that Cartographer subscribes to.
```
roslaunch sara_description jackal_cartographer_sara_sensors.launch which_scan:=ouster
roslaunch sara_description jackal_cartographer_sara_sensors.launch which_scan:=realsense
```
## Troubleshooting

If you run into trouble involving "ignition fuel", do the following (you can use whatever text editor you prefer):
```
gedit ~/ignition/fuel/config.yaml
```
Edit the line "api.ignitionfuel.org" to read, "fuel.ignitionrobotics.org".  Save and close.

### TODO:
```
 -Update with Husky and GVR-Bot
 -Make more self-contained to eliminate need to edit robot repositories
 -Make OS1-64 work in Gazebo without using Velodyne
 -Add instructions for working with actual robots ```
