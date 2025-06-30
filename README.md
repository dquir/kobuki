# Kobuki from the Intelligent Robotics Lab using ROS 2

![distro](https://img.shields.io/badge/Ubuntu%2022-Jammy%20Jellyfish-green)
![distro](https://img.shields.io/badge/ROS2-Humble-blue)

This project contains the launchers to run the [Turtlebot2 Kobuki](https://github.com/kobuki-base), both in simulated running different Gazebo worlds, as in the real robot using its drivers.
It is based on the project of [Intelligent Robotics Lab](https://github.com/IntelligentRoboticsLabs/kobuki)

# Installation on your own computer
You need to have previously installed ROS2. Please follow this [guide](https://docs.ros.org/en/humble/Installation.html) if you don't have it.
```bash
source /opt/ros/humble/setup.bash
```

Clone the repository to your workspace:
```bash
cd <ros2-workspace>/src
git clone -b humble https://github.com/dquir/kobuki.git
```

Prepare your thirparty repos:
```bash
sudo apt update
sudo apt install python3-vcstool python3-pip python3-rosdep python3-colcon-common-extensions -y
cd <ros2-workspace>/src/
vcs import < kobuki/thirdparty.repos
```
> This repo uses the [vcs tool](http://wiki.ros.org/vcstool). It can be installed via apt ```sudo apt install python3-vcs-tool``` or with pip ```pip install -U vcstool```
>*Please make sure that this last command has not failed. If this happens, run it again.*

### Install libusb, libftdi & libuvc
```bash
sudo apt install libusb-1.0-0-dev libftdi1-dev libuvc-dev
```

### Install third party packages via apt
```bash
sudo apt install -y ros-humble-gazebo-ros-pkgs ros-humble-gazebo-plugins
sudo apt install -y ros-humble-openni2-camera

```

### Building project
```bash
cd ~/<ros2_workspace>
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install --cmake-args -DCMAKE_POLICY_VERSION_MINIMUM=3.5
```

>  If your terminal has crashed or closed while compiling, please try compiling your packages as follows `colcon build --symlink-install --parallel-workers 1`
> Also, if you want to prevent it from recompiling that package, add a `COLCON_IGNORE` inside the package

### Setup Gazebo to find models - GAZEBO_MODEL_PATH and project path
```bash
source /usr/share/gazebo/setup.bash
source <ros2-workspace>/install/setup.bash
```
*It is recommended to add these two lines inside your `.bashrc` to avoid having to run it every time you open a new shell*

# Run the robot in ROS 2
## Run Gazebo
You can launch the simulator as follows:
```bash
ros2 launch kobuki simulation.launch.py
```
Or you can add the path of your world to the world parameter like this:
```bash
ros2 launch kobuki simulation.launch.py world:=install/aws_robomaker_small_warehouse_world/share/aws_robomaker_small_warehouse_world/worlds/small_warehouse/small_warehouse.world
``` 

If you have a low performance, close the Gazebo's client. Check gzclient process, and kill it:
```bash
kill -9 `pgrep -f gzclient`
``` 

## Run a real kobuki
Run the kobuki drivers:

```bash
ros2 launch kobuki kobuki.launch.py
``` 

If you want to use a lidar or camera, you have to set the following parameters to True:
```bash
ros2 launch kobuki kobuki.launch.py lidar:=True
ros2 launch kobuki kobuki.launch.py lidar_s2:=True
ros2 launch kobuki kobuki.launch.py xtion:=True
ros2 launch kobuki kobuki.launch.py astra:=True
``` 

# Run Navigation in ROS 2

You can use [Nav2] using robot with this launcher:

```bash
ros2 launch kobuki navigation.launch.py map:=<path-to-map>
``` 

or this other command if you need to navigate in the simulator
```bash
ros2 launch kobuki navigation_sim.launch.py
```

If you want to use another map, you have to put the route in the map parameter


# About

This is a project made by the [Intelligent Robotics Lab], a research group from the [Universidad Rey Juan Carlos].
Copyright &copy; 2024.

Maintainers:

* [Juan Carlos Manzanares]


[Universidad Rey Juan Carlos]: https://www.urjc.es/
[Intelligent Robotics Lab]: https://intelligentroboticslab.gsyc.urjc.es/
[José Miguel Guerrero]: https://sites.google.com/view/jmguerrero
[Juan Carlos Manzanares]: https://github.com/Juancams
[Francisco Martín]: https://github.com/fmrico
[Nav2]: https://navigation.ros.org/
[Keepout Zones]: https://navigation.ros.org/tutorials/docs/navigation2_with_keepout_filter.html?highlight=keep
[SLAM Toolbox]: https://vimeo.com/378682207
[Navigate While Mapping]: https://navigation.ros.org/tutorials/docs/navigation2_with_slam.html
