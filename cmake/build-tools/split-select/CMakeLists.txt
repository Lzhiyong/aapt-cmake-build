cmake_minimum_required(VERSION 3.14.2)

project(split-select)

set(SPLIT-SELECT ${AOSP}/frameworks/base/tools/split-select)

set(LIBSPLIT-SELECT_SRC_FILES
    ${SPLIT-SELECT}/Abi.cpp
    ${SPLIT-SELECT}/Grouper.cpp
    ${SPLIT-SELECT}/Rule.cpp
    ${SPLIT-SELECT}/RuleGenerator.cpp
    ${SPLIT-SELECT}/SplitDescription.cpp
    ${SPLIT-SELECT}/SplitSelector.cpp
    )

include_directories(
    ${aapt_headers}
    ${libutils_headers}
    ${libcutils_headers}
    ${libbase_headers}
    ${liblog_headers}
    ${androidfw_headers}
    ${libsystem_headers}
    ${incfs_headers}
    ${fmtlib_headers}
    ${SPLIT-SELECT}/../
    )


set(CMAKE_CXX_FLAGS "${GLOBAL_CXX_FLAGS} -D_DARWIN_UNLIMITED_STREAMS")
set(CMAKE_EXE_LINKER_FLAGS "-static")

add_library(split-select_static STATIC ${LIBSPLIT-SELECT_SRC_FILES})
target_link_libraries(split-select_static)

add_executable(split-select ${SPLIT-SELECT}/Main.cpp)
target_link_libraries(split-select 
    split-select_static
    aapt_static
    androidfw 
    incfs_utils
    incfs
    fmt
    selinux
    pcre2
    sepol
    packagelistparser
    crypto
    ssl
    utils 
    processgroup 
    cgrouprc 
    cgrouprc_format 
    jsoncpp 
    log 
    cutils 
    expat 
    ziparchive 
    png 
    base 
    z 
    c++ 
    c++abi 
    m 
    dl
    )

