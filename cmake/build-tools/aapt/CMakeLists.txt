cmake_minimum_required(VERSION 3.14.2)

project(aapt)

set(AAPT ${AOSP}/frameworks/base/tools/aapt)

# libaapt_static 
set(LIBAAPT_STATIC_SRC_FILES
    ${AAPT}/AaptAssets.cpp
    ${AAPT}/AaptConfig.cpp
    ${AAPT}/AaptUtil.cpp
    ${AAPT}/AaptXml.cpp
    ${AAPT}/ApkBuilder.cpp
    ${AAPT}/Command.cpp
    ${AAPT}/CrunchCache.cpp
    ${AAPT}/FileFinder.cpp
    ${AAPT}/Images.cpp
    ${AAPT}/Package.cpp
    ${AAPT}/pseudolocalize.cpp
    ${AAPT}/Resource.cpp
    ${AAPT}/ResourceFilter.cpp
    ${AAPT}/ResourceIdCache.cpp
    ${AAPT}/ResourceTable.cpp
    ${AAPT}/SourcePos.cpp
    ${AAPT}/StringPool.cpp
    ${AAPT}/WorkQueue.cpp
    ${AAPT}/XMLNode.cpp
    ${AAPT}/ZipEntry.cpp
    ${AAPT}/ZipFile.cpp
    )

# aapt
set(AAPT_SRC_FILES ${AAPT}/Main.cpp)

include_directories(
    ${aapt_headers}
    ${androidfw_headers}
    ${libpng_headers}
    ${libutils_headers}
    ${liblog_headers}
    ${libcutils_headers}
    ${expat_headers}
    ${libziparchive_headers}
    ${libbase_headers}
    ${zlib_headers}
    ${libsystem_headers}
    ${libbuildversion_headers}
    ${native_headers}
    ${incfs_headers}
    ${fmtlib_headers}
    )

set(CMAKE_CXX_FLAGS " \
    ${GLOBAL_CXX_FLAGS} \
    -Wno-format-y2k \
    -DSTATIC_ANDROIDFW_FOR_TOOLS \
    -Wno-error=implicit-fallthrough \
    ")

# libcrypto and libssl
include_directories(
    ${CMAKE_SOURCE_DIR}/cmake/lib/boringssl/build/crypto
    ${CMAKE_SOURCE_DIR}/cmake/lib/boringssl/build/ssl
    )

# static linking
set(CMAKE_EXE_LINKER_FLAGS "-static")

add_library(aapt_static STATIC ${LIBAAPT_STATIC_SRC_FILES})
target_link_libraries(aapt_static)

add_executable(aapt ${AAPT_SRC_FILES})
target_link_libraries(aapt 
    aapt_static 
    png 
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
    base 
    buildversion 
    z 
    c++ 
    c++abi 
    m 
    dl
    )
