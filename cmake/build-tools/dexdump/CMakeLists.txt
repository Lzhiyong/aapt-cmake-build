cmake_minimum_required(VERSION 3.14.2)

project(dexdump)

set(DEXDUMP ${AOSP}/art/dexdump)

set(DEXDUMP_SRC_FILES
    ${DEXDUMP}/dexdump_cfg.cc
    ${DEXDUMP}/dexdump_main.cc
    ${DEXDUMP}/dexdump.cc
    )

include_directories(
    ${libartbase_headers}
    ${libdexfile_headers}
    ${libartpalette_headers}
    ${liblog_headers}
    ${libbase_headers}
    ${libziparchive_headers}
    )

set(CMAKE_CXX_FLAGS "${GLOBAL_CXX_FLAGS}")

set(CMAKE_EXE_LINKER_FLAGS "-static")

add_executable(dexdump ${DEXDUMP_SRC_FILES})
target_link_libraries(dexdump 
    dexfile 
    artbase 
    artpalette 
    ziparchive 
    base 
    log 
    z 
    c++ 
    c++abi 
    m 
    dl
    )
