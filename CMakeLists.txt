cmake_minimum_required(VERSION 3.29.3 FATAL_ERROR)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


project(log_system LANGUAGES CXX)

set(LOG_SYS_DIR ${PROJECT_SOURCE_DIR})
set(LOG_SYS_SOURCE_DIR ${LOG_SYS_DIR}/source)
set(LOG_SYS_INCLUDE_DIR ${LOG_SYS_DIR}/include)

set(LOG_SYS_THIRDPARTY_DIR ${LOG_SYS_DIR}/thirdparty)
set(LOG_SYS_THIRDPARTY_SPDLOG_DIR ${LOG_SYS_THIRDPARTY_DIR}/spdlog)


add_subdirectory(${LOG_SYS_THIRDPARTY_SPDLOG_DIR})


file(GLOB_RECURSE LOG_SYS_FILES CONFIGURE_DEPENDS 
    ${LOG_SYS_SOURCE_DIR}/*.h
    ${LOG_SYS_SOURCE_DIR}/*.hpp
    ${LOG_SYS_SOURCE_DIR}/*.cpp

    ${LOG_SYS_INCLUDE_DIR}/*.h
    ${LOG_SYS_INCLUDE_DIR}/*.hpp
)

add_library(log_system ${LOG_SYS_FILES})

target_include_directories(log_system
    PUBLIC ${LOG_SYS_INCLUDE_DIR})

target_link_libraries(log_system PUBLIC spdlog::spdlog)

target_compile_options(log_system PRIVATE
    $<$<CXX_COMPILER_ID:MSVC>:/W4>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall -Wextra -Wpedantic -Wno-gnu-zero-variadic-macro-arguments -Wno-gnu-anonymous-struct -Wno-nested-anon-types>
)