cmake_minimum_required (VERSION 3.25)

project(shared-utils)

find_package(PkgConfig REQUIRED)
pkg_check_modules(CAIRO cairo REQUIRED)
pkg_check_modules(PIXMAN pixman-1 REQUIRED)
pkg_check_modules(PNG libpng REQUIRED)
pkg_check_modules(LIBWESTON libweston-${LIBWESTON_VER} REQUIRED)

include_directories(
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/..
    ${CAIRO_INCLUDE_DIRS}
    ${PIXMAN_INCLUDE_DIRS}
    ${LIBWESTON_INCLUDE_DIRS}
)

link_directories(
    ${CAIRO_LIBRARY_DIRS}
    ${PIXMAN_LIBRARY_DIRS}
    ${LIBWESTON_LIBRARY_DIRS}
)

set(LIBS
    ${LIBS}
    m
    ${CAIRO_LIBRARIES}
    ${PIXMAN_LIBRARIES}
    ${PNG_LIBRARIES}
    ${LIBWESTON_LIBRARIES}
)

add_library(${PROJECT_NAME} STATIC
    cairo-util.c
    file-util.c
    image-loader.c
    os-compatibility.c
    process-util.c
)

target_link_libraries(${PROJECT_NAME} ${LIBS})

add_library(shared-utils-headers INTERFACE)
target_include_directories(shared-utils-headers INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/..)

