cmake_minimum_required(VERSION 3.3 FATAL_ERROR)
project( sqlitecipher )

set(CMAKE_CXX_STANDARD 11) # This is equal to QMAKE_CXX_FLAGS += -std=c++0x

add_definitions ( -Wall )

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(SQL_DIR ${CMAKE_CURRENT_SOURCE_DIR}/sqlite3)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
#set(CMAKE_AUTOUIC ON)

find_package(Qt5 COMPONENTS Core Sql REQUIRED)
find_program(QMAKE_EXEC qmake DOC "Complete path to the qmake executable")

set(INCLUDES
    sqlitecipher_p.h
    sqlitecipher_global.h
)

set(SOURCES
    smain.cpp
    sqlitecipher.cpp
)

add_library(sqlitecipher MODULE ${INCLUDES} ${SOURCES} SqliteCipherDriverPlugin.json)

target_link_libraries(sqlitecipher Qt5::Core Qt5::Sql)

# We need this part of code to find private headers in Qt.
foreach(module Core Sql PrintSupport)
    set(qt_module Qt${module})
    set(qt5_module Qt5${module})
    find_package(${qt5_module})
    if("${${qt5_module}_PRIVATE_INCLUDE_DIRS}" STREQUAL "")
        foreach(base_dir ${${qt5_module}_INCLUDE_DIRS})
            if("${base_dir}" MATCHES "/${qt_module}\$")
                list(APPEND ${qt5_module}_PRIVATE_INCLUDE_DIRS "${base_dir}/${${qt5_module}_VERSION}/${qt_module}")
            endif()
        endforeach()
    endif()
    target_include_directories(sqlitecipher PRIVATE ${${qt5_module}_PRIVATE_INCLUDE_DIRS})
endforeach()

get_filename_component( HEADERS_PREFIX sqlite3 ABSOLUTE)
target_include_directories( sqlitecipher PRIVATE . ${HEADERS_PREFIX} sqlite3)

get_target_property(QtCore_location Qt5::Core LOCATION)
get_target_property(QtSql_location Qt5::Sql LOCATION)

target_compile_features(sqlitecipher PUBLIC cxx_std_14)

#parameters from pri file.

if(WIN32)
	if(MSVC)
		#target_compile_options(sqlitecipher PUBLIC "/Zc:__cplusplus")
	endif()
    add_definitions(
	-DSQLITE_OS_WINRT
	-DSQLITE_CORE
	-DSQLITE_HAS_CODEC
	-DCODEC_TYPE
	-DCODEC_TYPE_UNKNOWN
	)
endif()

add_definitions(
	-DCHECK_SQLITE_KEY
    -DSQLITE_OMIT_LOAD_EXTENSION 
	-DSQLITE_OMIT_COMPLETE
	-DSQLITE_ENABLE_RTREE
)

set(THREADSAFE 1)
set(SQLITE_HAS_CODEC 1)
set(MAX_ATTACHED 10)
set(TEMP_STORE 2)
set(SQLITE_WIN32_FILEMAPPING_API 1)

add_definitions(
	-DNDEBUG
    -DSQLITE_OMIT_LOAD_EXTENSION
    -DSQLITE_OMIT_COMPLETE
    -DSQLITE_ENABLE_FTS3
    -DSQLITE_ENABLE_FTS3_PARENTHESIS
    -DSQLITE_OS_WINRT
    -DQT_NO_CAST_TO_ASCII
    -DQT_NO_CAST_FROM_ASCII
    -D_CRT_SECURE_NO_WARNINGS
    -D_CRT_SECURE_NO_DEPRECATE
    -D_CRT_NONSTDC_NO_DEPRECATE
    -DTHREADSAFE=${THREADSAFE}
    -DSQLITE_MAX_ATTACHED=${MAX_ATTACHED}
    -DSQLITE_SOUNDEX
    -DSQLITE_ENABLE_EXPLAIN_COMMENTS
    -DSQLITE_ENABLE_COLUMN_METADATA
    -DSQLITE_HAS_CODEC=${SQLITE_HAS_CODEC}
    -DCODEC_TYPE
    -DSQLITE_SECURE_DELETE
    -DSQLITE_ENABLE_FTS4
    -DSQLITE_ENABLE_FTS5
    -DSQLITE_ENABLE_JSON1
    -DSQLITE_CORE
    -DSQLITE_ENABLE_EXTFUNC
    -DSQLITE_ENABLE_CSV
    -DSQLITE_ENABLE_SHA3
    -DSQLITE_ENABLE_CARRAY
    -DSQLITE_ENABLE_FILEIO
    -DSQLITE_ENABLE_SERIES
    -DSQLITE_TEMP_STORE=${TEMP_STORE}
    -DSQLITE_USE_URI
    -DSQLITE_USER_AUTHENTICATION
	-DDISABLE_LIBRARY_FEATURES
	-D_QNX_SOURCE
	-DSQLITE_WIN32_FILEMAPPING_API=${SQLITE_WIN32_FILEMAPPING_API}
    )

add_subdirectory(sqlite3)
file(GLOB sqlitecipher "${CMAKE_CURRENT_LIST_DIR}/SqliteCipherDriverPlugin*sqldrivers.cmake")