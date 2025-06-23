cmake_minimum_required (VERSION 3.25)

project(ivi-webruntime-ui-controller)

find_package(PkgConfig REQUIRED)
pkg_check_modules(WAYLAND_SERVER wayland-server>=1.23.0 REQUIRED)
pkg_check_modules(WESTON weston>=14.0.0 REQUIRED)

include_directories(
    ${CMAKE_CURRENT_BINARY_DIR}
    ${WAYLAND_SERVER_INCLUDE_DIRS}
    ${WESTON_INCLUDE_DIRS}
)

link_directories(
    ${WAYLAND_SERVER_LIBRARY_DIRS}
)

set(LIBS
    ${LIBS}
    ${WAYLAND_SERVER_LIBRARIES}
    ivi-protocols
    ivi-protocols-headers
    shared-utils
    shared-utils-headers
)

set(CMAKE_C_LDFLAGS "-module -avoid-version")

add_library(${PROJECT_NAME} MODULE
    ivi-webruntime-ui-controller.c
)

set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "")

target_link_libraries(${PROJECT_NAME} ${LIBS})

install (
    TARGETS ${PROJECT_NAME}
    LIBRARY DESTINATION ${LIBWESTON_LIBDIR}/weston
)
