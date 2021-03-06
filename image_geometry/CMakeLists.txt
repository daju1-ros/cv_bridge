cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

find_package(OpenCV REQUIRED)

rosbuild_add_library(${PROJECT_NAME} src/pinhole_camera_model.cpp src/stereo_camera_model.cpp)
target_link_libraries(${PROJECT_NAME} 
  ${OpenCV_LIBS} 
  )

rosbuild_add_pyunit(test/directed.py)

rosbuild_add_gtest(test/utest test/utest.cpp)
target_link_libraries(test/utest
  ${PROJECT_NAME}
  ${OpenCV_LIBS}
  )
