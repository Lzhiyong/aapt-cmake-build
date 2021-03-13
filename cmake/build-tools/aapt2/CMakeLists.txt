cmake_minimum_required(VERSION 3.14.2)

project(aapt2)

set(AAPT2 ${AOSP}/frameworks/base/tools/aapt2)

set(TOOL_SRC_FILES
    ${AAPT2}/cmd/Command.cpp
    ${AAPT2}/cmd/Compile.cpp
    ${AAPT2}/cmd/Convert.cpp
    ${AAPT2}/cmd/Diff.cpp
    ${AAPT2}/cmd/Dump.cpp
    ${AAPT2}/cmd/Link.cpp
    ${AAPT2}/cmd/Optimize.cpp
    ${AAPT2}/cmd/Util.cpp
    )


# libaapt2 static lib
set(LIBAAPT2_STATIC_SRC_FILES
    ${AAPT2}/compile/IdAssigner.cpp
    ${AAPT2}/compile/InlineXmlFormatParser.cpp
    ${AAPT2}/compile/NinePatch.cpp
    ${AAPT2}/compile/Png.cpp
    ${AAPT2}/compile/PngChunkFilter.cpp
    ${AAPT2}/compile/PngCrunch.cpp
    ${AAPT2}/compile/PseudolocaleGenerator.cpp
    ${AAPT2}/compile/Pseudolocalizer.cpp
    ${AAPT2}/compile/XmlIdCollector.cpp
    ${AAPT2}/configuration/ConfigurationParser.cpp
    ${AAPT2}/dump/DumpManifest.cpp
    ${AAPT2}/filter/AbiFilter.cpp
    ${AAPT2}/filter/ConfigFilter.cpp
    ${AAPT2}/format/Archive.cpp
    ${AAPT2}/format/Container.cpp
    ${AAPT2}/format/binary/BinaryResourceParser.cpp
    ${AAPT2}/format/binary/ResChunkPullParser.cpp
    ${AAPT2}/format/binary/TableFlattener.cpp
    ${AAPT2}/format/binary/XmlFlattener.cpp
    ${AAPT2}/format/proto/ProtoDeserialize.cpp
    ${AAPT2}/format/proto/ProtoSerialize.cpp
    ${AAPT2}/io/BigBufferStream.cpp
    ${AAPT2}/io/File.cpp
    ${AAPT2}/io/FileStream.cpp
    ${AAPT2}/io/FileSystem.cpp
    ${AAPT2}/io/StringStream.cpp
    ${AAPT2}/io/Util.cpp
    ${AAPT2}/io/ZipArchive.cpp
    ${AAPT2}/link/AutoVersioner.cpp
    ${AAPT2}/link/ManifestFixer.cpp
    ${AAPT2}/link/NoDefaultResourceRemover.cpp
    ${AAPT2}/link/ProductFilter.cpp
    ${AAPT2}/link/PrivateAttributeMover.cpp
    ${AAPT2}/link/ReferenceLinker.cpp
    ${AAPT2}/link/ResourceExcluder.cpp
    ${AAPT2}/link/TableMerger.cpp
    ${AAPT2}/link/XmlCompatVersioner.cpp
    ${AAPT2}/link/XmlNamespaceRemover.cpp
    ${AAPT2}/link/XmlReferenceLinker.cpp
    ${AAPT2}/optimize/MultiApkGenerator.cpp
    ${AAPT2}/optimize/ResourceDeduper.cpp
    ${AAPT2}/optimize/ResourceFilter.cpp
    ${AAPT2}/optimize/ResourcePathShortener.cpp
    ${AAPT2}/optimize/VersionCollapser.cpp
    ${AAPT2}/process/SymbolTable.cpp
    ${AAPT2}/split/TableSplitter.cpp
    ${AAPT2}/text/Printer.cpp
    ${AAPT2}/text/Unicode.cpp
    ${AAPT2}/text/Utf8Iterator.cpp
    ${AAPT2}/util/BigBuffer.cpp
    ${AAPT2}/util/Files.cpp
    ${AAPT2}/util/Util.cpp
    ${AAPT2}/Debug.cpp
    ${AAPT2}/DominatorTree.cpp
    ${AAPT2}/java/AnnotationProcessor.cpp
    ${AAPT2}/java/ClassDefinition.cpp
    ${AAPT2}/java/JavaClassGenerator.cpp
    ${AAPT2}/java/ManifestClassGenerator.cpp
    ${AAPT2}/java/ProguardRules.cpp
    ${AAPT2}/LoadedApk.cpp
    ${AAPT2}/Resource.cpp
    ${AAPT2}/ResourceParser.cpp
    ${AAPT2}/ResourceTable.cpp
    ${AAPT2}/ResourceUtils.cpp
    ${AAPT2}/ResourceValues.cpp
    ${AAPT2}/SdkConstants.cpp
    ${AAPT2}/StringPool.cpp
    ${AAPT2}/trace/TraceBuffer.cpp
    ${AAPT2}/xml/XmlActionExecutor.cpp
    ${AAPT2}/xml/XmlDom.cpp
    ${AAPT2}/xml/XmlPullParser.cpp
    ${AAPT2}/xml/XmlUtil.cpp
    ${AAPT2}/Configuration.proto
    ${AAPT2}/Resources.proto
    ${AAPT2}/ResourcesInternal.proto
    )


