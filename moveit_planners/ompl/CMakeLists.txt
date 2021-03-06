cmake_minimum_required(VERSION 3.1.3)
project(moveit_planners_ompl)

# At least C++11 required for OMPL
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Boost REQUIRED system filesystem date_time thread serialization)
find_package(catkin REQUIRED COMPONENTS
  moveit_core
  moveit_ros_planning
  roscpp
  rosconsole
  pluginlib
  tf2_ros
  dynamic_reconfigure
  )

find_package(ompl REQUIRED)

generate_dynamic_reconfigure_options("ompl_interface/cfg/OMPLDynamicReconfigure.cfg")

catkin_package(
  LIBRARIES
    moveit_ompl_interface
  INCLUDE_DIRS
    ompl_interface/include
  CATKIN_DEPENDS
    dynamic_reconfigure
    moveit_core
    roscpp
  DEPENDS
    OMPL
)

include_directories(ompl_interface/include
                    ${Boost_INCLUDE_DIRS}
                    ${catkin_INCLUDE_DIRS}
                    ${OMPL_INCLUDE_DIRS})

add_subdirectory(ompl_interface)

install(FILES ompl_interface_plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
