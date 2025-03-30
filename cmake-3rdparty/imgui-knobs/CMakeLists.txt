cmake_minimum_required (VERSION 3.5)
cmake_policy(VERSION 3.5)


project(imgui-knobs C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-knobs"
)

file(GLOB SRC_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-knobs/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-knobs/*.cpp"
    )

add_library(${PROJECT_NAME} STATIC
    ${SRC_LIST}
)

target_link_libraries(${PROJECT_NAME} imgui)

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-knobs"
)
