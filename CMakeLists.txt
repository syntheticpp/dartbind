# Copyright (c) 2012 Peter Kümmel
# All rights reserved. Use of this source code is governed by a
# BSD-style license that can be found in the LICENSE file.


cmake_minimum_required(VERSION 2.8)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)


option(release "Release build" OFF)


project(dartbind)


set(EXECUTABLE_OUTPUT_PATH  ${CMAKE_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH     ${CMAKE_BINARY_DIR}/lib)


if(release)
    set(CMAKE_BUILD_TYPE Release)
else()
    set(CMAKE_BUILD_TYPE Debug)
endif()


add_subdirectory(dartruntime)
message(STATUS "dart_includes: ${dart_includes}")


enable_testing()
macro(p_addExecutables _dir _prefix)
    foreach(_it ${ARGN})
        set(name ${_prefix}${_it})
        include_directories(${dart_includes})
        add_executable(${name} ${_dir}/${name}.cc)
        target_link_libraries(${name} ${dart_libraries})
        add_test(NAME ${name} COMMAND ${name})
    endforeach()
endmacro()


p_addExecutables(examples bind_ NativeFunction_foo NativeFunction_userdata NativeFunction_nativefield NativeFunction_class)
#p_addExecutables(examples "" minimal_http_server)


