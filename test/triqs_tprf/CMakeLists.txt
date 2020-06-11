# Start configuration
cmake_minimum_required(VERSION 3.3.2 FATAL_ERROR)
cmake_policy(VERSION 3.3.2)
if(POLICY CMP0074)
  cmake_policy(SET CMP0074 NEW)
endif()

project(triqs_tprf CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Load Dependencies
find_package(TRIQS REQUIRED)
find_package(${PROJECT_NAME} REQUIRED)

# Build googletest
add_subdirectory(/usr/src/gtest gtest EXCLUDE_FROM_ALL)

# Lift the relevant triqs_cthyb targets out of the triqs_cthyb namespace
add_library(${PROJECT_NAME}_warnings INTERFACE)

enable_testing()

# The C++ Tests
add_subdirectory(c++)

# The Python Tests
if(${TRIQS_WITH_PYTHON_SUPPORT})
  add_subdirectory(python)
endif()
