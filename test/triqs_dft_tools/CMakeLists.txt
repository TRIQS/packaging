# Start configuration
cmake_minimum_required(VERSION 2.8.12)
project(triqs_dft_tools_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Load Dependencies
find_package(TRIQS)
#find_package(triqs_dft_tools)
#link_libraries(atm_c)

enable_testing()

# The dft_tools tests
add_subdirectory(test)