# libaapt2_jni shared lib
set(LIBAAPT2_JNI_SRC_FILES
    ${AAPT2}/jni/aapt2_jni.cpp
    ${TOOL_SRC_FILES}
    )

# aapt2
set(AAPT2_SRC_FILES 
    ${AAPT2}/Main.cpp
    ${TOOL_SRC_FILES}
    )


include_directories(
    ${aapt2_headers}
    ${protobuf_headers}
    ${androidfw_headers}
    ${libbase_headers}
    ${libziparchive_headers}
    ${liblog_headers}
    ${libcutils_headers}
    ${libutils_headers}
    ${libsystem_headers}
    ${zlib_headers}
    ${native_headers}
    ${libpng_headers}
    ${expat_headers}
    ${libbuildversion_headers}
    ${incfs_headers}
    ${fmtlib_headers}
    ${gtest_headers}
    ${libidmap2_policies_headers}
    )

message(STATUS "CMAKE_BINARY_DIR: ${CMAKE_BINARY_DIR}")

# generate files in android-11/frameworks/base/tools/aapt2/frameworks/base/tools/aapt2
# check Configuration.pb.cc
if (EXISTS ${AAPT2}/frameworks/base/tools/aapt2/Configuration.pb.cc)
    list(APPEND LIBAAPT2_STATIC_SRC_FILES ${AAPT2}/frameworks/base/tools/aapt2/Configuration.pb.cc)
else ()                                             
    # generate Configuration.pb.h and Configuration.pb.cc
    execute_process(
        COMMAND ${CMAKE_BINARY_DIR}/cmake/lib/protobuf/aprotoc 
        frameworks/base/tools/aapt2/Configuration.proto 
        --cpp_out=frameworks/base/tools/aapt2
        WORKING_DIRECTORY ${AOSP}
        )
endif ()

# check Resources.pb.cc
if (EXISTS ${AAPT2}/frameworks/base/tools/aapt2/Resources.pb.cc)
    list(APPEND LIBAAPT2_STATIC_SRC_FILES ${AAPT2}/frameworks/base/tools/aapt2/Resources.pb.cc)
else ()
    # generate Resources.pb.h and Resources.pb.cc
    execute_process(
        COMMAND ${CMAKE_BINARY_DIR}/cmake/lib/protobuf/aprotoc 
        frameworks/base/tools/aapt2/Resources.proto 
        --cpp_out=frameworks/base/tools/aapt2
        WORKING_DIRECTORY ${AOSP}
        )
endif ()

# check ResourcesInternal.pb.cc
if (EXISTS ${AAPT2}/frameworks/base/tools/aapt2/ResourcesInternal.pb.cc)
    list(APPEND LIBAAPT2_STATIC_SRC_FILES ${AAPT2}/frameworks/base/tools/aapt2/ResourcesInternal.pb.cc)
else ()
    # generate ResourcesInternal.pb.h and ResourcesInternal.pb.cc
    execute_process(
        COMMAND ${CMAKE_BINARY_DIR}/cmake/lib/protobuf/aprotoc 
        frameworks/base/tools/aapt2/ResourcesInternal.proto 
        --cpp_out=frameworks/base/tools/aapt2
        WORKING_DIRECTORY ${AOSP}
    )
endif ()


set(CMAKE_C_FLAGS " \
    ${GLOBAL_C_FLAGS} \
    -Wall \
    -Werror \
    -Wextra \
    -Wno-unused-parameter \
    ")
    
set(CMAKE_CXX_FLAGS " \
    ${GLOBAL_CXX_FLAGS} \
    -Wno-missing-field-initializers \
    -Wno-defaulted-function-deleted \
    -fno-exceptions \
    -fno-rtti \
    ")

# static linking
set(CMAKE_EXE_LINKER_FLAGS "-static")

add_library(aapt2_static STATIC ${LIBAAPT2_STATIC_SRC_FILES} ${PROTO_SRCS})
target_link_libraries(aapt2_static)

add_library(aapt2_jni STATIC ${LIBAAPT2_JNI_SRC_FILES})
target_link_libraries(aapt2_jni)

add_executable(aapt2 ${AAPT2_SRC_FILES})
target_link_libraries(aapt2 
    aapt2_static 
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
    cutils 
    expat 
    ziparchive 
    png 
    base 
    protobuf-cpp-full
    buildversion 
    log
    z 
    c++ 
    c++abi 
    m 
    dl
    )
