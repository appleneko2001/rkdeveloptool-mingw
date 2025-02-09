cmake_minimum_required (VERSION 3.10)
set(CMAKE_CXX_STANDARD 17)

# require pkg-config to import command pkg_search_module
find_package(PkgConfig REQUIRED)

# The version number.
set (RKDevelopTool_PACKAGE_VERSION "1.3")

file (GLOB LOCAL_CPP_FILES *.cpp)
set (SOURCE_FILES ${SOURCE_FILES} ${LOCAL_CPP_FILES})

file (GLOB LOCAL_H_FILES *.h)
set (HEADER_FILES ${HEADER_FILES} ${LOCAL_H_FILES})

# find installed libusb-1.0 instead of set then manually in CMAKELISTS
# can be installed by using `pacman -S mingw-w64-ucrt-x86_64-libusb`
# (this package is UCRT variant since im prefer use MinGW UCRT)
# package details: https://packages.msys2.org/base/mingw-w64-libusb
pkg_search_module(LIBUSB REQUIRED libusb-1.0 > 1.0)

# same as libiconv. you can use `pacman -S iconv` command in MinGW instance to install this package.
#pkg_search_module(ICONV REQUIRED iconv > 1.0)

project ("rkDevelopTool")

# Turn off build log
# set (CMAKE_VERBOSE_MAKEFILE ON)

# Configure a header file to pass some of the CMake settings
# to the source code
configure_file (
    "${PROJECT_SOURCE_DIR}/config.h.in"
    "${PROJECT_BINARY_DIR}/config.h"
)

# add the binary tree to the search path for include files
# so that we will find config.h
include_directories (
    "${PROJECT_BINARY_DIR}"
    "${PROJECT_SOURCE_DIR}"
    "${LIBUSB_INCLUDEDIR}"
#   "${ICONV_INCLUDEDIR}"
)

link_directories (
    ${LIBUSB_LIBDIR}
#   ${ICONV_LIBDIR}
)

add_executable (rkdevtool ${SOURCE_FILES})

target_link_libraries (rkdevtool
    ${LIBUSB_LINK_LIBRARIES}
#   ${ICONV_LINK_LIBRARIES}
)

if (WIN32)
    add_definitions(-DDEVTOOL_WIN32)
elseif (UNIX)
    add_definitions(-DDEVTOOL_UNIX)
endif ()