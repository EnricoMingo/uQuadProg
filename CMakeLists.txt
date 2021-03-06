project(uQuadProg)
cmake_minimum_required(VERSION 3.0)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
include(MacroInstallLib)

find_package(Eigen3 REQUIRED)

# C++ 11
include(CheckCXXCompilerFlag)
check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
check_cxx_compiler_flag("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

include_directories(include ${EIGEN3_INCLUDE_DIRS})

add_library(uQuadProg SHARED src/uquadprog.cpp)
add_library(eigQuadProg SHARED src/eiquadprog.cpp)

add_executable(uQuadProgExample src/example.C)
target_link_libraries(uQuadProgExample uQuadProg)

library_install(uQuadProg 1 0 0)
library_install(eigQuadProg 1 0 0)
