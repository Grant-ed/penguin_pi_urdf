cmake_minimum_required(VERSION 3.16)
project(diffdrive_penguinpi LANGUAGES CXX)

if(CMAKE_CXX_COMPILER_ID MATCHES "(GNU|Clang)")
  add_compile_options(-Wall -Wextra)
endif()

set(LIBSERIAL_DIR ~/libserial/src/)

# find dependencies
set(THIS_PACKAGE_INCLUDE_DEPENDS
  hardware_interface
  pluginlib
  rclcpp
  rclcpp_lifecycle
)

# find dependencies
find_package(ament_cmake REQUIRED)
foreach(Dependency IN ITEMS ${THIS_PACKAGE_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()

include_directories(${LIBSERIAL_DIR})
set(SOURCES
  ${LIBSERIAL_DIR}SerialPort.cpp
  ${LIBSERIAL_DIR}SerialStream.cpp
  ${LIBSERIAL_DIR}SerialStreamBuf.cpp
)

## COMPILE
add_library(
  diffdrive_penguinpi
  SHARED
  hardware/diffbot_system.cpp
  hardware/penguinpi_comms.cpp
  ${SOURCES}
)
target_compile_features(diffdrive_penguinpi PUBLIC cxx_std_17)
target_include_directories(diffdrive_penguinpi PUBLIC
$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/hardware/include>
$<INSTALL_INTERFACE:include/diffdrive_penguinpi>
)
ament_target_dependencies(
  diffdrive_penguinpi PUBLIC
  ${THIS_PACKAGE_INCLUDE_DEPENDS}
)

# target_link_libraries(diffdrive_penguinpi PUBLIC serial)

# Causes the visibility macros to use dllexport rather than dllimport,
# which is appropriate when building the dll but not consuming it.
target_compile_definitions(${PROJECT_NAME} PRIVATE "DIFFDRIVE_PENGUINPI_BUILDING_DLL")

# Export hardware plugins
pluginlib_export_plugin_description_file(hardware_interface diffdrive_penguinpi.xml)

# INSTALL
install(
  DIRECTORY hardware/include/
  DESTINATION include/diffdrive_penguinpi
)
install(
  DIRECTORY description/launch description/ros2_control description/urdf description/rviz description/meshes
  DESTINATION share/diffdrive_penguinpi
)
install(
  DIRECTORY bringup/launch bringup/config
  DESTINATION share/diffdrive_penguinpi
)
install(TARGETS diffdrive_penguinpi
  EXPORT export_diffdrive_penguinpi
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

if(BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
endif()

## EXPORTS
ament_export_targets(export_diffdrive_penguinpi HAS_LIBRARY_TARGET)
ament_export_dependencies(${THIS_PACKAGE_INCLUDE_DEPENDS})
ament_package()
