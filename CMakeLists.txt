cmake_minimum_required(VERSION 3.20)

project(
    LearnOpenGL
    VERSION 0.1.0
    LANGUAGES CXX C
)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-c2y-extensions")
endif()

# System GLFW
find_package(PkgConfig REQUIRED)
pkg_check_modules(GLFW REQUIRED glfw3)

# GLAD
add_library(glad STATIC
    glad/src/glad.c
)
target_include_directories(glad PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/glad/include
)

add_executable(App)

target_include_directories(App PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${GLFW_INCLUDE_DIRS}
)

target_sources(App PRIVATE
    main.cpp
)

target_link_libraries(App PRIVATE
    ${GLFW_LIBRARIES}
    glad
)

set_target_properties(App PROPERTIES
    CXX_STANDARD 23
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
    COMPILE_WARNING_AS_ERROR OFF
)

if (MSVC)
    target_compile_options(App PRIVATE /W4)
else()
    target_compile_options(App PRIVATE -Wall -Wextra -pedantic)
endif()