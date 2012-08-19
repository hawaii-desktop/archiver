project(Archiver)

cmake_minimum_required(VERSION 2.8.7)

# Enable automoc and include current directory
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Set version
set(ARCHIVER_VERSION_MAJOR 0)
set(ARCHIVER_VERSION_MINOR 0)
set(ARCHIVER_VERSION_PATCH 0)
set(ARCHIVER_VERSION_STRING "${ARCHIVER_VERSION_MAJOR}.${ARCHIVER_VERSION_MINOR}.${ARCHIVER_VERSION_PATCH}")

# Our CMake modules
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules")

# Installation directories
include(MauiInstallDirs)

# Make CPack available to easy generate binary packages
include(CPack)

# Add custom uninstall target
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)
add_custom_target(uninstall "${CMAKE_COMMAND}" -P "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake")

# Build flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fvisibility=hidden -fvisibility-inlines-hidden -Wall -std=c++0x")

# Assume release build by default
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "" FORCE)
endif()

# Disable debug output for release builds
if(CMAKE_BUILD_TYPE MATCHES "^[Rr]elease$")
    add_definitions(-DQT_NO_DEBUG_OUTPUT)
endif()

# Find Vibe libraries
find_package(VibeWidgets REQUIRED)
if(NOT VibeWidgets_FOUND)
    message(FATAL_ERROR "VibeWidgets module is required!")
endif()

# Subdirectories
add_subdirectory(src)