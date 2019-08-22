# Start configuration
cmake_minimum_required(VERSION 2.8.12)
project(triqs_maxent_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

#Load Dependencies
find_package(TRIQS)

enable_testing()

# The Python Maxent Tests
if(${TRIQS_WITH_PYTHON_SUPPORT})
  add_subdirectory(python)
endif()
