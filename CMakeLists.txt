cmake_minimum_required (VERSION 3.11)

project (winobjc)

list (APPEND CMAKE_MODULE_PATH "${SOURCE_DIR}/scripts")
include (CommonVariables)

add_custom_target (prep
    COMMENT "Superbuild"
    COMMAND ninja config-winobjc-x86-Debug # TODO: Don't hardcode this.
    WORKING_DIRECTORY "${BINARY_DIR}"
    USES_TERMINAL)

add_subdirectory (tools)
