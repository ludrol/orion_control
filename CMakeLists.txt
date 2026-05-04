cmake_minimum_required(VERSION 3.8)
project(orion_control LANGUAGES CXX)

find_package(ros2_control_cmake REQUIRED)
set_compiler_options()
export_windows_symbols()

# find dependencies
set(THIS_PACKAGE_INCLUDE_DEPENDS
  hardware_interface
  pluginlib
  rclcpp
  rclcpp_lifecycle
  rclpy
  mqtt_hardware_interface
)

# Specify the required version of ros2_control
find_package(controller_manager 4.0.0)
# Handle the case where the required version is not found
if(NOT controller_manager_FOUND)
  message(FATAL_ERROR "ros2_control version 4.0.0 or higher is required. "
  "Are you using the correct branch of the ros2_control_demos repository?")
endif()

# find dependencies
find_package(backward_ros REQUIRED)
find_package(ament_cmake REQUIRED)
foreach(Dependency IN ITEMS ${THIS_PACKAGE_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()

## COMPILE
add_library(
  orion_control
  SHARED
  src/hardware/orionVI.cpp
)
target_compile_features(orion_control PUBLIC cxx_std_17)
target_include_directories(orion_control PUBLIC
$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/hardware/include>
$<INSTALL_INTERFACE:include/orion_control>
)
target_link_libraries(orion_control PUBLIC
  hardware_interface::hardware_interface
  pluginlib::pluginlib
  rclcpp::rclcpp
  rclcpp_lifecycle::rclcpp_lifecycle
  mqtt_hardware_interface::MqttHardwareInterface
)

# Export hardware plugins
pluginlib_export_plugin_description_file(hardware_interface orion_control.xml)

# INSTALL
install(
  DIRECTORY src/hardware/include/
  DESTINATION include/orion_control
)
install(
  DIRECTORY src/description/launch src/description/ros2_control src/description/urdf src/description/gazebo
  DESTINATION share/orion_control
)
install(
  DIRECTORY src/bringup/launch src/bringup/config
  DESTINATION share/orion_control
)
install(TARGETS orion_control
  EXPORT export_orion_control
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

## EXPORTS
ament_export_targets(export_orion_control HAS_LIBRARY_TARGET)
ament_export_dependencies(${THIS_PACKAGE_INCLUDE_DEPENDS})
ament_package()
