# Universal Source Code
# Wasim Abbas
# http://www.waZim.com
# Copyright (c) 2020
#
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the 'Software'),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the Software
# is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
# OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
# CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
# TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE
# OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
#
# Version: 1.0.0

cmake_minimum_required (VERSION 3.12)

project(Universal VERSION 1.1.0.0 LANGUAGES CXX C)
list(APPEND CMAKE_MESSAGE_CONTEXT Watch_Cat)

set(UNIVERSAL_NAME universal)

set(UNIVERSAL_ROOT_DIR ${CMAKE_CURRENT_LIST_DIR}) # Always prefer the _LIST_ version FILES/DIRS instead of the _SOURCE_
set(UNIVERSAL_SOURCE_DIR ${UNIVERSAL_ROOT_DIR})

include(${UNIVERSAL_ROOT_DIR}/build.cmake)

# Always generate compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

# Configure version source file universal_version.cpp
configure_file(${UNIVERSAL_SOURCE_DIR}/universal_version.cpp.in universal_version.cpp @ONLY)
add_library(universal_version STATIC ${CMAKE_CURRENT_BINARY_DIR}/universal_version.cpp)
target_include_directories(universal_version PUBLIC ${CMAKE_CURRENT_LIST_DIR})

set(UNIVERSAL_HEADERS
  ${UNIVERSAL_SOURCE_DIR}/universal.hpp)

set(UNIVERSAL_SOURCES
  ${UNIVERSAL_SOURCE_DIR}/universal.cpp)

add_library(${UNIVERSAL_NAME} SHARED ${UNIVERSAL_SOURCES} ${UNIVERSAL_HEADERS})
# add_executable(${UNIVERSAL_NAME} ${UNIVERSAL_SOURCES} ${UNIVERSAL_HEADERS})

set_target_properties(${UNIVERSAL_NAME} PROPERTIES VERSION 1.0.0.0 SOVERSION 1)          # TODO: Fix the hardcoded version
set_target_properties(${UNIVERSAL_NAME} PROPERTIES CMAKE_VISIBILITY_INLINES_HIDDEN true) # Don't export all of those C++ template generated symbols
set_target_properties(${UNIVERSAL_NAME} PROPERTIES CMAKE_CXX_VISIBILITY_PRESET hidden)   # Set default to hide symbols visibility for Universal
set_target_properties(${UNIVERSAL_NAME} PROPERTIES CMAKE_C_VISIBILITY_PRESET hidden)     # Set default to hide symbols visibility for Universal

set(CMAKE_VISIBILITY_INLINES_HIDDEN YES)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)    # Set default to hide symbols visibility for all targets
set(CMAKE_C_VISIBILITY_PRESET hidden)      # Set default to hide symbols visibility for all targets

# Only use the below if building library
message(STATUS "Now generating universal_export_import.h file")
include(GenerateExportHeader)
generate_export_header(${UNIVERSAL_NAME}
  EXPORT_FILE_NAME universal_export_import.hpp
  EXPORT_MACRO_NAME UNIVERSAL_ITEM
  NO_EXPORT_MACRO_NAME UNIVERSAL_PRIVATE)

target_include_directories(${UNIVERSAL_NAME} PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(${UNIVERSAL_NAME} PRIVATE ${UNIVERSAL_SOURCE_DIR})
target_include_directories(${UNIVERSAL_NAME} PRIVATE ${UNIVERSAL_ROOT_DIR})

set(UNIVERSAL_REQUIRED_LIBRARIES
  universal_version)

target_link_libraries(${UNIVERSAL_NAME} PRIVATE ${UNIVERSAL_REQUIRED_LIBRARIES})

build_options(${UNIVERSAL_NAME}) # Set common build options
