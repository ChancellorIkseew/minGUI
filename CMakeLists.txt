cmake_minimum_required(VERSION 3.10)
project(MINGUI LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

file(GLOB_RECURSE SOURCES "*.cpp")
file(GLOB_RECURSE HEADERS "*.hpp")

add_library(mingui STATIC ${SOURCES} ${HEADERS})

target_include_directories(mingui PRIVATE include)
