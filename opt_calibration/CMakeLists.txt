cmake_minimum_required(VERSION 2.8.3)
project(opt_calibration)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_BUILD_TYPE Release)#RelWithDebInfo)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS 
    cmake_modules
    roscpp
    opt_msgs
    detection
    calibration_common
    camera_info_manager
    cv_bridge
    image_transport
    tf_conversions 
    pcl_ros
    tf
    kinect
)

include_directories( include ${catkin_INCLUDE_DIRS} )

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)
find_package(Boost REQUIRED)
include_directories( ${BOOST_INCLUDE_DIRS} )
find_package(Eigen REQUIRED)
include_directories( ${EIGEN_INCLUDE_DIRS})
find_package(PCL REQUIRED)
include_directories( ${PCL_INCLUDE_DIRS} )
find_package(OpenCV REQUIRED)
include_directories( ${OpenCV_INCLUDE_DIRS} )

find_package(OpenMP REQUIRED)
if(OPENMP_FOUND)
    message("OpenMP FOUND")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

###################################################
## Declare things to be passed to other projects ##
###################################################
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need

catkin_package(
    INCLUDE_DIRS include
    CATKIN_DEPENDS roscpp opt_msgs calibration_common camera_info_manager cv_bridge image_transport
    DEPENDS OpenCV eigen pcl
    LIBRARIES 
)   

add_executable(opt_calibration
  src/opt_calibration.cpp              include/open_ptrack/opt_calibration/opt_calibration.h
  src/opt_calibration_node.cpp         include/open_ptrack/opt_calibration/opt_calibration_node.h
  src/ros_device.cpp                   include/open_ptrack/opt_calibration/ros_device.h
  src/opt_checkerboard_extraction.cpp  include/open_ptrack/opt_calibration/opt_checkerboard_extraction.h
)

## Add dependencies to the executable
## Specify libraries to link a library or executable target against
target_link_libraries(opt_calibration
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBS}
)

# Publish camera_info topic for stereo:
add_executable(stereo_publisher apps/stereo_publisher.cpp)

target_link_libraries(stereo_publisher
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBS}
)

add_executable(opt_calibration_refinement
  apps/opt_calibration_refinement.cpp
  src/trajectory_registration.cpp
)
target_link_libraries(opt_calibration_refinement
  ${catkin_LIBRARIES}
)

add_executable(opt_define_reference_frame
  src/opt_calibration.cpp              include/open_ptrack/opt_calibration/opt_calibration.h
  src/opt_define_reference_frame.cpp   include/open_ptrack/opt_calibration/opt_define_reference_frame.h
  src/ros_device.cpp                   include/open_ptrack/opt_calibration/ros_device.h
  src/opt_checkerboard_extraction.cpp  include/open_ptrack/opt_calibration/opt_checkerboard_extraction.h
)

target_link_libraries(opt_define_reference_frame
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBS}
)
