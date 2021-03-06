# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)


#设置生成的so动态库最后输出的路径
set(jnilibs "${CMAKE_SOURCE_DIR}/src/main/jniLibs")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${jnilibs}/${ANDROID_ABI})

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

#设置编译指令
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fno-rtti -fno-exceptions -Wall")

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.



#如果当前系统版本过低设置拒绝编译
if (${ANDROID_PLATFORM_LEVEL} LESS 11)
  message(FATAL_ERROR "OpenGL 2 is not supported before API level 11 (currently using ${ANDROID_PLATFORM_LEVEL}).")
  return()
elseif (${ANDROID_PLATFORM_LEVEL} LESS 18)
  add_definitions("-DDYNAMIC_ES3")
  set(OPENGL_LIB GLESv2)
else ()
  set(OPENGL_LIB GLESv3)
endif (${ANDROID_PLATFORM_LEVEL} LESS 11)

#add_library( # Sets the name of the library.
#             vertex-lib
#
#             # Sets the library as a shared library.
#             SHARED
#
#             # Provides a relative path to your source file(s).
#             src/main/cpp/esUtil.cpp
#             src/main/cpp/vertex-lib.cpp)
#add_library( # Sets the name of the library.
#             vertexbuffer-lib
#
#             # Sets the library as a shared library.
#             SHARED
#
#             # Provides a relative path to your source file(s).
#             src/main/cpp/esUtil.cpp
#             src/main/cpp/vertexbuffer-lib.cpp)

add_library( # Sets the name of the library.
             vertexarray-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/esUtil.cpp
             src/main/cpp/vertexarray-lib.cpp)


# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.



find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       #vertex-lib
                       #vertexbuffer-lib
                       vertexarray-lib
                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib}
                       ${OPENGL_LIB}
                       android
                       EGL
                       m)