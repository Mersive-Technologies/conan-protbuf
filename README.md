# conan-protobuf

To build:

1. git clone
1. mkdir build
1. cd build
1. conan install ..
1. cmake -DCMAKE_CXX_FLAGS=/MT ..
1. cmake --build . --target test_package
