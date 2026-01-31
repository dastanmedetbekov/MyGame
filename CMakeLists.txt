cmake_minimum_required(VERSION 3.16)
project(MyGame VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Find SFML
find_package(SFML 2.6 COMPONENTS system window graphics audio REQUIRED)

# Include directories
include_directories(${CMAKE_SOURCE_DIR}/include)

# Source files
set(SOURCES
    src/main.cpp
    src/Rendering/FireEffect.cpp
    src/Rendering/SectorRenderer.cpp
    src/States/MenuState.cpp
)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCES})

# Link libraries
target_link_libraries(${PROJECT_NAME} PRIVATE 
    sfml-system
    sfml-window
    sfml-graphics
    sfml-audio
)

# Copy assets to build directory
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${CMAKE_SOURCE_DIR}/assets
    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/assets
)

# Platform-specific settings
if(WIN32)
    # Windows-specific settings
    set_target_properties(${PROJECT_NAME} PROPERTIES WIN32_EXECUTABLE TRUE)
    
    # Copy SFML DLLs to output directory on Windows
    if(SFML_FOUND)
        foreach(LIB system window graphics audio)
            add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy_if_different
                $<TARGET_FILE:sfml-${LIB}>
                $<TARGET_FILE_DIR:${PROJECT_NAME}>
            )
        endforeach()
    endif()
elseif(UNIX)
    # Linux-specific settings
    target_link_libraries(${PROJECT_NAME} PRIVATE pthread)
endif()

# Installation rules
install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/assets DESTINATION bin)
