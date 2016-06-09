PROJECT (See5)

cmake_minimum_required(VERSION 2.8)

#default compiler flags

SET(C_STANDARD_VALUE "c99" CACHE STRING "specify the c standard, use iso9899:1990 for c89")
SET(CXX_STANDARD_VALUE "c++0x" CACHE STRING "specify the c standard, use c++11 for newer compilers")

SET( CMAKE_CXX_FLAGS_RELEASE "-fPIC -std=${CXX_STANDARD_VALUE} -DVerbOpt -ffloat-store -Ofast" CACHE STRING "" FORCE)
SET( CMAKE_C_FLAGS_RELEASE "-fPIC -std=${C_STANDARD_VALUE} -DVerbOpt -ffloat-store -Ofast" CACHE STRING "" FORCE)
SET( CMAKE_CXX_FLAGS_DEBUG "-fPIC -std=${CXX_STANDARD_VALUE} -DVerbOpt -ffloat-store -g3 -O0" CACHE STRING "" FORCE )
SET( CMAKE_C_FLAGS_DEBUG "-fPIC -std=${C_STANDARD_VALUE} -DVerbOpt -ffloat-store -g3 -O0" CACHE STRING "" FORCE)

IF( CMAKE_SIZEOF_VOID_P EQUAL 8 )
	SET(NATIVE_OS_ARCH_SIZE 64)
ELSE()
	SET(NATIVE_OS_ARCH_SIZE 32)
ENDIF()

SET(FORCE_32_BIT OFF CACHE BOOL "force bulding of 32 bit binaries")
IF(FORCE_32_BIT)
IF(NOT MSVC)
	SET( CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -m32" CACHE STRING "" FORCE)
	SET( CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -m32" CACHE STRING "" FORCE)
	SET( CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -m32" CACHE STRING "" FORCE)
	SET( CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -m32" CACHE STRING "" FORCE)
ENDIF()
ENDIF()

#define the variable with the register data size for the OS
if( ${NATIVE_OS_ARCH_SIZE} EQUAL 64 AND FORCE_32_BIT )
	SET (BINARY_BITS 32)
else()
	SET (BINARY_BITS ${NATIVE_OS_ARCH_SIZE})
endif()

SET(BUILD_SHARED_LIBS ON CACHE BOOL "choose between static or dynamic linking")
SET(BUILD_SHARED_BASE_LIBS OFF CACHE BOOL "enable/disable dynamic linking for common libraries, STATIC linking is default")

#profiling compiler options
IF(ENABLE_PROFILING)
	add_definitions(
		-pg
		)
ENDIF(ENABLE_PROFILING)

#coverage compiler options
IF(ENABLE_GCOV)
	add_definitions(
		-fprofile-arcs
		-ftest-coverage
		)
ENDIF(ENABLE_GCOV)

find_package(Doxygen)
if(DOXYGEN_FOUND)
	add_custom_target(doc
		${DOXYGEN_EXECUTABLE} ${CMAKE_SOURCE_DIR}/Doxyfile
		WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
		COMMENT "Generating API documentation with Doxygen" VERBATIM
		)
endif(DOXYGEN_FOUND)

add_subdirectory(apps)

