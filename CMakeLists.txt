cmake_minimum_required(VERSION 3.15)

set(basic_VERSION 1.0.0)

project(basic
	VERSION ${basic_VERSION}
	DESCRIPTION "A simple utf-8 based unicode decoding and encoding library"
	HOMEPAGE_URL "https://github.com/northwindtraders/basic"
	LANGUAGES CXX)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

add_library(basic src/basic.cpp include/basic.h)

target_compile_definitions(basic
		PRIVATE "BASIC_EXPORT=__declspec(dllexport)"
		INTERFACE "BASIC_EXPORT=__declspec(dllimport)")

target_compile_features(basic PUBLIC cxx_std_17)
target_include_directories(basic
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

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