SET(execName zed_openpose)
cmake_minimum_required(VERSION 2.4)
PROJECT(${execName})

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 OLD)
	cmake_policy(SET CMP0015 OLD)
	cmake_policy(SET CMP0072 NEW)
endif(COMMAND cmake_policy)

SET(EXECUTABLE_OUTPUT_PATH ".")

set(OPENPOSE_INCLUDE_DIRS 
	"D:\\openpose-binaries\\openpose\\include"
	"D:\\openpose\\3rdparty\\windows\\caffe\\include"
	"D:\\openpose\\3rdparty\\windows\\caffe\\include2"
	"D:\\openpose\\3rdparty\\windows\\caffe3rdparty\\include"
	"D:\\openpose\\3rdparty\\windows\\freeglut\\include"
	"D:\\openpose\\3rdparty\\windows\\opencv\\include")

set(OPENPOSE_LINK_LIBRARY_DIRECTORIES
	"D:\\openpose-binaries\\openpose\\lib"
	"D:\\openpose\\3rdparty\\windows\\caffe\\lib"
	"D:\\openpose\\3rdparty\\windows\\caffe3rdparty\\lib"
	"D:\\openpose\\3rdparty\\windows\\freeglut\\lib"  
	"D:\\openpose\\3rdparty\\windows\\opencv\\x64\\vc15\\lib")

set(OPENPOSE_LINK_LIBRARIES 
	"D:\\openpose-binaries\\openpose\\lib\\openpose.lib"
	"D:\\openpose\\3rdparty\\windows\\caffe\\lib\\caffe.lib"
	"D:\\openpose\\3rdparty\\windows\\caffe3rdparty\\lib\\gflags.lib"
	"D:\\openpose\\3rdparty\\windows\\caffe3rdparty\\lib\\glog.lib"
	"D:\\openpose\\3rdparty\\windows\\freeglut\\lib\\freeglut.lib"  "D:\\openpose\\3rdparty\\windows\\opencv\\x64\\vc15\\lib\\opencv_world420.lib")


find_package(ZED 3 REQUIRED)
find_package(GLEW REQUIRED)
find_package(OpenGL REQUIRED)
find_package(CUDA REQUIRED)
#find_package(GLUT REQUIRED)
#find_package(OpenPose REQUIRED)
#find_package(OpenCV REQUIRED)

IF(NOT WIN32)
     SET(SPECIAL_OS_LIBS "pthread" "X11")
    add_definitions(-std=c++11 -Wno-write-strings -O3)
ENDIF()

include_directories(${ZED_INCLUDE_DIRS})
include_directories(${GLEW_INCLUDE_DIRS})
include_directories(${CUDA_INCLUDE_DIRS})
include_directories(${OPENPOSE_INCLUDE_DIRS})
#include_directories(${GLUT_INCLUDE_DIRS})
#include_directories(${OPENCV_INCLUDE_DIRS})

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

link_directories(${ZED_LIBRARY_DIR})
link_directories(${GLEW_LIBRARY_DIRS})
link_directories(${OpenGL_LIBRARY_DIRS})
link_directories(${CUDA_LIBRARY_DIRS})
#link_directories(${GLUT_LIBRARY_DIRS})
#link_directories(${OpenCV_LIBRARY_DIRS})

FILE(GLOB_RECURSE SRC_FILES src/*.cpp)
FILE(GLOB_RECURSE HDR_FILES include/*.hpp)

ADD_EXECUTABLE(${execName} ${HDR_FILES} ${SRC_FILES})

target_link_libraries(${execName}
                        ${OPENPOSE_LINK_LIBRARIES}
                        ${ZED_LIBRARIES}
                        ${GLEW_LIBRARIES}
                        ${SPECIAL_OS_LIBS}
                        ${OPENGL_LIBRARIES}
                        ${CUDA_CUDA_LIBRARY} ${CUDA_CUDART_LIBRARY} ${CUDA_NPP_LIBRARIES_ZED} )
