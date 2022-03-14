# Project root level CMakeLists.txt must have cmake_minimum_required(VERSION <cmake_version>) and project(<project_name>)
# This project should be embeddable in a larger project.
cmake_minimum_required(VERSION 3.21)
project(rlImGui)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS NO)

# include FetchContent_* cmake functions
include(FetchContent)

# declare that raylib is an external repository and linked to the stated git commit.
FetchContent_Declare(
    raylib
    GIT_REPOSITORY https://github.com/lukekras/raylib.git
    GIT_TAG 7584ce6f48ae58c625fce1718319d7d046503c41
)
# raylib supports cmake, and so can expose raylib headers and libs via FetchContent_MakeAvailable
FetchContent_MakeAvailable(raylib)

# declare that imgui is an external repository and linked to the stated git commit.
FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/lukekras/imgui.git
    GIT_TAG 0f14933577a1de01d90f8e87622296c466146f21
)

# imgui doesn't support cmake, so we have to exposed headers and libs more manually. Checkout imgui, if not done already (via FetchContent_Populate)
FetchContent_GetProperties(imgui)
if (NOT imgui_POPULATED)
    FetchContent_Populate(imgui)
endif ()

# create imgui target (INTERFACE library) that exposes headers and source (imgui_SOURCE_DIR).
add_library(imgui INTERFACE)

# add headers to imgui INTERFACE library
target_include_directories(imgui INTERFACE ${imgui_SOURCE_DIR})

# add source to imgui INTERFACE library
target_sources(imgui INTERFACE
        ${imgui_SOURCE_DIR}/imgui.cpp
        ${imgui_SOURCE_DIR}/imgui_draw.cpp
        ${imgui_SOURCE_DIR}/imgui_widgets.cpp
        ${imgui_SOURCE_DIR}/imgui_tables.cpp
        ${imgui_SOURCE_DIR}/imgui_demo.cpp
)

# create RlImGui target (INTERFACE library) that exposes headers and source.
add_library(RlImGui INTERFACE)

# add header directory to RlImGui INTERFACE library
target_include_directories(RlImGui
    INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
)

# add source to RlImGui INTERFACE library
target_sources(RlImGui INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/rlImGui.cpp
)

# run cmake in examples directory (assumes there is a CMakeLists.txt in the subdirectory)
add_subdirectory(examples)