# orion_control
This package translates a singular input vector into separate wheel and turntable commands, using the ros2_control framework.
## Building from source
To build the package run:
```
colcon build --merge-install
source install/local_setup.bash
```
A custom version of swerve_drive_controller is required.
It can be found here: [text](https://github.com/Bungok/ros2_controllers_orion)
In the ros2_controllers_orion directory run:
```
colcon build --packages-select swerve_drive_controller
source install/local_setup.bash
```

## Running
To run in the Gazebo simulation mode run:
```
ros2 launch orion_control orionVI_gazebo.launch.py
```