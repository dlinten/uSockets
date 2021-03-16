# Works with 3.11 and tested through 3.18
cmake_minimum_required(VERSION 3.14...3.18)

# Project name and a few useful settings. Other commands can pick up the results
project(uSockets
        VERSION 0.1
        DESCRIPTION "A C/C++ uSockets CMake Implementation"
        LANGUAGES CXX C)

add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/uSockets.a
        COMMAND env "MAKEFLAGS=$(subst s,,$(MAKEFLAGS)) WITH_OPENSSL=1" make -f ./Makefile
        COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/uSockets.a  ${CMAKE_CURRENT_BINARY_DIR}/uSockets.a
        COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/include
        COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/src/libusockets.h ${CMAKE_CURRENT_BINARY_DIR}/include/libusockets.h
        COMMAND ${CMAKE_COMMAND} -E remove -f ${CMAKE_CURRENT_SOURCE_DIR}/*.a ${CMAKE_CURRENT_SOURCE_DIR}/*.o
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

add_custom_target(uSockets ALL
        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/uSockets.a)

set_target_properties(uSockets
        PROPERTIES
        IMPORTED_LOCATION ${CMAKE_CURRENT_BINARY_DIR}/uSockets.a
        INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_BINARY_DIR}/include)
