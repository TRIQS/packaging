# Start configuration
cmake_minimum_required(VERSION 2.8.12)
project(triqs_tprf_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Load Dependencies
find_package(TRIQS)
find_package(triqs_tprf)

# Lift the relevant triqs_tprf targets out of the triqs_tprf namespace
add_library(triqs_tprf_c INTERFACE)
target_link_libraries(triqs_tprf_c INTERFACE triqs_tprf::triqs_tprf_c)
add_library(project_warnings INTERFACE)
target_link_libraries(project_warnings INTERFACE triqs_tprf::project_warnings)

enable_testing()

# The C++ tprf Tests
add_subdirectory(c++)

# The Python tprf Tests
if(${TRIQS_WITH_PYTHON_SUPPORT})
  add_subdirectory(python)
endif()
