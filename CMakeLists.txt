cmake_minimum_required(VERSION 3.30)

project(
  Hello
  VERSION 0.1
  DESCRIPTION "13. Forwarding Reference and Perfect Forwarding"
  LANGUAGES CXX)

SET(CMAKE_C_COMPILER "/opt/homebrew/Cellar/llvm/18.1.8/bin/clang")
SET(CMAKE_CXX_COMPILER "/opt/homebrew/Cellar/llvm/18.1.8/bin/clang++")
SET(CMAKE_CXX_STANDARD 20)
SET(CMAKE_EXPORT_COMPILE_COMMANDS ON)
SET(CMAKE_BUILD_TYPE Debug)

# Save file list to ${sources}
file(GLOB_RECURSE sources
      "${CMAKE_SOURCE_DIR}/src/*.c"
      "${CMAKE_SOURCE_DIR}/src/*.cpp")

# Add precompiler definitions
#add_definitions(-DSOME_DEFINITION)

add_executable(main ${sources})

# Add more include directories if needed
#target_include_directories(hello PRIVATE "${CMAKE_SOURCE_DIR)/include")
