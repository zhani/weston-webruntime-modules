cmake_minimum_required (VERSION 3.25)

project(ivi-shell-webruntime-ui)

find_package(PkgConfig REQUIRED)
pkg_check_modules(WAYLAND_CLIENT wayland-client>=1.23.0 REQUIRED)
pkg_check_modules(WAYLAND_CURSOR wayland-cursor REQUIRED)
pkg_check_modules(CAIRO cairo REQUIRED)
pkg_check_modules(PIXMAN pixman-1 REQUIRED)
pkg_check_modules(LIBWESTON libweston-${LIBWESTON_VER} REQUIRED)

include_directories(
    ${CMAKE_CURRENT_BINARY_DIR}
    ${WAYLAND_CLIENT_INCLUDE_DIRS}
    ${WAYLAND_CURSOR_INCLUDE_DIRS}
    ${LIBWESTON_INCLUDE_DIRS}
    ${CAIRO_INCLUDE_DIRS}
    ${PIXMAN_INCLUDE_DIRS}
)

link_directories(
    ${WAYLAND_CLIENT_LIBRARY_DIRS}
    ${WAYLAND_CURSOR_LIBRARY_DIRS}
    ${CAIRO_LIBRARY_DIRS}
    ${PIXMAN_LIBRARY_DIRS}
    ${LIBWESTON_LIBRARY_DIRS}
)

set(LIBS
    ${LIBS}
    ${CAIRO_LIBRARIES}
    ${PIXMAN_LIBRARIES}
    ${LIBWESTON_LIBRARIES}
    ${WAYLAND_CLIENT_LIBRARIES}
    ${WAYLAND_CURSOR_LIBRARIES}
    ivi-protocols
    ivi-protocols-headers
    shared-utils
    shared-utils-headers
)

add_executable(${PROJECT_NAME}
    ivi-shell-webruntime-ui.c
)

target_link_libraries(${PROJECT_NAME} PRIVATE ${LIBS})

install (
    TARGETS ${PROJECT_NAME}
    DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBEXECDIR}
)
