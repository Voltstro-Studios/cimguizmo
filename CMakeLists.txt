cmake_minimum_required(VERSION 3.1)

project(cimguizmo)

set (CMAKE_CXX_STANDARD 11)

#General Settings
file(GLOB IMGUI_SOURCES
    cimguizmo.cpp
    ImGuizmo/ImGuizmo.cpp
)

set(IMGUIZMO_STATIC "no" CACHE STRING "Build as a static library")
set(IMGUIZMO_CIMGUI CACHE STRING "Where cimgui is")
set(IMGUIZMO_CIMGUI_OUTPUT "${IMGUIZMO_CIMGUI}/build/Release/" CACHE STRING "Where cimgui output is")

if(NOT IMGUIZMO_CIMGUI)
    message(FATAL_ERROR "IMGUIZMO_CIMGUI must be set!")
endif(NOT IMGUIZMO_CIMGUI)

#Add library
add_library(cimguizmo SHARED ${IMGUI_SOURCES})
add_library(cimgui SHARED IMPORTED)

target_compile_definitions(cimguizmo PUBLIC IMGUI_DISABLE_OBSOLETE_FUNCTIONS=1)
if (WIN32)
    set_property(TARGET cimgui PROPERTY IMPORTED_LOCATION ${IMGUIZMO_CIMGUI_OUTPUT}cimgui.dll)
    set_property(TARGET cimgui PROPERTY IMPORTED_IMPLIB ${IMGUIZMO_CIMGUI_OUTPUT}cimgui.lib)
    target_compile_definitions(cimguizmo PUBLIC IMGUI_IMPL_API=extern\t\"C\"\t__declspec\(dllexport\))
elseif (UNIX)
    set_property(TARGET cimgui PROPERTY IMPORTED_LOCATION ${IMGUIZMO_CIMGUI_OUTPUT}cimgui.so)
    target_compile_definitions(cimguizmo PUBLIC IMGUI_IMPL_API=extern\t\"C\"\t)
elseif (APPLE)
    set_property(TARGET cimgui PROPERTY IMPORTED_LOCATION ${IMGUIZMO_CIMGUI_OUTPUT}cimgui.dylib)
    target_compile_definitions(cimguizmo PUBLIC IMGUI_IMPL_API=extern\t\"C\"\t)
endif (WIN32)

target_include_directories(cimguizmo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(cimguizmo PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/ImGuizmo)
target_include_directories(cimguizmo PUBLIC ${IMGUIZMO_CIMGUI})
target_include_directories(cimguizmo PUBLIC ${IMGUIZMO_CIMGUI}/imgui/)

target_link_libraries(cimguizmo cimgui)

set_target_properties(cimguizmo PROPERTIES PREFIX "")

#install
install(TARGETS cimguizmo
    RUNTIME DESTINATION  .
    LIBRARY DESTINATION  .
    ARCHIVE DESTINATION  .
)
