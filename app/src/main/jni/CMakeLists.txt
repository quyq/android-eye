# Sets the minimum version of CMake required to build your native library.
# This ensures that a certain set of CMake features is available to
# your build.

cmake_minimum_required(VERSION 3.4.1)

# Specifies a library name, specifies whether the library is STATIC or
# SHARED, and provides relative paths to the source code. You can
# define multiple libraries by adding multiple add_library() commands,
# and CMake builds them for you. When you build your app, Gradle
# automatically packages shared libraries with your APK.

#add_library( # Specifies the name of the library.
#             x264_pre
#             # Sets the library as a shared library.
#             STATIC
#             # Provides a relative path to your source file(s).
#             x264/libx264.a )

add_library(x264pre STATIC IMPORTED)
set_property(TARGET x264pre PROPERTY IMPORTED_LOCATION "${CMAKE_CURRENT_LIST_DIR}/x264/libx264.a")
#set_target_properties(x264pre PROPERTIES IMPORTED_LOCATION "${CMAKE_CURRENT_LIST_DIR}/x264/libx264.a")

include_directories( . )
add_library( # Specifies the name of the library.
             MediaEncoder
             # Sets the library as a shared library.
             SHARED
             # Provides a relative path to your source file(s).
             main_jni.cpp h264encoder.cpp g72x/g726_32.c g72x/g711.c g72x/g72x.c )

find_library( # Defines the name of the path variable that stores the
              # location of the NDK library.
              log-lib

              # Specifies the name of the NDK library that
              # CMake needs to locate.
              log )
#LOCAL_LDLIBS += -llog -lz -ldl

# Links your native library against one or more other native libraries.
target_link_libraries( # Specifies the target library.
                       MediaEncoder
                       x264pre
                       # Links the log library to the target library.
                       ${log-lib} )


#LOCAL_CPPFLAGS := -O2 -Werror -Wall