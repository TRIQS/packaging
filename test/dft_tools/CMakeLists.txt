# Start configuration
cmake_minimum_required(VERSION 2.8.12)
project(dft_tools_tests CXX)

# Default to Release build type
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Type of build" FORCE)
endif()
message( STATUS "-------- BUILD-TYPE: ${CMAKE_BUILD_TYPE} --------")

# Global options and definitions
add_compile_options(
	-std=c++17
	$<$<CONFIG:Debug>:-Og>
	)

# Load Dependencies
find_package(Cpp2Py)
find_package(TRIQS)
link_libraries(triqs)

enable_testing()


# Copy h5 files to binary dir
FILE(GLOB all_h5_files RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.h5)
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/${all_h5_files} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
# Copy other files
FILE(COPY SrVO3.pmat SrVO3.struct SrVO3.outputs SrVO3.oubwin SrVO3.ctqmcout SrVO3.symqmc SrVO3.sympar SrVO3.parproj SrIrO3_rot.h5 hk_convert_hamiltonian.hk LaVO3-Pnma_hr.dat LaVO3-Pnma.inp DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

# List all tests
set(all_tests wien2k_convert hk_convert w90_convert sumkdft_basic srvo3_Gloc srvo3_transp sigma_from_file blockstructure analyze_block_structure_from_gf analyze_block_structure_from_gf2)

foreach(t ${all_tests})
  add_test(NAME ${t} COMMAND python ${CMAKE_CURRENT_SOURCE_DIR}/${t}.py)
endforeach()

# Set the PythonPath : put the build dir first (in case there is an installed version). 
set_property(TEST ${all_tests} PROPERTY ENVIRONMENT PYTHONPATH=${CMAKE_BINARY_DIR}/python:$ENV{PYTHONPATH} )

# VASP converter tests
add_subdirectory(plovasp)
