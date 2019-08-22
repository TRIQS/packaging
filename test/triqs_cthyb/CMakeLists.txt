# Start configuration
cmake_minimum_required(VERSION 2.8.12)
project(triqs_cthyb_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Load Dependencies
find_package(TRIQS)
find_package(triqs_cthyb)

# Lift the relevant triqs_cthyb targets out of the triqs_cthyb namespace
add_library(triqs_cthyb_c INTERFACE)
target_link_libraries(triqs_cthyb_c INTERFACE triqs_cthyb::triqs_cthyb_c)
add_library(project_warnings INTERFACE)
target_link_libraries(project_warnings INTERFACE triqs_cthyb::project_warnings)
add_library(nfft INTERFACE)
target_link_libraries(nfft INTERFACE triqs_cthyb::nfft)

enable_testing()

# The C++ Cthyb Tests
add_subdirectory(c++)

# The Python Cthyb Tests
if(${TRIQS_WITH_PYTHON_SUPPORT})
  add_subdirectory(python)
endif()
