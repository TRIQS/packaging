# Start configuration
cmake_minimum_required(VERSION 3.3.2 FATAL_ERROR)
cmake_policy(VERSION 3.3.2)
if(POLICY CMP0074)
  cmake_policy(SET CMP0074 NEW)
endif()

project(triqs_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR})

# Load Dependencies
find_package(Cpp2Py)
find_package(TRIQS)

# Load Triqs specific test macros in the proper way
macro(add_all_subdirectories_with_cmakelist)
  file(GLOB ALLSUBS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} */CMakeLists.txt)
  foreach(f ${ALLSUBS})
     get_filename_component(d ${f} PATH)
     add_subdirectory(${d})
  endforeach()
endmacro()

set(MPIEXEC_EXECUTABLE		${TRIQS_MPIEXEC_EXECUTABLE} 	CACHE STRING "")
set(MPIEXEC_NUMPROC_FLAG	${TRIQS_MPIEXEC_NUMPROC_FLAG} 	CACHE STRING "")
set(MPIEXEC_PREFLAGS		${TRIQS_MPIEXEC_PREFLAGS} 	CACHE STRING "")
set(MPIEXEC_POSTFLAGS		${TRIQS_MPIEXEC_POSTFLAGS} 	CACHE STRING "")

link_libraries(triqs)

# Build googletest
add_subdirectory(/usr/src/gtest gtest EXCLUDE_FROM_ALL)

enable_testing()

# Create empty Interface target for compiler warnings
add_library(triqs_warnings INTERFACE)

# The C++ Triqs Tests
add_subdirectory(c++)

# The Python Triqs Tests
if(${TRIQS_WITH_PYTHON_SUPPORT})
  add_subdirectory(python)
endif()
