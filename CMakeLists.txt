cmake_minimum_required(VERSION 3.0.2)
project(demo_teleoperation)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)
