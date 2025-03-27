cmake_minimum_required (VERSION 3.0)
cmake_policy(VERSION 3.0)


project(imgui-sdl3 C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)


include_directories(${CMAKE_CURRENT_SOURCE_DIR}/includes)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/includes)



link_directories(${CMAKE_CURRENT_SOURCE_DIR}/libs)

file(GLOB SRC_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_sdl3*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_sdl3*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_sdlgpu3*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_sdlgpu3*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_sdlrenderer3*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_sdlrenderer3*.cpp"
)

add_library(${PROJECT_NAME} STATIC
    ${SRC_LIST}
)

target_link_libraries(${PROJECT_NAME} PUBLIC ${LIBS} imgui SDL3::SDL3)

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends"
)
