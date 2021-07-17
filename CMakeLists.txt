# ------------------------------------------------------------------------------------------------
# Copyright (c) 2016 Microsoft Corporation
# Copyright (c) 2021 Wei Ji
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy of this software and
# associated documentation files (the "Software"), to deal in the Software without restriction,
# including without limitation the rights to use, copy, modify, merge, publish, distribute,
# sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
# 
# The above copyright notice and this permission notice shall be included in all copies or
# substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT
# NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
# NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
# DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
# ------------------------------------------------------------------------------------------------

cmake_minimum_required(VERSION 3.0)
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.312.tar.gz"
    SHA1 "42311324f6b859983d833cffc2151d0b86b8bea4"
    LOCAL
)

project(simple-malmo-agent)

hunter_add_package(Boost COMPONENTS chrono date_time filesystem iostreams program_options regex system thread)
hunter_add_package(ZLIB)

find_package( Boost 1.65.1 COMPONENTS chrono date_time filesystem iostreams program_options regex system thread REQUIRED )
find_package(ZLIB)
find_package(Threads REQUIRED)

find_library( Malmo_LIBRARIES Malmo ${CMAKE_CURRENT_SOURCE_DIR}/malmo/lib )

set( SOURCES  src/run_mission.cpp)

# ------------ Build the executable ---------------------------------

add_executable( run_mission ${SOURCES} )

set( MALMO_INCLUDE_FOLDERS 
    ${CMAKE_CURRENT_SOURCE_DIR}/malmo/include
    ${Boost_INCLUDE_DIR} 
    ${ZLIB_INCLUDE_DIRS}
)

include_directories( ${MALMO_INCLUDE_FOLDERS} )

target_link_libraries( run_mission 
  ${Malmo_LIBRARIES}
  ${Boost_LIBRARIES}
  ${ZLIB_LIBRARIES}
  Threads::Threads
)