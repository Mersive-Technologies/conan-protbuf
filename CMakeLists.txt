cmake_minimum_required(VERSION 3.1)
project(test_package)

#set(protobuf_DIR "c:/users/bgardner/workspace/conan-center-index/recipes/protobuf/all/build_subfolder/source_subfolder/cmake/lib/cmake/protobuf/")

include("${CMAKE_BINARY_DIR}/conanbuildinfo.cmake")
conan_basic_setup(TARGETS)

find_package(Protobuf REQUIRED)

find_program(PROTOC_PROGRAM protoc)
if (NOT PROTOC_PROGRAM)
    message(WARNING "Protoc was not found")
endif()

#get_cmake_property(_variableNames VARIABLES)
#list (SORT _variableNames)
#foreach (_variableName ${_variableNames})
#    if (ARGV0)
#        unset(MATCHED)
#        string(REGEX MATCH ${ARGV0} MATCHED ${_variableName})
#        if (NOT MATCHED)
#            continue()
#        endif()
#    endif()
#    message(STATUS "${_variableName}=${${_variableName}}")
#endforeach()

add_executable(${PROJECT_NAME} test_package.cpp addressbook.proto)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)
target_link_directories(${PROJECT_NAME} PUBLIC "${CMAKE_BINARY_DIR}/lib")
target_include_directories(${PROJECT_NAME} PRIVATE "${CMAKE_BINARY_DIR}" "${CMAKE_BINARY_DIR}/include")
target_link_libraries(${PROJECT_NAME} libprotobuf)

target_link_libraries(${PROJECT_NAME} libprotoc)

protobuf_generate(PROTO_SRCS PROTO_HDRS TARGET ${PROJECT_NAME})
protobuf_generate(LANGUAGE cpp TARGET ${PROJECT_NAME} PROTOS addressbook.proto)
