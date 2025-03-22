cmake_minimum_required(VERSION 3.29.3 FATAL_ERROR)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


project(log_system LANGUAGES CXX)

set(LOG_SYS_DIR ${PROJECT_SOURCE_DIR})
set(LOG_SYS_SOURCE_DIR ${LOG_SYS_DIR}/source)
set(LOG_SYS_INCLUDE_DIR ${LOG_SYS_DIR}/include)


file(GLOB_RECURSE LOG_SYS_FILES CONFIGURE_DEPENDS 
    ${LOG_SYS_SOURCE_DIR}/*.h
    ${LOG_SYS_SOURCE_DIR}/*.hpp
    ${LOG_SYS_SOURCE_DIR}/*.cpp

    ${LOG_SYS_INCLUDE_DIR}/*.h
    ${LOG_SYS_INCLUDE_DIR}/*.hpp
)

add_library(log_system ${LOG_SYS_FILES})


target_compile_options(log_system PRIVATE
    $<$<CXX_COMPILER_ID:MSVC>:/W4>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall -Wextra -Wpedantic -Wno-gnu-zero-variadic-macro-arguments -Wno-gnu-anonymous-struct -Wno-nested-anon-types>
)


target_include_directories(log_system
    PUBLIC ${LOG_SYS_INCLUDE_DIR})


include(FetchContent)
FetchContent_Declare(
    spdlog
    GIT_REPOSITORY https://github.com/gabime/spdlog.git
    GIT_TAG        v1.10.0
)
FetchContent_MakeAvailable(spdlog)


target_link_libraries(log_system PUBLIC spdlog::spdlog)