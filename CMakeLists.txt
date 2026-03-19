cmake_minimum_required(VERSION 3.16)
project(csr8675_i2s_host_stack VERSION 0.2.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(csr8675_ui STATIC src/display.cpp)
target_include_directories(csr8675_ui PUBLIC include)

add_executable(csr8675_demo src/main.cpp)
target_link_libraries(csr8675_demo PRIVATE csr8675_ui)
