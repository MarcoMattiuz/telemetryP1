cmake_minimum_required(VERSION 3.21)
project(Project1 VERSION 1.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-pthread -g -rdynamic -std=c++1z -DGLEW_STATIC -DNDEBUG -w")

find_package(glfw3 3.3 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)


include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/imgui
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/implot
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/hash-library

)

FILE(GLOB imgui_srcs ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/imgui/*)
add_library(
    ${PROJECT_NAME}
    SHARED

    # Sources
    ${CMAKE_CURRENT_SOURCE_DIR}/core/app.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/core/components/graphs.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/core/components/globals.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/core/components/login.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/core/tabItems/userstab.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/core/tabItems/graphstab.cpp


    
    # GUI
    ${imgui_srcs}
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/imgui/backends/imgui_impl_glfw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/imgui/backends/imgui_impl_opengl2.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/imgui/misc/cpp/imgui_stdlib.cpp

    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/implot/implot.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/implot/implot_items.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/implot/implot_demo.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/hash-library/sha256.cpp



)

target_link_libraries(${PROJECT_NAME}
    glfw
    OpenGL::GL
    GLEW::GLEW

    dl
)
if(NOT APPLE)
    target_link_libraries(${PROJECT_NAME} stdc++fs)
endif()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
add_executable(project_1 core/main.cpp thirdparty/csv.h)
target_link_libraries(project_1 ${PROJECT_NAME})
