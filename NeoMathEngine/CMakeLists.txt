cmake_minimum_required(VERSION 3.11 FATAL_ERROR)

project(NeoMathEngine LANGUAGES CXX)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../cmake)

if(NOT CMAKE_PROJECT_NAME STREQUAL "NeoML")
    include(Settings)
    include(Version)
    if(USE_FINE_OBJECTS)
        include(FineInstall)
    else()
        include(GNUInstallDirs)
    endif()

    set_global_variables()
endif()

set(PROJECT_VERSION ${FINE_VERSION_MAJOR}.${FINE_VERSION_MINOR}.${FINE_VERSION_PATCH})

get_directory_property(IS_SUBPROJECT PARENT_DIRECTORY)

# Build NeoMathEngine tests
option(NeoMathEngine_BUILD_TESTS "Build all of NeoMathEngine's tests." ON)

# Build NeoMathEngine as shared library
option(NeoMathEngine_BUILD_SHARED "Build NeoMathEngine as shared library." ON)

# Install NeoMathEngine
if(DEFINED NeoML_INSTALL)
    option(NeoMathEngine_INSTALL "Install NeoMathEngine." ${NeoML_INSTALL})
else()
    set(NeoMathEngine_INSTALL ON)
endif()

add_subdirectory(src)

# Tests
if(NeoMathEngine_BUILD_TESTS AND (NOT IS_SUBPROJECT OR USE_FINE_OBJECTS) AND NOT IOS AND NOT ANDROID)
    enable_testing()
    add_subdirectory(test/FullTestDesktop)
endif()
