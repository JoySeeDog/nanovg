cmake_minimum_required(VERSION 3.10)
project(nanovg)


if (APPLE)
    # Qt 5.9.0 do not support arm64 currently.
    set(CMAKE_OSX_ARCHITECTURES x86_64)
    set(OS_BUNDLE MACOSX_BUNDLE)
elseif (WIN32)
    set(OS_BUNDLE WIN32)
endif(APPLE)

# Set build type if not set.
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_C_STANDARD 99)

find_package(OpenGL REQUIRED)
find_package(glfw3 3.3 REQUIRED)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)

set(NANOVG_SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/src/nanovg.c
        )

add_library(nanovg ${NANOVG_SOURCES})
target_link_libraries(nanovg ${OPENGL_gl_LIBRARY})

add_executable(example
        ${CMAKE_CURRENT_SOURCE_DIR}/example/demo.c
        ${CMAKE_CURRENT_SOURCE_DIR}/example/perf.c
        ${CMAKE_CURRENT_SOURCE_DIR}/example/example_gl3.c
#        ${CMAKE_CURRENT_SOURCE_DIR}/example/example_gl2.c
        )
target_link_libraries(example glfw nanovg ${OPENGL_gl_LIBRARY} m)
