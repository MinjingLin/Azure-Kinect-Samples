# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.
#set(glfw3_DIR "/media/min/d84b4767-6283-4b67-89ca-ed3fc4849fd5/azurekinect/Azure-Kinect-Samples/build/body-tracking-samples/extern/glfw/src/src")
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 99)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)

FIND_PACKAGE(k4a REQUIRED)
FIND_PACKAGE(k4abt REQUIRED)
FIND_PACKAGE(glfw3 REQUIRED) #add by lmj


# These specific settings tell the loader to search the directory of the
# executable for shared objects. This is done on Linux to emulate the default
# behavior of the Windows loader, which searches for DLLs in the path of the
# executable.
if ("${CMAKE_SYSTEM_NAME}" STREQUAL "Linux")
    set(CMAKE_BUILD_RPATH "\$ORIGIN")
endif()

# If using clang or GCC, set default visibilty to hidden
if ("${CMAKE_C_COMPILER_ID}" STREQUAL "GNU" OR "${CMAKE_C_COMPILER_ID}" STREQUAL "Clang")
    set(CMAKE_CXX_VISIBILITY_PRESET hidden)
    set(CMAKE_C_VISIBILITY_PRESET hidden)
    set(CMAKE_VISIBILITY_INLINES_HIDDEN 1)
endif()

# If using clang or GCC only linked shared libraries if needed
if ("${CMAKE_C_COMPILER_ID}" STREQUAL "GNU" OR "${CMAKE_C_COMPILER_ID}" STREQUAL "Clang")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--as-needed,-rpath-link=${CMAKE_LIBRARY_OUTPUT_DIRECTORY}")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--as-needed,-rpath-link=${CMAKE_LIBRARY_OUTPUT_DIRECTORY}")
endif()


#FIND_PACKAGE(window_controller_3d REQUIRED) #add by lmj

add_executable(simple_3d_viewer main.cpp)

target_include_directories(simple_3d_viewer PRIVATE ../sample_helper_includes)





# Dependencies of this library
link_libraries("/media/min/d84b4767-6283-4b67-89ca-ed3fc4849fd5/azurekinect/Azure-Kinect-Samples/body-tracking-samples/sample_helper_libs/window_controller_3d/build/CMakeFiles/window_controller_3d.dir" )
target_link_libraries(simple_3d_viewer PRIVATE 
    -Wl,--start-group
    k4a
    k4abt
    k4arecord
    window_controller_3d::window_controller_3d
    glfw::glfw
    -Wl,--end-group
    )

# 使用group可以解决静态库的顺序问题

