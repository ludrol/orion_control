# orion_control
This package translates a singular input vector into separate wheel and turntable commands, using the ros2_control framework.
## Building from source

Install required dependencies:
```
sudo apt install ros-${ROS_DISTRO}-ros2-control ros-${ROS_DISTRO}-ros2-controllers ros-${ROS_DISTRO}-ros2-control-cmake ros-$ROS_DISTRO-gz-ros2-control
sudo apt install libpaho-mqtt-dev
#sudo apt install libpaho-mqttpp-dev #idk if this is required
```

```
git clone https://github.com/Bungok/ros2_controllers_orion.git
cd ros2_controllers_orion
git checkout feature/swerve-drive-controller
colcon build --packages-select swerve_drive_controller
#FIX errors
cd ..

```

```
git clone https://github.com/Bungok/mqtt_hardware_interface.git
cd mqtt_hardware_interface
colcon build
cd ..
```

To build the package run:

```
cd ../orion_control/
source ../ros2_controllers_orion/install/setup.bash
source ../mqtt_hardware_interface/install/setup.bash
colcon build --merge-install
source install/local_setup.bash

```




## Running
To run in the Gazebo simulation mode run:
```
ros2 launch orion_control orionVI_gazebo.launch.py
```

To send /cmd_vel from your gamepad use:
```
ros2 launch teleop_twist_joy teleop-launch.py joy_config:='xbox'
```