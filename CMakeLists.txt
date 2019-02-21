cmake_minimum_required(VERSION 3.0)

project(livox_sdk VERSION 0.9)

set(CMAKE_CXX_STANDARD 98)

message(STATUS "main project dir: " ${PROJECT_SOURCE_DIR})


include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(TARGETS)

add_subdirectory(sdk_core sdk_core)
add_subdirectory(sample/hub)
add_subdirectory(sample/lidar)

