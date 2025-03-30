cmake_minimum_required (VERSION 3.5)
cmake_policy(VERSION 3.5)


project(imgui-node-editor C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-node-editor/external/imgui"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-node-editor/external/stb_image"
)

file(GLOB SRC_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-node-editor/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-node-editor/*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-node-editor/external/imgui/*.cpp"
    )

add_library(${PROJECT_NAME} STATIC
    ${SRC_LIST}
)

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui-node-editor"
)
