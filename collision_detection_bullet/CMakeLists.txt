cmake_minimum_required(VERSION 2.8.3)
project(collision_detection_bullet)

add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  #trajopt
  bullet3_ros
  #libccd_ros
  #fcl_ros
  urdf
  moveit_core
  moveit_ros_planning
  pluginlib
  tesseract_ros
)

find_package(Eigen3 REQUIRED)
find_package(Boost COMPONENTS system thread REQUIRED)
find_package(octomap REQUIRED)

set(BULLET_LIBRARIES BulletCollision LinearMath)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS
    include
    ${EIGEN3_INCLUDE_DIRS}
  LIBRARIES
    moveit_collision_detection_bullet
  CATKIN_DEPENDS
    roscpp
    trajopt_test_support
    bullet3_ros
    # libccd_ros
    #fcl_ros
    urdf
    moveit_core
    moveit_ros_planning
    pluginlib
    tesseract_ros
  DEPENDS
    EIGEN3
    Boost
    OCTOMAP
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  SYSTEM ${EIGEN3_INCLUDE_DIRS}
  SYSTEM ${Boost_INCLUDE_DIRS}
  SYSTEM ${OCTOMAP_INCLUDE_DIRS}
)

add_library(moveit_collision_detection_bullet
  src/collision_common.cpp
  src/collision_robot_bullet.cpp
  src/collision_world_bullet.cpp
)

target_link_libraries(moveit_collision_detection_bullet ${catkin_LIBRARIES} ${console_bridge_LIBRARIES} ${urdfdom_LIBRARIES} ${urdfdom_headers_LIBRARIES} ${Boost_LIBRARIES} ${OCTOMAP_LIBRARIES})

add_library(moveit_collision_detection_bullet_plugin src/collision_detector_allocator_bullet.cpp)
target_link_libraries(moveit_collision_detection_bullet_plugin moveit_collision_detection_bullet ${catkin_LIBRARIES})

