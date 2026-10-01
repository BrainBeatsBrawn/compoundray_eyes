#
# Example .eye file generation for compoundray. eye files are CSV
# formatted text containing ommatidia position, direction, acceptance
# angle and fovea offset. Here are some programs to generate (on
# stdout) some example eye files for cylinders, spheres etc.
#

cmake_minimum_required(VERSION 3.28.5 FATAL_ERROR)

project(compoundray_eyes LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(MATHS_SOURCE_DIR "${PROJECT_SOURCE_DIR}/extern/maths" CACHE PATH "The path to sebsjames/maths")
set(JSON_SOURCE_DIR "${PROJECT_SOURCE_DIR}/extern/json" CACHE PATH "The path to nlohmann/json")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -g -O3 -Wno-unused-result -Wno-unknown-pragmas -Wfatal-errors")

include(${MATHS_SOURCE_DIR}/cmake/module_definitions.cmake)
setup_module_variables_for_maths (${MATHS_SOURCE_DIR} ${JSON_SOURCE_DIR})

set(MY_MODULES
  ${SM_VEC_MODULES}
  ${SM_GEOMETRY_POLYHEDRA_MODULES}
)
list(REMOVE_DUPLICATES MY_MODULES)

# Add a library of the modules we'll use from sebsjames/maths
add_library(local_modules SHARED)
target_sources(local_modules
  PUBLIC FILE_SET CXX_MODULES BASE_DIRS ${PROJECT_SOURCE_DIR}
  FILES ${MY_MODULES}
)

# Cylindrical eye
add_executable(cyl cyl.cpp)
target_link_libraries(cyl local_modules)

# Spherical polyhedral eye
add_executable(poly poly.cpp)
target_link_libraries(poly local_modules)
