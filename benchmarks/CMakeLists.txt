cmake_minimum_required(VERSION 2.8)
project(bench_mixers)

# C90 requires the gcc extensions for function attributes like always_inline
# C99 provides the function attributes: no gcc extensions required
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_EXTENSIONS OFF)

set(CMAKE_CXX_STANDARD 98)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

########################################################################
# select the release build type by default to get optimization flags
########################################################################
if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE "Release")
   message(STATUS "Build type not specified: defaulting to release.")
endif(NOT CMAKE_BUILD_TYPE)

if ( CMAKE_C_COMPILER_ID MATCHES "MSVC" )
  # using Visual Studio C++
  message(STATUS "INFO: detected MSVC: will not link math lib m")
  set(MATHLIB "")

  add_definitions("/D_CRT_SECURE_NO_WARNINGS")
  set(MSVC_DISABLED_WARNINGS_LIST "C4996")

else()
  message(STATUS "INFO: detected NO MSVC: ${CMAKE_C_COMPILER_ID}: will link math lib m")
  set(MATHLIB "m")
endif()

######################################################

add_executable(bench_mixers   bench_mixers.c)
target_compile_definitions(bench_mixers PRIVATE _USE_MATH_DEFINES)
target_link_libraries( bench_mixers csdr ${MATHLIB} )

