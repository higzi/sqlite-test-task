cmake_minimum_required(VERSION 3.10)
project(sqlite_build C)

set(SOURCE_FILES sqlite3.c)
set(HEADER_FILES sqlite3.h)

# .dll для Windows
# .so для Linux
add_library(sqlite3 SHARED ${SOURCE_FILES} ${HEADER_FILES})

# Добавляем текущую директорию в пути поиска заголовков (include path)
target_include_directories(sqlite3 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if(MSVC)
    message(STATUS "Configuring for Windows (MSVC)...")
    
    set_target_properties(sqlite3 PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)

    
    set_target_properties(sqlite3 PROPERTIES OUTPUT_NAME "sqlite3")
    
elseif(UNIX AND NOT APPLE)
    message(STATUS "Configuring for Linux (GCC)...")

    
    find_package(Threads REQUIRED)
    target_link_libraries(sqlite3 PRIVATE Threads::Threads)

    target_link_libraries(sqlite3 PRIVATE dl)

    target_link_libraries(sqlite3 PRIVATE m)

    set_target_properties(sqlite3 PROPERTIES OUTPUT_NAME "sqlite3")
endif()

message(STATUS "Build setup complete for ${CMAKE_SYSTEM_NAME}")
