cmake_minimum_required(VERSION 3.21)

# Define the STF project
project(stf
        VERSION 1.0.0.0
        DESCRIPTION "Simple Test Framework Library"
        LANGUAGES CXX)

# Set options depending on whether this is a subproject
if(PROJECT_IS_TOP_LEVEL)
    # Option to control whether tests are built
    option(stf_BUILD_TESTS "Build Tests for Simple Test Framework Library" ON)
else()
    # Option to control whether tests are built
    option(stf_BUILD_TESTS "Build Tests for Simple Test Framework Library" OFF)
endif()

# Option to control ability to install the library
option(stf_INSTALL "Install the STF Library" ON)

# By default, library built for C++20; set stf_CPP_STD to 11, 14, 17, or 20
if(NOT DEFINED stf_CPP_STD)
    set(stf_CPP_STD 20 CACHE STRING "C++ Version for Testing with STF")
endif()

# Include the source directory
add_subdirectory(src)

# Include CTest, which will set BUILD_TESTING
include(CTest)

# Build tests if conditions are met
if(BUILD_TESTING AND stf_BUILD_TESTS)
    add_subdirectory(test)
endif()
