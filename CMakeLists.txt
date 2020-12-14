cmake_minimum_required(VERSION 3.1)
project(test_package)

set( CMAKE_CXX_STANDARD 14 )

message( "CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}")
if( "${CMAKE_BUILD_TYPE}" MATCHES "Debug"  )
    message( "===> DEBUG" )
    set(CMAKE_CXX_FLAGS "/MTd")
else()
    message( "===> RELEASE" )
    set(CMAKE_CXX_FLAGS "/MT")
endif()

execute_process(
    COMMAND "powershell" "cd ${CMAKE_BINARY_DIR}; conan install -s build_type=${CMAKE_BUILD_TYPE} .."
)

include("${CMAKE_BINARY_DIR}/conanbuildinfo.cmake")
conan_basic_setup()

find_package(Protobuf REQUIRED)

find_program(PROTOC_PROGRAM protoc)
if (NOT PROTOC_PROGRAM)
    message(WARNING "Protoc was not found")
endif()

add_executable(${PROJECT_NAME} test_package.cpp addressbook.proto)
protobuf_generate(TARGET ${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} PRIVATE "${CMAKE_BINARY_DIR}")

target_link_libraries(${PROJECT_NAME} ${CONAN_LIBS})


