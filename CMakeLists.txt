cmake_minimum_required(VERSION 3.10)
project(miniEngine)

# Add GLFW as a subdirectory
add_subdirectory(thirdparty/glfw-3.4)

# Create a library for glad
add_library(glad STATIC
    thirdparty/glad/src/glad.c
)
target_include_directories(glad PUBLIC
    thirdparty/glad/include
)

# Add the executable for main.cpp
add_executable(miniEngine
    src/main.cpp
)

# Include directories for main.cpp
target_include_directories(miniEngine PRIVATE
    thirdparty/glad/include
    thirdparty/glfw-3.4/include
)

# Link glad and GLFW to the executable
target_link_libraries(miniEngine PRIVATE
    glad
    glfw
)