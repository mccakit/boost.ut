cmake_minimum_required(VERSION 4.2)
set(CMAKE_EXPERIMENTAL_CXX_IMPORT_STD "451f2fe2-a8a2-47c3-bc32-94786d8fc91b")
set(CMAKE_CXX_MODULE_STD ON)
project(ut VERSION 1.0.0 DESCRIPTION "placeholder" LANGUAGES C CXX)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
add_subdirectory(src)
option(UT_BUILD_EXAMPLES "Build examples" OFF)

if(UT_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
