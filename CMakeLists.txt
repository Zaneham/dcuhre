cmake_minimum_required(VERSION 3.14)

project(dcuhre
    VERSION 0.1.0
    DESCRIPTION "Adaptive multidimensional integration (TOMS Algorithm 698)"
    LANGUAGES Fortran
)

# Options
option(DCUHRE_BUILD_TESTS "Build test programs" ON)
option(DCUHRE_BUILD_SHARED "Build shared library" OFF)

# Fortran compiler settings
if(CMAKE_Fortran_COMPILER_ID MATCHES "GNU")
    # gfortran flags for fixed-form Fortran 77/90 code
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -std=legacy -ffixed-form")
    set(CMAKE_Fortran_FLAGS_RELEASE "-O3 -funroll-loops")
    set(CMAKE_Fortran_FLAGS_DEBUG "-g -fcheck=all -fbacktrace")
elseif(CMAKE_Fortran_COMPILER_ID MATCHES "Intel")
    # Intel Fortran
    if(WIN32)
        set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} /fixed")
    else()
        set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -fixed")
    endif()
    set(CMAKE_Fortran_FLAGS_RELEASE "-O3")
    set(CMAKE_Fortran_FLAGS_DEBUG "-g -check all -traceback")
elseif(CMAKE_Fortran_COMPILER_ID MATCHES "Flang")
    set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -Mfixed")
    set(CMAKE_Fortran_FLAGS_RELEASE "-O3")
    set(CMAKE_Fortran_FLAGS_DEBUG "-g")
endif()

# Library sources
set(DCUHRE_SOURCES
    src/dcuhre.f
    src/dadhre.f
    src/dchhre.f
    src/dinhre.f
    src/drlhre.f
    src/dfshre.f
    src/dtrhre.f
    src/d07hre.f
    src/d09hre.f
    src/d113re.f
    src/d132re.f
)

# Build library
if(DCUHRE_BUILD_SHARED)
    add_library(dcuhre SHARED ${DCUHRE_SOURCES})
else()
    add_library(dcuhre STATIC ${DCUHRE_SOURCES})
endif()

target_include_directories(dcuhre PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:include>
)

set_target_properties(dcuhre PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 0
    POSITION_INDEPENDENT_CODE ON
)

# Tests
if(DCUHRE_BUILD_TESTS)
    enable_testing()

    add_executable(dtest1 test/dtest1.f)
    target_link_libraries(dtest1 PRIVATE dcuhre)

    add_executable(dtest2 test/dtest2.f)
    target_link_libraries(dtest2 PRIVATE dcuhre)

    add_test(NAME dtest1 COMMAND dtest1)
    add_test(NAME dtest2 COMMAND dtest2)
endif()

# Installation
include(GNUInstallDirs)

install(TARGETS dcuhre
    EXPORT dcuhre-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(EXPORT dcuhre-targets
    FILE dcuhre-targets.cmake
    NAMESPACE dcuhre::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/dcuhre
)

# Package config
include(CMakePackageConfigHelpers)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/dcuhre-config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/dcuhre-config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/dcuhre
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/dcuhre-config-version.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/dcuhre-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/dcuhre-config-version.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/dcuhre
)
