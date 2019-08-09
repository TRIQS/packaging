# Start configuration
cmake_minimum_required(VERSION 2.8.12)
project(cthyb_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Load Dependencies
find_package(TRIQS)
find_package(triqs_cthyb)
link_libraries(triqs_cthyb_c)

enable_testing()

# The C++ Cthyb Tests
add_subdirectory(c++)

# The Python Cthyb Tests
if(${TRIQS_WITH_PYTHON_SUPPORT})
  add_subdirectory(python)
endif()
