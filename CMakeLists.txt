# =============================================================================
#
#   @file CMakeLists.txt
#
#   linenoise CMakeList
#
#   @version    0.1.0
#
#   @copyright  Copyright &copy; &nbsp; 2015 Bithium S.A.
#  
# =============================================================================

cmake_minimum_required (VERSION 2.8.11)

project (linenoise)

# ==============================================================================
# Default project setup.

## Standard project paths.
set (PROJECT_INCLUDE_DIR  "${PROJECT_SOURCE_DIR}" )

set (PROJECT_VERSION "1.0.0")

# ==============================================================================
# Library
# ==============================================================================

add_library(linenoise STATIC "linenoise.c")
target_include_directories(linenoise INTERFACE ${PROJECT_INCLUDE_DIR})

# ==============================================================================
# Package
# ==============================================================================

string(TOLOWER "${PROJECT_NAME}" PACKAGE_NAME)
string(TOUPPER "${PROJECT_NAME}" PACKAGE_PREFIX)

# ... for build folder

get_target_property(_include_dirs linenoise INTERFACE_INCLUDE_DIRECTORIES)

foreach(_include_dir ${_include_dirs})
    file(RELATIVE_PATH _rel_include_dir "${CMAKE_BINARY_DIR}" "${_include_dir}")
    list(APPEND CONF_INCLUDE_DIRS "\${${PACKAGE_PREFIX}_CMAKE_DIR}/${_rel_include_dir}")
endforeach()

configure_file("config.cmake.in" "${CMAKE_BINARY_DIR}/${PACKAGE_NAME}-config.cmake" @ONLY)

configure_file("config-version.cmake.in"
               "${CMAKE_BINARY_DIR}/${PACKAGE_NAME}-config-version.cmake" @ONLY)

# Add all targets to the build-tree export set
export(TARGETS linenoise FILE "${CMAKE_BINARY_DIR}/${PACKAGE_NAME}-targets.cmake")
