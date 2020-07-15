add_executable(offline_processor main.cpp)
target_include_directories(offline_processor PRIVATE ../sample_helper_includes)
target_link_libraries(offline_processor PRIVATE 
    k4a
    k4abt
    k4arecord
    window_controller_3d::window_controller_3d
    glfw::glfw
    )
