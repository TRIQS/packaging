# Start configuration
cmake_minimum_required(VERSION 3.22 FATAL_ERROR)
cmake_policy(VERSION ${CMAKE_VERSION})

project(triqs_dft_tools CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Load Dependencies
find_package(TRIQS REQUIRED)
find_package(${PROJECT_NAME} REQUIRED)

enable_testing()

# The Python Tests
# (the DFT converters, incl. plovasp, live in triqs_dftkit and are tested
#  under test/triqs_dftkit, so dft_tools has no c++ tests here)
if(TRIQS_WITH_PYTHON_SUPPORT)
  add_subdirectory(python)
endif()
