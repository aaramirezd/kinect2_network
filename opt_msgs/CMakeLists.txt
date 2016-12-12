cmake_minimum_required(VERSION 2.8.3)
project(opt_msgs)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    message_generation
    geometry_msgs
    sensor_msgs
)

include_directories( ${catkin_INCLUDE_DIRS} )

include_directories(include include/open_ptrack/${PROJECT_NAME}/)

add_message_files(FILES 
    Rois.msg
    RoiRect.msg
    HumanEntry.msg
    HumanEntries.msg
    BoundingBox2D.msg
    BoundingBox3D.msg Detection.msg DetectionArray.msg Track.msg TrackArray.msg CalibrationStatus.msg IDArray.msg)

add_service_files(FILES OPTSensor.srv OPTTransform.srv)

generate_messages(DEPENDENCIES std_msgs geometry_msgs sensor_msgs)

catkin_package(
    CATKIN_DEPENDS roscpp std_msgs sensor_msgs message_runtime
    LIBRARIES
    INCLUDE_DIRS 
)

add_library(roi_msgs src/overlap.cpp )

target_link_libraries(roi_msgs ${catkin_LIBRARIES})
