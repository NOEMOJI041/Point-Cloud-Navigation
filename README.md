# MR-Robot
MR. Robot: Modular Robot

# INTRODUCTION
This package was created as part of the Autonomous Navigation tutorial for MR. Robot. It provides configuration for SLAM and autonomous navigation for MR. Robot equipped with Depth and LiDAR sensors.

# INSTALLATION

## MR. Robot setup

Clone MR. RObot repository into cd catkin_ws/src/

```sh
git@github.com:atom-robotics-lab/MR-Robot.git
``` 

Now we will use catkin make command to get the ROS on our system to recognise the examples.
```sh
cd ..
catkin_make
```

Source the setup file in newly created ‘devel’ directory so that our ROS environment can recognise the launch files.
```sh
source devel/setup.sh
```
 Now run terminal this will launch your world in gazebo 
 ```sh
 roslaunch mr_robot_gazebo turtlebot3_house.launch 
```

## Depth_sensor setup


__Point_Cloud__

To 
As always, start with an update and upgrade.
```sh

sudo apt-get update
sudo apt-get upgrade

```


Install the dependencies

```sh

sudo apt-get install git-core cmake freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev

```
```sh
sudo apt install ros-noetic-rgbd-launch
```


Get the libfreenect repository from GitHub

```sh

git clone git://github.com/OpenKinect/libfreenect.git

```

Make and install

```sh
cd libfreenect
mkdir build
cd build
cmake -L ..
make
sudo make install
sudo ldconfig /usr/local/lib64/

```

To use kinect without sudoing every time

```sh

sudo adduser $USER video
sudo adduser $USER plugdev

```


Next we will add some device rules

```sh
sudo nano /etc/udev/rules.d/51-kinect.rules

```


Paste the following and ctrl+q to save.
 ```sh
 # ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02b0", MODE="0666"
# ATTR{product}=="Xbox NUI Audio"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ad", MODE="0666"
# ATTR{product}=="Xbox NUI Camera"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ae", MODE="0666"
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02c2", MODE="0666"
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02be", MODE="0666"
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02bf", MODE="0666"
```


Now Create your catkin workspace directory, skip this step if you already have it setup.
```sh
mkdir -r ~/catkin_ws/src
```

Now we will download the required ROS package.

```cd
cd ~/catkin_ws/src
git clone https://github.com/ros-drivers/freenect_stack.git
```


Now we will use catkin make command to get the ROS on our system to recognise the examples.
```sh
cd ..
catkin_make
```


Source the setup file in newly created ‘devel’ directory so that our ROS environment can recognise the launch files.
```sh
source devel/setup.sh
```

Now run these comands to get depth image on rviz and change fixed frame from map to camera_link


```sh
 roslaunch mr_robot_gazebo turtlebot3_house.launch 
```
```sh
roslaunch freenect_launch freenect.launch depth_registration:=true

```

__Octomapping__

Install Octomap
```sh
sudo apt-get install ros-noetic-octomap ros-noetic-octomap-mapping ros-noetic-octomap-msgs ros-noetic-octomap-ros ros-noetic-octomap-rviz-plugins ros-noetic-octomap-server

```

Now go to directory __octomap_mapping/octomap_server/launch__ and open __octomapping.launch__ and change following

in line 14 odom_combined --> odom
in line 20 /narrow_stereo/points_filtered2 --> /kinect/depth/points

### 

Run 
```sh
roslaunch octomap_server octomap_mapping.launch
```
###

Now on Rviz add __MarkerArray__ and change topic to __/occupied_cells_vis_array__

## Creating map

Now navigate bot in gazebo using teleop twist keyboard 

```sh
rosrun teleop_twist_keyboard teleop_twist_keyboard.py 

``` 
this will create your map

## Saving the new map

Now that we have created a new map, we need to save it to be able to use it in future. Open a new terminal in the maps directory inside the mr_robot_nav package and execute the following command there :

```sh
rosrun map_server map_saver -f custom_map
```



