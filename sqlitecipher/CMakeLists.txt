cmake_minimum_required(VERSION 3.3 FATAL_ERROR)
project( sqlitecipher )

add_definitions ( -Wall )

find_package(Qt5 COMPONENTS Core Sql REQUIRED)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(INCLUDES
	sqlitecipher_p.h
	sqlitecipher_global.h
)

set(SOURCES
    smain.cpp
    sqlitecipher.cpp
)

add_library ( sqlitecipher MODULE ${INCLUDES} ${SOURCES})

target_link_libraries ( sqlitecipher Qt5::Core Qt5::Sql)

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

get_target_property(QtCore_location Qt5::Core LOCATION)
message(STATUS "QtCore_location : ${QtCore_location}")
get_target_property(QtSql_location Qt5::Sql LOCATION)
message(STATUS "QtSql_location : ${QtSql_location}")