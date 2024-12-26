cmake_minimum_required(VERSION 3.27)
project(particulator CXX C)
set(CMAKE_C_STANDARD 99)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address -fno-omit-frame-pointer -g")

# Adding Raylib
include(FetchContent)
set(FETCHCONTENT_QUIET FALSE)
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE) # don't build the supplied examples
set(BUILD_GAMES    OFF CACHE BOOL "" FORCE) # don't build the supplied example games

FetchContent_Declare(
    raylib
    GIT_REPOSITORY "https://github.com/raysan5/raylib.git"
    GIT_TAG "master"
    GIT_PROGRESS TRUE
    GIT_SHALLOW TRUE
)

FetchContent_MakeAvailable(raylib)

add_executable(particulator src/main.cpp src/multi-compilation-unit.c)

target_compile_options(particulator PUBLIC)
target_include_directories(particulator PUBLIC .)

target_link_libraries(particulator PUBLIC raylib)
set(CMAKE_CXX_FLAGS_DEBUG "-Wall -Werror -DCLAY_DEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

add_custom_command(
        TARGET particulator POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_CURRENT_SOURCE_DIR}/resources
        ${CMAKE_CURRENT_BINARY_DIR}/resources)