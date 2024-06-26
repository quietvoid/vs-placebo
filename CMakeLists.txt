cmake_minimum_required(VERSION 3.29.2)
project(vs_placebo C)
project(p2p CXX)

set(CMAKE_C_STANDARD 17)
include_directories(".")

add_library(p2p STATIC libp2p/p2p_api.cpp libp2p/v210.cpp)
add_library(vs_placebo SHARED vs-placebo.c vs-placebo.h shader.c shader.h deband.c deband.h tonemap.c tonemap.h resample.c resample.h)
target_compile_options(vs_placebo PRIVATE -Wno-discarded-qualifiers)
target_compile_options(p2p PRIVATE -fPIC)
target_link_libraries(vs_placebo p2p)
option(WSTATIC "Use hardcoded paths to compile statically without any deps" OFF)
if (WSTATIC)
    include_directories("E:/Programs/msys64/mingw64/include")
    target_link_libraries(vs_placebo "E:/building/libplacebo/build/src/libplacebo.a")
    target_link_libraries(vs_placebo "E:/building/shaderc/build/libshaderc/libshaderc_combined.a")
    target_link_libraries(vs_placebo "E:/Programs/msys64/mingw64/lib/liblcms2.a")
    target_link_libraries(vs_placebo "E:/Programs/msys64/mingw64/lib/libvulkan.dll.a")
else()
    target_link_libraries(vs_placebo -lplacebo)
endif ()
