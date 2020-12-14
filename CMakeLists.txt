cmake_minimum_required(VERSION 3.1)
project(test_package)

set( CMAKE_CXX_STANDARD 14 )

include("${CMAKE_BINARY_DIR}/conanbuildinfo.cmake")
conan_basic_setup(TARGETS)

find_package(Protobuf REQUIRED)

message( "CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}")
if( "${CMAKE_BUILD_TYPE}" MATCHES "Debug"  )
    message( "===> DEBUG" )
    set(CMAKE_CXX_FLAGS "/MTd")
else()
    message( "===> RELEASE" )
    set(CMAKE_CXX_FLAGS "/MT")
endif()

find_program(PROTOC_PROGRAM protoc)
if (NOT PROTOC_PROGRAM)
    message(WARNING "Protoc was not found")
endif()

add_executable(${PROJECT_NAME} test_package.cpp addressbook.proto)

target_include_directories(${PROJECT_NAME} PRIVATE "${CMAKE_BINARY_DIR}" "${CONAN_INCLUDE_DIRS}")
target_link_directories( ${PROJECT_NAME} PUBLIC ${CONAN_LIB_DIRS} )

target_link_libraries(${PROJECT_NAME} ${CONAN_LIBS_PROTOBUF})

protobuf_generate(TARGET ${PROJECT_NAME})
