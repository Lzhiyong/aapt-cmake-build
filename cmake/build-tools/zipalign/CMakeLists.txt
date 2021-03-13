cmake_minimum_required(VERSION 3.14.2)

project(zipalign)

set(ZIPALIGN ${AOSP}/build/tools/zipalign)

set(LIBZIPALIGN_SRC_FILES
    ${ZIPALIGN}/ZipAlign.cpp
    ${ZIPALIGN}/ZipEntry.cpp
    ${ZIPALIGN}/ZipFile.cpp
	)

set(ZIPALIGN_SRC_FILES
    ${ZIPALIGN}/ZipAlignMain.cpp
	)
	
include_directories(
    ${zlib_headers}
    ${libzopfli_headers}
    ${libutils_headers}
    ${libcutils_headers}
    ${androidfw_headers}
    ${liblog_headers}
    ${libbase_headers}
    ${libziparchive_headers}
    ${ZIPALIGN}/include
    )

set(CMAKE_CXX_FLAGS "${GLOBAL_CXX_FLAGS}")

# static linking
set(CMAKE_EXE_LINKER_FLAGS "-static")

add_library(zipalign_static STATIC ${LIBZIPALIGN_SRC_FILES})
target_link_libraries(zipalign_static)

add_executable(zipalign ${ZIPALIGN_SRC_FILES})
target_link_libraries(zipalign 
    zipalign_static
    utils 
    cutils 
    ziparchive 
    base 
    zopfli 
    cpu_features 
    log
    z
    c++ 
    c++abi 
    m 
    dl
    )
