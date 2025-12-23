include(CheckIncludeFiles)
include(cmake/get_cpm.cmake)

cmake_minimum_required(VERSION 3.10)
project(GlyphGL VERSION 1.0.9)

option(GLYPHGL_BUILD_DEMOS OFF)
option(GLYPHGL_BUILD_EXAMPLES OFF)

add_library(GlyphGL INTERFACE)
target_include_directories(GlyphGL INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")

CPMAddPackage(
    NAME GLFW
    VERSION 3.4
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)

CPMAddPackage(
    NAME GLUT
    VERSION 3.8.0
    GIT_REPOSITORY https://github.com/freeglut/freeglut.git
    GIT_TAG v3.8.0
)

check_include_file("GLFW/glfw3.h" HAS_GLFW3)
check_include_file("GL/glut.h" HAS_GLUT)
check_include_file("windows.h" HAS_WINDOWS_H)
check_include_files("unistd.h;X11/Xlib.h;X11/Xutil.h;GL/glx.h" HAS_X11)

if (GLYPHGL_BUILD_DEMOS)
    if (HAS_GLFW3)
        add_executable(glyphgl_hello_world demos/hello_world.cpp)
        target_link_libraries(glyphgl_hello_world PRIVATE GlyphGL glfw freeglut_static)

        add_executable(glyphgl_rainbow demos/rainbow.cpp)
        target_link_libraries(glyphgl_rainbow PRIVATE GlyphGL glfw freeglut_static)

        add_executable(glyphgl_timer_demo demos/timer_demo.cpp)
        target_link_libraries(glyphgl_timer_demo PRIVATE GlyphGL glfw freeglut_static)
    endif()
endif()

if (GLYPHGL_BUILD_EXAMPLES)
    if (HAS_GLFW3)
        add_executable(glyphgl_glfw_example examples/glfw_example.cpp)
        target_link_libraries(glyphgl_glfw_example PRIVATE GlyphGL glfw freeglut_static)
    endif()

    if (HAS_GLUT)
        add_executable(glyphgl_glut_example examples/glut_example.cpp)
        target_link_libraries(glyphgl_glut_example PRIVATE GlyphGL glfw freeglut_static)
    endif()

    if (HAS_WINDOWS)
        add_executable(glyphgl_winapi_example examples/winapi_example.cpp)
        target_link_libraries(glyphgl_winapi_example PRIVATE GlyphGL glfw freeglut_static)
    endif()

    if (HAS_X11)
        add_executable(glyphgl_x11_example examples/x11_example.cpp)
        target_link_libraries(glyphgl_x11_example PRIVATE GlyphGL glfw freeglut_static)
    endif()
endif()