cmake_minimum_required(VERSION 3.0)
set(CMAKE_TOOLCHAIN_FILE "C:/vcpkg/scripts/buildsystems/vcpkg.cmake" )
project(basic)

add_compile_definitions(COREBOX_EXPORT)
add_compile_definitions(_USE_MATH_DEFINES)

find_package(Qt5 CONFIG REQUIRED Core)

set(basic_VERSION 1.0.0)

project(basic
	VERSION ${basic_VERSION}
	DESCRIPTION "A simple json parsing and stringifying library"
	HOMEPAGE_URL "https://github.com/ThomasDavenel/basic"
	LANGUAGES CXX)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)



file(GLOB SRCS CONFIGURE_DEPENDS
            "src/basic.cpp"
            "include/basic.h"
    )

add_library(basic STATIC ${SRCS})
add_library(basic::basic ALIAS basic)

if(BUILD_SHARED_LIBS AND WIN32)
	target_compile_definitions(basic
		PRIVATE "COREBOX_EXPORT=__declspec(dllexport)"
		INTERFACE "COREBOX_EXPORT=__declspec(dllimport)")
endif()

target_compile_features(basic PUBLIC cxx_std_17)
target_include_directories(basic
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
		
target_precompile_headers(basic PUBLIC ${CMAKE_SOURCE_DIR}/include/basic/basic.h) # PUBLIC will make it used by targets using this target

target_link_libraries(basic PUBLIC Qt5::Core)

configure_package_config_file(cmake/config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/basic-config.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/basic
	NO_SET_AND_CHECK_MACRO)
write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/basic-config-version.cmake
	VERSION ${basic_VERSION}
	COMPATIBILITY SameMajorVersion)
install(
	FILES
		${CMAKE_CURRENT_BINARY_DIR}/basic-config.cmake
		${CMAKE_CURRENT_BINARY_DIR}/basic-config-version.cmake
	DESTINATION
		${CMAKE_INSTALL_DATADIR}/basic)

install(TARGETS basic EXPORT basic-targets)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(EXPORT basic-targets
	NAMESPACE basic::
	DESTINATION ${CMAKE_INSTALL_DATADIR}/basic)