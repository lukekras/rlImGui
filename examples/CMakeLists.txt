# Build examples simple and editor. Currently only tested on MacOS.

# Simple Example
set(EXAMPLE_SIMPLE_APP_NAME simple)
add_executable(${EXAMPLE_SIMPLE_APP_NAME} simple.cpp)
target_include_directories(${EXAMPLE_SIMPLE_APP_NAME} PRIVATE ${imgui_SOURCE_DIR})
target_link_libraries(${EXAMPLE_SIMPLE_APP_NAME} PRIVATE
    raylib
    imgui
    RlImGui
)

if (APPLE)
    target_link_libraries(${EXAMPLE_SIMPLE_APP_NAME} PRIVATE
        "-framework IOKit"
        "-framework Cocoa"
        "-framework OpenGL"
    )
endif ()

# Editor Example
set(EXAMPLE_EDITOR_APP_NAME Editor)

# Resources files list
set(RESOURCE_FILES
    resources/parrots.png
    imgui.ini
)

# Executable target
add_executable(${EXAMPLE_EDITOR_APP_NAME}
    editor.cpp
    ${RESOURCE_FILES}
)

# imgui is not a cmake project, so we need to add it's header directory
target_include_directories(${EXAMPLE_EDITOR_APP_NAME} PRIVATE
    ${imgui_SOURCE_DIR}
)

# raylib is accessible via FetchContent, and more specifically FetchContent_MakeAvailable
# imgui is accessible via FetchContent. imgui doesn't support cmake, so it is accessible via target_include_directories(INTERFACE) and target_sources(INTERFACE).
# RlImGui is accessible via add_library(INTERFACE), target_include_directories(INTERFACE) and target_sources(INTERFACE).
target_link_libraries(${EXAMPLE_EDITOR_APP_NAME} PRIVATE
    raylib
    imgui
    RlImGui
)

if (APPLE)
    # Make a MacOS bundle to include resources with application. TODO: include resources for Linux and Windows
    set_target_properties(${EXAMPLE_EDITOR_APP_NAME} PROPERTIES
        MACOSX_BUNDLE TRUE
        MACOSX_FRAMEWORK_IDENTIFIER org.cmake.ExecutableTarget
        RESOURCE "${RESOURCE_FILES}"
    )
    target_link_libraries(${EXAMPLE_EDITOR_APP_NAME} PRIVATE
        "-framework IOKit"
        "-framework Cocoa"
        "-framework OpenGL"
    )
endif ()