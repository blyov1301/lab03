cmake_minimum_required(VERSION 3.16)
project(formatter_ex LANGUAGES CXX)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter)

add_library(formatter_ex STATIC formatter_ex.cpp)
target_include_directories(formatter_ex PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(formatter_ex PUBLIC formatter)
target_compile_features(formatter_ex PUBLIC cxx_std_11)
