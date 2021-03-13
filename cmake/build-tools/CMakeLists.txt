cmake_minimum_required(VERSION 3.14.2)

project(build-tools)

add_subdirectory(aapt)
add_subdirectory(aapt2)
add_subdirectory(aidl)
add_subdirectory(zipalign)
add_subdirectory(dexdump)
add_subdirectory(split-select)

