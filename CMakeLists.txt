cmake_minimum_required(VERSION 3.30)
project(sofpycdb)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(DEV_MODULE Development.Module)
find_package(Python 3.8 COMPONENTS Interpreter ${DEV_MODULE} REQUIRED)

# Source files
file(GLOB_RECURSE bindings_source "src/*.h" "src/*.cpp")

# Main Python extension module
add_subdirectory(ext/nanobind)
nanobind_build_library(nanobind)
nanobind_add_module(_sofpycdb NB_STATIC ${bindings_source})

# Header files from cdbpp
#include_directories(ext/cdbpp/include)
target_include_directories(_sofpycdb PRIVATE ext/cdbpp/include)

install(TARGETS _sofpycdb
    LIBRARY DESTINATION sofpycdb
    RUNTIME DESTINATION sofpycdb
    ARCHIVE DESTINATION sofpycdb
)
