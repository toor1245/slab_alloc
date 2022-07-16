cmake_minimum_required(VERSION 3.21)
project(slab_mem_alloc C)

set(CMAKE_C_STANDARD 99)

include(CheckIncludeFile)
include(CheckSymbolExists)
include(GNUInstallDirs)

macro(setup_include_and_definitions TARGET_NAME)
    target_include_directories(${TARGET_NAME}
            PUBLIC  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
            PUBLIC  $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include/internal>)
endmacro()

macro(add_headers_and_sources HDRS_LIST_NAME SRCS_LIST_NAME)
endmacro()

set(HEADERS)
set(SOURCES)
add_headers_and_sources(HEADERS SOURCES)
add_library(slab_alloc ${HEADERS} ${SOURCES})
set_target_properties(slab_alloc PROPERTIES PUBLIC_HEADER "${HEADERS}")
setup_include_and_definitions(slab_alloc)
target_link_libraries(slab_alloc PUBLIC ${CMAKE_DL_LIBS})
target_include_directories(slab_alloc
        PUBLIC $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/slab_alloc>
        )
add_library(slab_mem_alloc::slab_alloc ALIAS slab_alloc)

#
# program : mem_alloc demo
#

add_executable(slab_alloc_demo src/main.c)
target_link_libraries(slab_alloc_demo PRIVATE slab_alloc)
add_executable(slab_mem_alloc::slab_alloc_demo ALIAS slab_alloc_demo)
