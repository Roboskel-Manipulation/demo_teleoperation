cmake_minimum_required(VERSION 3.0.2)
project(openpose_teleoperation)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
)

catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES openpose_teleoperation
#  CATKIN_DEPENDS roscpp rospy
#  DEPENDS system_lib
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)
