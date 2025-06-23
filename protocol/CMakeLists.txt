cmake_minimum_required (VERSION 3.25)

project(ivi-protocols)

find_package(PkgConfig REQUIRED)
pkg_check_modules(WAYLAND_CLIENT wayland-client>=1.14.0 REQUIRED)
pkg_check_modules(WAYLAND_SERVER wayland-server>=1.14.0 REQUIRED)

find_program(WAYLAND_SCANNER_EXECUTABLE NAMES wayland-scanner)

set(IVI_APPLICATION_XML ${CMAKE_SOURCE_DIR}/protocol/ivi-application.xml)
set(IVI_APPLICATION_PROTOCOL ${CMAKE_CURRENT_BINARY_DIR}/ivi-application-protocol.c)
set(IVI_APPLICATION_CLIENT_PROTOCOL_HEADER ${CMAKE_CURRENT_BINARY_DIR}/ivi-application-client-protocol.h)

set(IVI_HMI_CONTROLLER_XML ${CMAKE_SOURCE_DIR}/protocol/ivi-hmi-controller.xml)
set(IVI_HMI_CONTROLLER_PROTOCOL ${CMAKE_CURRENT_BINARY_DIR}/ivi-hmi-controller-protocol.c)
set(IVI_HMI_CLIENT_PROTOCOL_HEADER ${CMAKE_CURRENT_BINARY_DIR}/ivi-hmi-controller-client-protocol.h)
set(IVI_HMI_SERVER_PROTOCOL_HEADER ${CMAKE_CURRENT_BINARY_DIR}/ivi-hmi-controller-server-protocol.h)

add_custom_command(
    OUTPUT  ${IVI_APPLICATION_CLIENT_PROTOCOL_HEADER}
    COMMAND ${WAYLAND_SCANNER_EXECUTABLE} client-header
            < ${IVI_APPLICATION_XML}
            > ${IVI_APPLICATION_CLIENT_PROTOCOL_HEADER}
    DEPENDS ${IVI_APPLICATION_XML}
)

add_custom_command(
    OUTPUT  ${IVI_APPLICATION_PROTOCOL}
    COMMAND ${WAYLAND_SCANNER_EXECUTABLE} code
            < ${IVI_APPLICATION_XML}
            > ${IVI_APPLICATION_PROTOCOL}
    DEPENDS ${IVI_APPLICATION_XML}
)

add_custom_command(
    OUTPUT  ${IVI_HMI_CLIENT_PROTOCOL_HEADER}
    COMMAND ${WAYLAND_SCANNER_EXECUTABLE} client-header
            < ${IVI_HMI_CONTROLLER_XML}
            > ${IVI_HMI_CLIENT_PROTOCOL_HEADER}
    DEPENDS ${IVI_HMI_CONTROLLER_XML}
)

add_custom_command(
    OUTPUT  ${IVI_HMI_SERVER_PROTOCOL_HEADER}
    COMMAND ${WAYLAND_SCANNER_EXECUTABLE} server-header
            < ${IVI_HMI_CONTROLLER_XML}
            > ${IVI_HMI_SERVER_PROTOCOL_HEADER}
    DEPENDS ${IVI_HMI_CONTROLLER_XML}
)

add_custom_command(
    OUTPUT  ${IVI_HMI_CONTROLLER_PROTOCOL}
    COMMAND ${WAYLAND_SCANNER_EXECUTABLE} code
            < ${IVI_HMI_CONTROLLER_XML}
            > ${IVI_HMI_CONTROLLER_PROTOCOL}
    DEPENDS ${IVI_HMI_CONTROLLER_XML}
)

add_custom_target(generate-protocol-headers ALL DEPENDS
    ${IVI_APPLICATION_CLIENT_PROTOCOL_HEADER}
    ${IVI_HMI_CLIENT_PROTOCOL_HEADER}
    ${IVI_HMI_SERVER_PROTOCOL_HEADER}
)

add_library(${PROJECT_NAME} STATIC
    ${IVI_APPLICATION_PROTOCOL}
    ${IVI_HMI_CONTROLLER_PROTOCOL}
)

add_library(ivi-protocols-headers INTERFACE)
target_include_directories(ivi-protocols-headers INTERFACE ${CMAKE_CURRENT_BINARY_DIR})
