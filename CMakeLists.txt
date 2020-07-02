cmake_minimum_required( VERSION 3.3 FATAL_ERROR)

project( QtCipherSqlitePlugin )

set(CMAKE_CXX_STANDARD 14)

#this is a private property used only when building the target and won’t affect its users 
add_definitions ( -Wall )

set(CMAKE_INCLUDE_CURRENT_DIR ON)

add_subdirectory(sqlitecipher)
add_subdirectory(test)
add_subdirectory(testapp)