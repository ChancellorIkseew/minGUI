cmake_minimum_required(VERSION 3.10)
project(MINGUI LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

file(GLOB_RECURSE SOURCES "MINGUI/*.cpp")
file(GLOB_RECURSE HEADERS "MINGUI/*.hpp")

add_library(mingui STATIC ${SOURCES} ${HEADERS})
target_include_directories(mingui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if(WIN32)
    set_target_properties(mingui PROPERTIES OUTPUT_NAME "mingui_win64")
else()
    set_target_properties(mingui PROPERTIES OUTPUT_NAME "mingui_linux")
endif()
