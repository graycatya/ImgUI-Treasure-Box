cmake_minimum_required (VERSION 3.0)
cmake_policy(VERSION 3.0)


project(imgui-glfw C CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

# set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules/")
list (APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules/")

include (FindGLFW3)
# if(WIN32)
# 	set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)
# endif(WIN32)


include_directories(${CMAKE_CURRENT_SOURCE_DIR}/includes)


message("CMAKE_MODULE_PATH: " ${CMAKE_MODULE_PATH})

find_package(GLFW3 REQUIRED)
message(STATUS "Found GLFW3 in ${GLFW3_INCLUDE_DIR} - ${GLFW3_LIBRARY}")

link_directories(${CMAKE_CURRENT_SOURCE_DIR}/libs)

file(GLOB SRC_LIST
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_glfw*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends/imgui_impl_glfw*.cpp"
)

add_library(${PROJECT_NAME} STATIC
    ${SRC_LIST}
)

target_link_libraries(${PROJECT_NAME} PUBLIC ${LIBS} imgui)

target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/../../3rdparty/imgui/backends"
    ${GLFW3_INCLUDE_DIR}
)
