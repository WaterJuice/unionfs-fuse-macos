cmake_minimum_required(VERSION 3.8)

project(unionfs-fuse VERSION 2.3
                     DESCRIPTION "user-space implementation of union filesystem (with help of fuse)"
                     LANGUAGES C)

INCLUDE(CheckIncludeFiles)

# Set a default build type for single-configuration
# CMake generators if no build type is set.
IF(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
	SET(CMAKE_BUILD_TYPE Release)
ENDIF(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)

if (UNIX AND APPLE)
	include_directories( /usr/local/include/ )
	link_directories( /usr/local/lib/ )
#	add_compile_options( -Wno-unused-parameter )
	#add_compile_options( -Wno-macro-redefined )
	add_compile_options( -Wno-tautological-overlap-compare )
	set(CMAKE_OSX_ARCHITECTURES arm64;x86_64)
	option(WITH_LIBFUSE3 "Enable libfuse3 support" OFF)
endif()


option(WITH_XATTR "Enable support for extended attributes" ON)

IF (WITH_XATTR)
	CHECK_INCLUDE_FILES("sys/xattr.h" HAVE_LIBC_XATTR)
	CHECK_INCLUDE_FILES("attr/xattr.h" HAVE_LIBATTR_XATTR)

	IF (HAVE_LIBC_XATTR)
		add_definitions(-DLIBC_XATTR)
	ELSEIF(HAVE_LIBATTR_XATTR)
		add_definitions(-DLIBATTR_XATTR)
	ENDIF()

	IF (NOT HAVE_LIBC_XATTR AND NOT HAVE_LIBATTR_XATTR)
		add_definitions(-DDISABLE_XATTR)
	ENDIF()
ELSE (WITH_XATTR)
	add_definitions(-DDISABLE_XATTR)
ENDIF (WITH_XATTR)

INSTALL(PROGRAMS mount.unionfs DESTINATION sbin)

add_subdirectory(src)
add_subdirectory(man)
