cmake_minimum_required(VERSION 3.18.1)

project("studyffmpeg")

# 定义 so 库和头文件所在目录，方便后面使用
set(ffmpeg_head_dir ${CMAKE_SOURCE_DIR}/ffmpeg/arm64-v8a)
set(ffmpeg_lib_dir ${CMAKE_SOURCE_DIR}/ffmpeg/arm64-v8a/lib)

include_directories(${ffmpeg_head_dir}/include)

add_library( # Sets the name of the library.
        studyffmpeg

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        native-lib.cpp)


add_library(avcodec SHARED IMPORTED)
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/libavcodec.so)

add_library(avfilter SHARED IMPORTED)
set_target_properties(avfilter PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/libavfilter.so)

add_library(avformat SHARED IMPORTED)
set_target_properties(avformat PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/libavformat.so)

add_library(avutil SHARED IMPORTED)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/libavutil.so)

add_library(swresample SHARED IMPORTED)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/libswresample.so)

add_library(swscale SHARED IMPORTED)
set_target_properties(swscale PROPERTIES IMPORTED_LOCATION ${ffmpeg_lib_dir}/libswscale.so)

find_library( # Sets the name of the path variable.
        log-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        log)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
        studyffmpeg

        avcodec
        avfilter
        avformat
        avutil
        swresample
        swscale
        -landroid
        # Links the target library to the log library
        # included in the NDK.
        ${log-lib})