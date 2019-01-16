cmake_minimum_required(VERSION 2.8.9)
project(cannula_kinematics)
set(CMAKE_BUILD_TYPE Release)
add_definitions(-std=c++14) # use C++14

find_package(Eigen3 REQUIRED NO_MODULE) # sets ${EIGEN3_INCLUDE_DIR}
find_package(Boost REQUIRED) # sets ${Boost_INCLUDE_DIR}

include_directories(
  include
  ${EIGEN3_INCLUDE_DIR}
  ${Boost_INCLUDE_DIR}
)

file(GLOB SOURCES "src/*.cpp")
add_library(cannula_kinematics SHARED ${SOURCES})

#add_library(cannula_kinematics SHARED 
#  src/DynamicMemoryBlock.cpp
#  src/ReferenceCannulaKinematics.cpp
#  src/Rk8_Coeffs.cpp
#  src/TaggedInterval.cpp
#)

