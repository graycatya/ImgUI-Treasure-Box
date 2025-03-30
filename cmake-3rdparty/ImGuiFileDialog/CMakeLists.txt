cmake_minimum_required (VERSION 3.5)
cmake_policy(VERSION 3.5)


project(ImGuiFileDialog C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)


file(GLOB SRC_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/ImGuiFileDialog/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/ImGuiFileDialog/*.cpp"
    )

add_library(${PROJECT_NAME} STATIC
    ${SRC_LIST}
)

target_link_libraries(${PROJECT_NAME} stb imgui)

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/ImGuiFileDialog"
)
