cmake_minimum_required (VERSION 3.5)
cmake_policy(VERSION 3.5)


project(IconFontCppHeaders C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/IconFontCppHeaders"
)

# file(GLOB SRC_LIST
#     "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/IconFontCppHeaders/*.h"

#     )

add_library(${PROJECT_NAME} INTERFACE
)

target_include_directories(${PROJECT_NAME} INTERFACE
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/IconFontCppHeaders"
)
