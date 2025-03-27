cmake_minimum_required(VERSION 3.9)

project(Imgui-Treasure-Box LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

list (APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

message(STATUS "------------------------------------------------------------" )
message(STATUS "[${PROJECT_NAME}] Configuration summary."                     )
message(STATUS "------------------------------------------------------------ ")
message(STATUS " System configuration:"                                       )
message(STATUS " .. Processor type .............. = ${CMAKE_SYSTEM_PROCESSOR}")
message(STATUS " .. CMake executable ............ = ${CMAKE_COMMAND}"         )
message(STATUS " .. CMake version ............... = ${CMAKE_VERSION}"         )
message(STATUS " .. System name ................. = ${CMAKE_SYSTEM}"          )
message(STATUS " .. C++ compiler ................ = ${CMAKE_CXX_COMPILER}"    )
message(STATUS " .. C compiler .................. = ${CMAKE_C_COMPILER}"      )
message(STATUS " .. size(void*) ................. = ${CMAKE_SIZEOF_VOID_P}"   )
message(STATUS " .. cmake current source ................. = ${CMAKE_CURRENT_SOURCE_DIR}")
message(STATUS " .. cmake system name ................. = ${CMAKE_SYSTEM_NAME}")
message(STATUS "------------------------------------------------------------ ")

set(IMGUI_WINDOWSYSTEM "glfw" CACHE STRING "Window system support (options: glfw SDL)")
set_property(CACHE IMGUI_WINDOWSYSTEM PROPERTY STRINGS "glfw" "SDL")
set(IMGUI_RENDERERS "opengl3" CACHE STRING "renderers (options: opengl3)")
set_property(CACHE IMGUI_RENDERERS PROPERTY STRINGS "opengl3")

option(IMGUI_EXAMPLES "Build imgui examples" OFF)

include(Import3rdparty)

file(GLOB IMGUI_SOURCE
    "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/*.cpp"
)
import_subdirectory(
    SUB_NAME imgui
    SOURCES ${IMGUI_SOURCE} 
    TARGET_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui)

if(IMGUI_RENDERERS STREQUAL "opengl3")
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/renderers/imgui-opengl3)
else()
    message(FATAL_ERROR "No renderer available!")
endif()

if(IMGUI_WINDOWSYSTEM STREQUAL "glfw")
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/windowsystem/imgui-glfw)
elseif(IMGUI_WINDOWSYSTEM STREQUAL "SDL")
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/SDL)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/windowsystem/imgui-sdl3)
else()
    message(FATAL_ERROR "No window system available!")
endif()

if(IMGUI_EXAMPLES)
    if(IMGUI_RENDERERS STREQUAL "opengl3" AND IMGUI_WINDOWSYSTEM STREQUAL "glfw")
        file(GLOB EXAMPLES_SOURCE
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}_${IMGUI_RENDERERS}/*.h"
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}_${IMGUI_RENDERERS}/*.cpp"
        )
        import_executable(
            SUB_NAME "example_${IMGUI_WINDOWSYSTEM}_${IMGUI_RENDERERS}"
            SOURCES ${EXAMPLES_SOURCE}
            LINKLIB imgui imgui-glfw imgui-opengl3)
    elseif(IMGUI_RENDERERS STREQUAL "opengl3" AND IMGUI_WINDOWSYSTEM STREQUAL "SDL")
        file(GLOB EXAMPLES_SOURCE
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}3_${IMGUI_RENDERERS}/*.h"
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}3_${IMGUI_RENDERERS}/*.cpp"
        )
        import_executable(
            SUB_NAME "example_${IMGUI_WINDOWSYSTEM}3_${IMGUI_RENDERERS}"
            SOURCES ${EXAMPLES_SOURCE}
            LINKLIB imgui SDL3::SDL3 imgui-sdl3 imgui-opengl3)
    endif()
endif()

