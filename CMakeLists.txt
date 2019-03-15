cmake_minimum_required(VERSION 3.5)
project(roborts_slam)

## Compile as C++11, supported in ROS Kinetic and newer

#set (CMAKE_CXX_COMPILER "/usr/bin/clang++-6.0")
set(CMAKE_CXX_STANDARD 14)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
        roscpp
        message_filters
        nav_msgs
        rosconsole
        roscpp
        sensor_msgs
        geometry_msgs
        tf
        visualization_msgs
        )

#find_package(PCL 1.7 REQUIRED)
#include_directories(
#        ${catkin_INCLUDE_DIRS}
#        ${PCL_INCLUDE_DIRS}
#)

find_package(Ceres REQUIRED)
include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${catkin_INCLUDE_DIRS}
        ${CERES_INCLUDE_DIRS}
)

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake_module)
find_package(Eigen3)
include_directories(
        ${catkin_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIR}
)

find_package(Glog REQUIRED)

catkin_package(


)

###########
## Build ##
###########

add_executable(
        roborts_slam_node
        src/roborts_slam_node.cpp
        src/laser_data_processor.cpp
        src/laser_data_processor.h
        src/slam/slam_processor.h
        src/slam/sensor_data_manager.h
        src/pose_graph/range_scan_pose_graph.h
        src/pose_graph/pose_graph.h
        src/pose_graph/range_scan_pose_graph.cpp
        src/pose_graph/ceres_pose_graph_solver.cpp
        src/util/boundbox.h
        src/scan_match/scan_matchers.h
        src/scan_match/optimize_scan_matcher.h
        src/scan_match/correlate_scan_matcher.h src/slam/slam_processor.cpp)

target_link_libraries(
        roborts_slam_node
        #${CMAKE_THREAD_LIBS_INIT}
        ${catkin_LIBRARIES}
        ${GLOG_LIBRARY}
        ${CERES_LIBRARIES}
        #${PCL_LIBRARIES}
)


add_executable(
        util_test
        src/test/util_test.cpp
)

target_link_libraries(
        util_test
        #${CMAKE_THREAD_LIBS_INIT}
        ${catkin_LIBRARIES}
        ${GLOG_LIBRARY}
)


