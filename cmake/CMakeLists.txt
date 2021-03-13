cmake_minimum_required(VERSION 3.14.2)

project(cmake)

enable_language(ASM)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

# set cflags and cxxflags
set(GLOBAL_C_FLAGS " \
    -fPIC \
    -Wall \
    -Wno-unused \
    -Wno-unused-parameter \
    -fcolor-diagnostics \
    ")

# use libcxx need set -isystem
set(GLOBAL_CXX_FLAGS " \
    -nostdinc++ \
    -Wall \
    -fPIC \
    -fcolor-diagnostics \
    -isystem ${libcxx_headers} \
    -isystem ${libcxxabi_headers} \
    -Wno-unknown-attributes \
    -Wno-defaulted-function-deleted \
    -Wno-unused \
    -Wno-unused-parameter \
    -Wno-unused-command-line-argument \
    ")


# check c11
include(CheckCCompilerFlag)
CHECK_C_COMPILER_FLAG("-std=c11" COMPILER_SUPPORTS_C11)
if(COMPILER_SUPPORTS_C11)
    set(GLOBAL_C_FLAGS "${GLOBAL_C_FLAGS} -std=c11")
else()
    message(FATAL_ERROR "compiler not support c11")
endif()


# check c++17
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++17" COMPILER_SUPPORTS_CXX17)

if(COMPILER_SUPPORTS_CXX17)
    set(GLOBAL_CXX_FLAGS "${GLOBAL_CXX_FLAGS} -std=c++17")
else()
    message(FATAL_ERROR "compiler not support c++17")
endif()


# default use libcxx and libcxxabi
include_directories(
    ${libcxx_headers}
    ${libcxxabi_headers}	
    )

# all libraries
add_subdirectory(lib)

# build tools
add_subdirectory(build-tools)

# platform tools
add_subdirectory(platform-tools)
