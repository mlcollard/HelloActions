# @file CMakeLists.txt
# 
# CMake files for the hello project

# Can be run by any version of CMake >= this
cmake_minimum_required(VERSION 3.18)

# project info
project(Hello)

# C++17 standard
set(CMAKE_CXX_STANDARD 17)

# Use Release build type if not specified
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

# Hello application
add_executable(hello hello.cpp)

# Turn on warnings
add_compile_options(
    $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-Wall>
    $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-Wextra>
    $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-pedantic>
    $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-ansi>

    $<$<CXX_COMPILER_ID:MSVC>:/Wall>
)

# Hello run command
add_custom_target(run
        COMMENT "Run hello"
        COMMAND ./hello
        DEPENDS hello
        USES_TERMINAL
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

# Setup testing
enable_testing()

# Oracle
file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/oracle.txt "Hello Action!\n")

# Test
add_test(NAME CheckOutput
    COMMAND bash -c "./hello > output.txt && diff oracle.txt output.txt"
)
