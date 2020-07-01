cmake_minimum_required(VERSION 3.3 FATAL_ERROR)

set(projectName testapp)

project( ${projectName} )

add_definitions( -Wall )

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(Qt5 COMPONENTS Core Sql REQUIRED)

add_executable(${projectName} main.cpp)

target_link_libraries(${projectName} Qt5::Core Qt5::Sql)
target_compile_options(${projectName} PRIVATE  ${COMPILE_OPTIONS})

get_target_property(QtCore_location Qt5::Core LOCATION)
get_target_property(QtSql_location Qt5::Sql LOCATION)

qt5_use_modules(${projectName} Core Sql)