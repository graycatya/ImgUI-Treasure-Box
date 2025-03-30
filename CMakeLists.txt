cmake_minimum_required(VERSION 3.9)

project(Imgui-Treasure-Box LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
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
set(IMGUI_RENDERERS "opengl3" CACHE STRING "renderers (options: opengl3 vulkan)")
set_property(CACHE IMGUI_RENDERERS PROPERTY STRINGS "opengl3" "vulkan")

option(IMGUI_EXAMPLES "Build imgui examples" OFF)

option(IMGUI_ZMO "Build ImGuizmo" ON)
option(IMGUI_FILEDIALOG "Build ImguiFileDialog" ON)
option(IMGUI_IMPLOT "Build implot" ON)
option(IMGUI_NODE_EDITOR "Build imgui-node-editor" ON)
option(IMGUI_MARKDOWN "Build imgui-markdown" ON)
option(IMGUI_ICONFONTCPPHEADERS "Build IconFontCppHeaders" ON)
option(IMGUI_KNOBS "Build imgui-knobs" ON)
option(IMGUI_IMGUICOLORTEXTEDIT  "Build ImGuiColorTextEdit" ON)

include(Import3rdparty)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/glm)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/stb)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/imgui)

if(IMGUI_ZMO)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/ImGuizmo)
endif()

if(IMGUI_ZMO)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/implot)
endif()

if(IMGUI_NODE_EDITOR)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/imgui-node-editor)
endif()

if(IMGUI_FILEDIALOG)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/ImGuiFileDialog)
endif()

if(IMGUI_MARKDOWN)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/imgui_markdown)
endif()

if(IMGUI_ICONFONTCPPHEADERS)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/IconFontCppHeaders)
endif()

if(IMGUI_KNOBS)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/imgui-knobs)
endif()

if(IMGUI_IMGUICOLORTEXTEDIT)
    #add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/cmake-3rdparty/ImGuiColorTextEdit)
endif()



if(IMGUI_RENDERERS STREQUAL "opengl3")
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/renderers/imgui-opengl3)
elseif(IMGUI_RENDERERS STREQUAL "vulkan")
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/renderers/imgui-vulkan)
else()
    message(FATAL_ERROR "No renderer available!")
endif()

if(IMGUI_WINDOWSYSTEM STREQUAL "glfw")
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/glfw)
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
    elseif(IMGUI_RENDERERS STREQUAL "vulkan" AND IMGUI_WINDOWSYSTEM STREQUAL "glfw")
        file(GLOB EXAMPLES_SOURCE
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}_${IMGUI_RENDERERS}/*.h"
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}_${IMGUI_RENDERERS}/*.cpp"
        )
        import_executable(
            SUB_NAME "example_${IMGUI_WINDOWSYSTEM}_${IMGUI_RENDERERS}"
            SOURCES ${EXAMPLES_SOURCE}
            LINKLIB imgui imgui-glfw imgui-vulkan)
    elseif(IMGUI_RENDERERS STREQUAL "vulkan" AND IMGUI_WINDOWSYSTEM STREQUAL "SDL")
        file(GLOB EXAMPLES_SOURCE
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}3_${IMGUI_RENDERERS}/*.h"
            "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/imgui/examples/example_${IMGUI_WINDOWSYSTEM}3_${IMGUI_RENDERERS}/*.cpp"
        )
        import_executable(
            SUB_NAME "example_${IMGUI_WINDOWSYSTEM}3_${IMGUI_RENDERERS}"
            SOURCES ${EXAMPLES_SOURCE}
            LINKLIB imgui SDL3::SDL3 imgui-sdl3 imgui-vulkan)
    else()
    endif()

    if(IMGUI_KNOBS)
        if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/examples/example-glfw-opengl3-imgui-knobs" AND IS_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/examples/example-glfw-opengl3-imgui-knobs")
            file(GLOB EXAMPLES_SOURCE
                "${CMAKE_CURRENT_SOURCE_DIR}/examples/example-${IMGUI_WINDOWSYSTEM}-${IMGUI_RENDERERS}-imgui-knobs/*.h"
                "${CMAKE_CURRENT_SOURCE_DIR}/examples/example-${IMGUI_WINDOWSYSTEM}-${IMGUI_RENDERERS}-imgui-knobs/*.cpp"
            )
            import_executable(
                SUB_NAME "example-${IMGUI_WINDOWSYSTEM}-${IMGUI_RENDERERS}-imgui-knobs"
                SOURCES ${EXAMPLES_SOURCE}
                LINKLIB imgui imgui-glfw imgui-opengl3 imgui-knobs)
        endif()
    endif()

    # if(IMGUI_ZMO)
    #     file(GLOB IMGUIZMO_EXAMPLE_SOURCE
    #         "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/ImGuizmo/example/ImApp.h"
    #         "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/ImGuizmo/example/main.cpp"
    #     )
    #     import_executable(
    #         SUB_NAME "example_imguizmo"
    #         SOURCES ${IMGUIZMO_EXAMPLE_SOURCE}
    #         #TARGET_INCLUDE "${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/ImGuizmo/example"
    #         LINKLIB stb imgui ImGuizmo)
    # endif()

endif()


