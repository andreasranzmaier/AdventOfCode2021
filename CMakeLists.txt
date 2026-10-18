cmake_minimum_required(VERSION 3.20)
project(aoc2021 CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif()

add_compile_options(-Wall -Wextra -Wpedantic)

# Every dayNN/src/dayNN.cpp becomes its own executable named dayNN.
file(GLOB DAY_SOURCES CONFIGURE_DEPENDS ${CMAKE_SOURCE_DIR}/day*/src/day*.cpp)
foreach(src ${DAY_SOURCES})
  get_filename_component(name ${src} NAME_WE)
  add_executable(${name} ${src})
endforeach()
