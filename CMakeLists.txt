cmake_minimum_required(VERSION 3.1)

project(cimguizmo)

set (CMAKE_CXX_STANDARD 11)

#general settings
file(GLOB IMGUI_SOURCES
    cimguizmo.cpp
    ImGuizmo/ImGuizmo.cpp
)

set(IMGUIZMO_STATIC "no" CACHE STRING "Build as a static library")
set(IMGUIZMO_CIMGUI CACHE STRING "Where cimgui is")

if(NOT IMGUIZMO_CIMGUI)
    message(FATAL_ERROR "IMGUIZMO_CIMGUI must be set!")
endif(NOT IMGUIZMO_CIMGUI)

#add library and link
if (IMGUIZMO_STATIC)
    add_library(cimguizmo STATIC ${IMGUI_SOURCES})
else (IMGUIZMO_STATIC)
    add_library(cimguizmo SHARED ${IMGUI_SOURCES})
endif (IMGUIZMO_STATIC)

target_compile_definitions(cimguizmo PUBLIC IMGUI_DISABLE_OBSOLETE_FUNCTIONS=1)
if (WIN32)
    target_compile_definitions(cimguizmo PUBLIC IMGUI_IMPL_API=extern\t\"C\"\t__declspec\(dllexport\))
else (WIN32)
    target_compile_definitions(cimguizmo PUBLIC IMGUI_IMPL_API=extern\t\"C\"\t)
endif (WIN32)

target_include_directories(cimguizmo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(cimguizmo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/ImGuizmo)
target_include_directories(cimguizmo PUBLIC ${IMGUIZMO_CIMGUI})
target_include_directories(cimguizmo PUBLIC ${IMGUIZMO_CIMGUI}/imgui/)

set_target_properties(cimguizmo PROPERTIES PREFIX "")

#install
install(TARGETS cimguizmo
    RUNTIME DESTINATION  .
    LIBRARY DESTINATION  .
    ARCHIVE DESTINATION  .
)
