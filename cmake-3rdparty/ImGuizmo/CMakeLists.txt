cmake_minimum_required (VERSION 3.5)
cmake_policy(VERSION 3.5)


project(ImGuizmo C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)


file(GLOB SRC_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/ImGuizmo/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/ImGuizmo/*.cpp"
    )

add_library(${PROJECT_NAME} STATIC
    ${SRC_LIST}
)

target_link_libraries(${PROJECT_NAME} imgui)

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/ImGuizmo"
)
