cmake_minimum_required(VERSION 2.8.3)
project(detection)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_BUILD_TYPE RelWithDebInfo)#RelWithDebInfo)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS 
    cmake_modules
    roscpp
    rospy
    calibration_common
    image_transport
    pcl_conversions 
    pcl_ros
    nodelet
    rosconsole
    cv_bridge
    opt_msgs
    opt_utils
    dynamic_reconfigure
)

include_directories(${catkin_INCLUDE_DIRS})

find_package(Boost REQUIRED)
include_directories( ${BOOST_INCLUDE_DIRS} )
find_package(Eigen3 REQUIRED)
include_directories( ${EIGEN_INCLUDE_DIRS})
find_package(PCL REQUIRED)
include_directories( ${PCL_INCLUDE_DIRS} )
find_package(OpenCV REQUIRED)
include_directories( ${OpenCV_INCLUDE_DIRS} )


include_directories( include )   
include_directories( cfg/cpp )
# Dynamic reconfigure support
generate_dynamic_reconfigure_options(cfg/HaarDispAdaDetector.cfg cfg/GroundBasedPeopleDetector.cfg)

catkin_package(
    CATKIN_DEPENDS roscpp pcl_ros pcl_conversions opt_utils opt_msgs
    DEPENDS OpenCV
    LIBRARIES ${PROJECT_NAME}
    INCLUDE_DIRS include
)

add_library(${PROJECT_NAME} src/detection_source.cpp src/detection.cpp)
add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_gencfg)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${OpenCV_LIBS})

add_executable(ground_based_people_detector apps/ground_based_people_detector_node.cpp)
SET_TARGET_PROPERTIES(ground_based_people_detector PROPERTIES LINK_FLAGS -L${PCL_LIBRARY_DIRS})
target_link_libraries(ground_based_people_detector ${PROJECT_NAME} ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_library(haar_disp_ada src/haardispada.cpp apps/haardispada_nodelet.cpp)
target_link_libraries(haar_disp_ada ${catkin_LIBRARIES})

add_executable(HaarDispAda174 apps/haardispada_node.cpp)
target_link_libraries(HaarDispAda174 haar_disp_ada ${PROJECT_NAME} ${catkin_LIBRARIES} ${OpenCV_LIBS} boost_system boost_signals)
