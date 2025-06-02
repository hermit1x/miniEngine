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

# 设置 shader 拷贝目标目录
set(SHADER_DST_DIR ${CMAKE_BINARY_DIR}/shaders)

# 自定义目标，每次构建都运行（不缓存）
add_custom_target(copy_shaders ALL
    COMMAND ${CMAKE_COMMAND} -E remove_directory ${SHADER_DST_DIR}
    COMMAND ${CMAKE_COMMAND} -E copy_directory
            ${CMAKE_SOURCE_DIR}/src/shaders
            ${SHADER_DST_DIR}
    COMMENT "Copying src/shaders to build directory"
)
add_dependencies(miniEngine copy_shaders)

# Link glad and GLFW to the executable
target_link_libraries(miniEngine PRIVATE
    glad
    glfw
)