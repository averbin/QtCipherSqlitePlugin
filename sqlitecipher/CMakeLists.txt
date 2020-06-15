cmake_minimum_required( VERSION 2.6 )
project( sqlitecipher )

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set ( CMAKE_BUILD_TYPE Release )

add_definitions ( -Wall )

find_package(Qt5 COMPONENTS Core Sql REQUIRED)
include_directories (
	${CMAKE_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}
	${QT_QTCORE_INCLUDE_DIR} ${QT_QTCORE-PRIVATE_INCLUDE_DIR} ${QT_QTSQL-PRIVATE_INCLUDE_DIR}
	)
set ( sqlitecipher_HDRS
	sqlitecipher_p.h
	sqlitecipher_global.h
	)

set ( sqlitecipher_SRCS
	smain.cpp
	sqlitecipher.cpp
	)

add_library ( sqlitecipher MODULE ${sqlitecipher_SRCS} ${sqlitecipher_HDRS} ${UIS} ${RSCS} ${TRS} ${MOCS} )
target_link_libraries ( sqlitecipher Qt5::Core Qt5::Sql ${QT_QTCORE_LIBRARY} ${QT_QTCORE-PRIVATE_LIBRARY} ${QT_QTSQL-PRIVATE_LIBRARY} )

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