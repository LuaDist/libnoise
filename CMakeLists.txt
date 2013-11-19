# Copyright (C) 2013 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

project( libnoise CXX )
cmake_minimum_required ( VERSION 2.8 )
include( cmake/dist.cmake )

# Determine install host
if ( WIN32 AND NOT CYGWIN )
	if( MSVC )
		add_definitions( -D_CRT_SECURE_NO_DEPRECATE )
		set( DEF_FILE src/win32/libnoise.def )
		set( DLL_RC_FILE src/win32/noise.rc )
	endif()
endif()

# Build Libraries
include_directories( src src/model src/module )
file( GLOB_RECURSE SRC_MODEL "src/model/*.cpp" )
file( GLOB_RECURSE SRC_MODULE "src/module/*.cpp" )

file( GLOB HEADERS "src/*.h" )
file( GLOB HEADERS_MODEL "src/model/*.h" )
file( GLOB HEADERS_MODULE "src/module/*.h" )

set( LIBRARY_NAME noise)
if( MSVC )
	set( LIBRARY_NAME libnoise)
endif()

add_library( ${LIBRARY_NAME} src/latlon.cpp src/noisegen.cpp ${SRC_MODEL} ${SRC_MODULE} ${DEF_FILE} ${DLL_RC_FILE})

# Install
install_library( ${LIBRARY_NAME} )
install_header( ${HEADERS} INTO libnoise)
install_header( ${HEADERS_MODEL} INTO libnoise/model )
install_header( ${HEADERS_MODULE} INTO libnoise/module )
install_doc( COPYING.txt doc/readme.txt doc/html doc/htmldata doc/img )
