# Copyright (c) 2012 Peter Kümmel
# All rights reserved. Use of this source code is governed by a
# BSD-style license that can be found in the LICENSE file.


if(NOT dartruntime_dir)
    get_filename_component(dartruntime_dir ${CMAKE_SOURCE_DIR}/dartruntime ABSOLUTE)
    if(NOT IS_DIRECTORY ${dartruntime_dir}/cmake)
        get_filename_component(dartruntime_dir ${CMAKE_SOURCE_DIR}/../dartruntime ABSOLUTE)
    endif()
    set(dartruntime_dir ${dartruntime_dir} PARENT_SCOPE)
endif()


if(IS_DIRECTORY ${dartruntime_dir}/cmake)
    message(STATUS "Using Dart form direcrory '${dartruntime_dir}'")
else()
    message(FATAL_ERROR "Directory '${dartruntime_dir}' doesn't contain the dartruntime sources")
endif()


#set(verbose 1)
include(${dartruntime_dir}/cmake/DartLibraries.cmake)


if(dart_executables)
    include(${dartruntime_dir}/cmake/DartExecutables.cmake)
endif()


set(dart_includes ${dartruntime_dir}/runtime/include CACHE STRING "Dart include directory" FORCE)
set(dart_libraries ${libdart_withcore} ${libopenssl} ${librt} CACHE STRING "Dart libraries" FORCE)

