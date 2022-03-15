cmake_minimum_required(VERSION 3.15)
project(basic CXX)

add_library(basic src/basic.cpp)

set_target_properties(basic PROPERTIES PUBLIC_HEADER "src/basic.h")

install(TARGETS basic DESTINATION "."
        PUBLIC_HEADER DESTINATION include
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        )
