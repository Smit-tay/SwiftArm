cmake_minimum_required (VERSION 3.22.0)
set (CMAKE_VERBOSE_MAKEFILE FALSE)

project (SwiftArm
         VERSION 0.1
         DESCRIPTION "uArm SwiftPro project"
         HOMEPAGE_URL http:/blah/blah.html
         LANGUAGES C CXX)


if (NOT EXISTS ${CMAKE_BINARY_DIR}/CMakeCache.txt)
  if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "" FORCE)
  endif()
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# This needs to be looked at - is linux=1 necessary ?
add_compile_options("-D__linux__=1"
                    "-D_REENTRANT"
                    "-Wall" "-Wextra"
                    "-fno-strict-aliasing")

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/lib)

add_executable(test_uarm example/uarm_example.cpp)
set_target_properties(test_uarm PROPERTIES COMPILE_FLAGS "-fPIC -std=c++11")

target_include_directories(test_uarm  PUBLIC
  "${CMAKE_CURRENT_SOURCE_DIR}/lib/include" )
target_link_libraries(test_uarm uarm)
add_dependencies(test_uarm uarm)

include(FindThreads)
find_package( Threads REQUIRED )
link_libraries(Threads::Threads)
add_compile_options(${CMAKE_THREAD_LIBS_INIT})

enable_testing()
add_test(
    NAME test_uarm
    COMMAND test_uarm /dev/swiftpro all
)
#get_cmake_property(_variableNames VARIABLES)
#list (SORT _variableNames)
#foreach (_variableName ${_variableNames})
#    message(STATUS "${_variableName}=${${_variableName}}")
#endforeach()
