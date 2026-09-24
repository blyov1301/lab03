cmake_minimum_required(VERSION 3.16)
project(hello_world LANGUAGES CXX)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex)

add_executable(hello_world hello_world.cpp)
target_link_libraries(hello_world PRIVATE formatter_ex)
target_compile_features(hello_world PRIVATE cxx_std_11)
