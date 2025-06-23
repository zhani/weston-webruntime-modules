cmake_minimum_required (VERSION 3.25)

project(weston-webruntime-modules)

include(GNUInstallDirs)

set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wall -Wextra -Wno-unused-parameter")
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wno-unused-parameter")

set(LIBWESTON_VER 14)
set(DATADIR ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATADIR})

configure_file(config.h.in config.h)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_subdirectory(protocol)
add_subdirectory(shared)

add_subdirectory(ivi-shell-webruntime-ui)
add_subdirectory(ivi-webruntime-ui-controller)
