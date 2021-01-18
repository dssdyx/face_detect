cmake_minimum_required(VERSION 2.8.3)
project(object_detection)

set(CMAKE_BUILD_TYPE "Release")
set (CMAKE_CXX_STANDARD 11)
set (CXX_STANDARD_REQUIRED true)
#set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror") #treating warnings as errors
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wuninitialized -Winit-self")
if(NOT ${CMAKE_CXX_COMPILER_ID} STREQUAL Clang)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wmaybe-uninitialized")
endif()

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  roscpp
  rospy
  sensor_msgs
  std_msgs
  image_transport
  monitors
)


find_package(OpenCV REQUIRED COMPONENTS core imgproc highgui)
find_package(InferenceEngine 2.0 REQUIRED)
find_package(ngraph REQUIRED)


catkin_package(
  INCLUDE_DIRS include
  LIBRARIES object_detection
  CATKIN_DEPENDS cv_bridge roscpp rospy sensor_msgs std_msgs monitors
#  DEPENDS system_lib
)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${InferenceEngine_INCLUDE_DIRS}
)


add_executable(${PROJECT_NAME}_node 
   src/main.cpp
)
target_link_libraries(${PROJECT_NAME}_node ${OpenCV_LIBRARIES} ${InferenceEngine_LIBRARIES} ${catkin_LIBRARIES} pthread ngraph::ngraph)

