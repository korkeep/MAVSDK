cmake_minimum_required(VERSION 3.1)

project(external-libressl)
include(ExternalProject)

set(LIBRESSL_VERSION 3.0.2)

list(APPEND CMAKE_ARGS
    "-DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_INSTALL_PREFIX}"
    "-DCMAKE_TOOLCHAIN_FILE:PATH=${CMAKE_TOOLCHAIN_FILE}"
    "-DCMAKE_BUILD_TYPE=Release"
    "-DBUILD_SHARED_LIBS=OFF"
    "-DLIBRESSL_APPS=OFF"
    "-DLIBRESSL_TESTS=OFF"
    )

if(IOS)
    list(APPEND CMAKE_ARGS
        "-DCMAKE_C_FLAGS=-Wno-implicit-function-declaration"
        "-DPLATFORM=${PLATFORM}"
        "-DDEPLOYMENT_TARGET=${DEPLOYMENT_TARGET}"
        "-DENABLE_STRICT_TRY_COMPILE=${ENABLE_STRICT_TRY_COMPILE}"
        )
endif()

message(STATUS "Preparing external project \"libressl\" with args:")
foreach(CMAKE_ARG ${CMAKE_ARGS})
    message(STATUS "-- ${CMAKE_ARG}")
endforeach()

ExternalProject_add(
    libressl
    URL https://ftp.openbsd.org/pub/OpenBSD/LibreSSL/libressl-${LIBRESSL_VERSION}.tar.gz
    PREFIX libressl
    CMAKE_ARGS "${CMAKE_ARGS}"
    )
