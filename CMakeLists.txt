cmake_minimum_required(VERSION 3.21)
project(rlImGui)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS NO)

include(FetchContent)
FetchContent_Declare(
    raylib
    GIT_REPOSITORY https://github.com/lukekras/raylib.git
    GIT_TAG 7584ce6f48ae58c625fce1718319d7d046503c41
)
FetchContent_MakeAvailable(raylib)

FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/lukekras/imgui.git
    GIT_TAG 0f14933577a1de01d90f8e87622296c466146f21
)

FetchContent_GetProperties(imgui)
if (NOT imgui_POPULATED)
    FetchContent_Populate(imgui)
endif ()
add_library(imgui INTERFACE)
target_include_directories(imgui INTERFACE ${imgui_SOURCE_DIR})
target_sources(imgui INTERFACE
        ${imgui_SOURCE_DIR}/imgui.cpp
        ${imgui_SOURCE_DIR}/imgui_draw.cpp
        ${imgui_SOURCE_DIR}/imgui_widgets.cpp
        ${imgui_SOURCE_DIR}/imgui_tables.cpp
        ${imgui_SOURCE_DIR}/imgui_demo.cpp
)

add_library(RlImGui INTERFACE)
target_include_directories(RlImGui
    INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
)
target_sources(RlImGui INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/rlImGui.cpp
)

add_subdirectory(examples)