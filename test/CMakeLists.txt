cmake_minimum_required(VERSION 3.3 FATAL_ERROR)

set(projectName test)

add_definitions( -Wall )

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(Qt5 COMPONENTS Test Sql REQUIRED)

add_executable(${projectName} main.cpp)

target_link_libraries(${projectName} Qt5::Test Qt5::Sql)
target_compile_options(${projectName} PRIVATE  ${COMPILE_OPTIONS})

get_target_property(QtCore_location Qt5::Test LOCATION)
get_target_property(QtSql_location Qt5::Sql LOCATION)

qt5_use_modules(${projectName} Test Sql)