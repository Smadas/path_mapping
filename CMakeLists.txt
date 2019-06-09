cmake_minimum_required(VERSION 2.8.3)
project(path_mapping)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
        cv_bridge
        pcl_ros
        roscpp
        rospy
        image_transport
        std_msgs
        tf
        tf_conversions
        )
find_package(PCL REQUIRED QUIET COMPONENTS common sample_consensus io segmentation filters)
find_package(OpenCV 3 REQUIRED
        COMPONENTS
        opencv_core			#find by cv_bridge but for sure also here
        videoio
        video
        highgui
        opencv_imgproc		#find by cv_bridge but for sure also here
        opencv_imgcodecs	#find by cv_bridge but for sure also here
        calib3d
        )


catkin_package(
  INCLUDE_DIRS include
#  LIBRARIES path_mapping
#  CATKIN_DEPENDS kittiseg_ros roscpp rospy std_msgs
#  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
# include
        include/path_mapping
  ${catkin_INCLUDE_DIRS}
        ${catkin_INCLUDE_DIRS}
        ${PCL_INCLUDE_DIRS}
        ${OpenCV_INCLUDE_DIRS}
)

add_definitions(-std=c++11)

set(LOCAL_INCLUDES 	src/recognize_sidewalk/recognize_sidewalk.cpp
        src/pavement_to_marker/pavement_to_marker.cpp
        src/pavement_to_cloud/pavement_to_cloud.cpp
        src/misc_tools/misc_tools.cpp
        src/recognize_sidewalk/picture_segmentation.cpp
        src/RecognizeSidewalkParams.cpp
        src/RecognizeSidewalkParams.h
        src/cloud_processing.cpp
        src/recognize_sidewalk/SidewalkEdge.cpp
        src/recognize_sidewalk/SidewalkEdge.h
        src/recognize_sidewalk/polyfit2.cpp
        src/recognize_sidewalk/polyfit2.h
        )

add_executable(recognize_sidewalk_video src/recognize_sidewalk_video.cpp ${LOCAL_INCLUDES})
target_link_libraries(recognize_sidewalk_video ${PCL_LIBRARIES} ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
#add_dependencies(recognize_sidewalk recognize_sidewalk_generate_messages_cpp)

add_executable(recognize_sidewalk_kinect src/recognize_sidewalk_kinect.cpp ${LOCAL_INCLUDES})
target_link_libraries(recognize_sidewalk_kinect ${PCL_LIBRARIES} ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
#add_dependencies(recognize_sidewalk_kinect recognize_sidewalk_kinect_generate_messages_cpp)

add_executable(camera_calibration src/camera_calibration.cpp ${LOCAL_INCLUDES} )
target_link_libraries(camera_calibration ${catkin_LIBRARIES} ${OpenCV_LIBS})

add_executable(filter_cloud src/FilterCloud.cpp ${LOCAL_INCLUDES})
target_link_libraries(filter_cloud ${catkin_LIBRARIES} ${OpenCV_LIBS})
#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_path_mapping.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
