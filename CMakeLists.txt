cmake_minimum_required(VERSION 3.16)
project(slog LANGUAGES C)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

option(SLOG_BUILD_SAMPLE "Build sample executable" OFF)
option(SLOG_BUILD_SHARED "Build as a shared library" ON)
option(SLOG_INSTALL "Install targets and headers" OFF)

#add_library(slog STATIC src/slog.c src/slog.h)
if (SLOG_BUILD_SHARED)
    add_library(slog SHARED)
else ()
    add_library(slog STATIC)
endif ()

target_sources(slog
        PRIVATE src/slog.c
        PUBLIC
            FILE_SET HEADERS
            BASE_DIRS include
            FILES
        include/slog/slog.h
)

target_include_directories(slog PUBLIC include)

if (SLOG_BUILD_SAMPLE)
    add_executable(slog-sample example/example.c)
    target_include_directories(slog-sample PRIVATE ${CMAKE_CURRENT_LIST_DIR}/src)
    target_link_libraries(slog-sample PRIVATE slog Threads::Threads)
endif ()

if (SLOG_INSTALL)
    install(TARGETS slog DESTINATION lib)
    install(FILES include/slog/slog.h DESTINATION include)
endif ()