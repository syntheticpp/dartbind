# Copyright (c) 2012 Peter Kümmel
# All rights reserved. Use of this source code is governed by a
# BSD-style license that can be found in the LICENSE file.


if(NOT dartruntime_dir)
    get_filename_component(dartruntime_dir ${CMAKE_SOURCE_DIR}/dartruntime/source ABSOLUTE)
    if(NOT IS_DIRECTORY ${dartruntime_dir}/cmake)
        get_filename_component(dartruntime_dir ${CMAKE_SOURCE_DIR}/../dartruntime ABSOLUTE)
    endif()
    set(dartruntime_dir ${dartruntime_dir} PARENT_SCOPE)
endif()

message(STATUS)
if(IS_DIRECTORY ${dartruntime_dir}/cmake)
    message(STATUS "Using Dart form directory '${dartruntime_dir}'")
else()
    message(STATUS "Directory '${dartruntime_dir}' doesn't contain the dartruntime sources")
    message(STATUS "Please checkout the submodule:\n")
    message("       git submodule init")
    message("       git submodule update\n")
    message(FATAL_ERROR "Aborting now")
endif()


#set(verbose 1)
include(${dartruntime_dir}/cmake/DartLibraries.cmake)


if(dart_executables)
    include(${dartruntime_dir}/cmake/DartExecutables.cmake)
endif()


set(dart_includes ${dartruntime_dir}/runtime/include CACHE STRING "Dart include directory" FORCE)
set(dart_libraries ${libdart_withcore} ${libopenssl} ${librt} CACHE STRING "Dart libraries" FORCE)

