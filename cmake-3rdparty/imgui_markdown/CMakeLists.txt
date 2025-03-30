cmake_minimum_required (VERSION 3.5)
cmake_policy(VERSION 3.5)


project(imgui_markdown C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui_markdown"
)

# file(GLOB SRC_LIST
#     "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui_markdown/*.h"

#     )

add_library(${PROJECT_NAME} INTERFACE
)

target_include_directories(${PROJECT_NAME} INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui_markdown"
)
