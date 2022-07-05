cmake_minimum_required(VERSION 3.10)

project("CryptoSuites" VERSION 1.0.1)

set(CMAKE_CXX_STANDARD 11)
SET(CMAKE_BUILD_TYPE "Release")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -Wall -Wextra -Wno-unused-parameter -Wno-unused-variable -fmacro-prefix-map=${CMAKE_SOURCE_DIR}=/safeheron")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -Wall -Wextra -Wno-unused-parameter -Wno-unused-variable -fmacro-prefix-map=${CMAKE_SOURCE_DIR}=/safeheron")

cmake_policy(SET CMP0076 NEW)
cmake_policy(SET CMP0077 NEW)
cmake_policy(SET CMP0079 NEW)

find_package(PkgConfig REQUIRED)
pkg_search_module(PROTOBUF REQUIRED protobuf)  # depend on pkg-config, this looks for opencv.pc file
#set(OPENSSL_USE_STATIC_LIBS TRUE)
find_package(OpenSSL REQUIRED)
add_library(${CMAKE_PROJECT_NAME} SHARED "")

target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
        third_party/crypto-bn-cpp/src
        third_party/crypto-hash-cpp/src
        third_party/crypto-encode-cpp/src
        third_party/crypto-curve-cpp/src
        third_party/crypto-commitment-cpp/src
        third_party/crypto-paillier-cpp/src
        third_party/crypto-sss-cpp/src
        third_party/crypto-zkp-cpp/src
        third_party/crypto-mta-cpp/src
        third_party/crypto-bip32-cpp/src
        third_party/crypto-ecies-cpp/src
        ${PROTOBUF_INCLUDE_DIRS}
        ${OPENSSL_INCLUDE_DIR}
        /usr/local/include
        )

target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE /usr/local/lib)
target_link_libraries(${CMAKE_PROJECT_NAME}
        ${PROTOBUF_LIBRARIES}
        OpenSSL::Crypto
        -ldl
        )

set(ENABLE_ASSEMBLE ON)
add_subdirectory(third_party/crypto-bn-cpp/src)
add_subdirectory(third_party/crypto-hash-cpp/src)
add_subdirectory(third_party/crypto-encode-cpp/src)
add_subdirectory(third_party/crypto-curve-cpp/src)
add_subdirectory(third_party/crypto-commitment-cpp/src)
add_subdirectory(third_party/crypto-paillier-cpp/src)
add_subdirectory(third_party/crypto-sss-cpp/src)
add_subdirectory(third_party/crypto-zkp-cpp/src)
add_subdirectory(third_party/crypto-mta-cpp/src)
add_subdirectory(third_party/crypto-bip32-cpp/src)
add_subdirectory(third_party/crypto-ecies-cpp/src)

option(ENABLE_TESTS "Enable tests" OFF)
if (${ENABLE_TESTS})
    enable_testing()
    include_directories(
            third_party/crypto-bn-cpp/src
            third_party/crypto-hash-cpp/src
            third_party/crypto-encode-cpp/src
            third_party/crypto-curve-cpp/src
            third_party/crypto-commitment-cpp/src
            third_party/crypto-paillier-cpp/src
            third_party/crypto-sss-cpp/src
            third_party/crypto-zkp-cpp/src
            third_party/crypto-mta-cpp/src
            third_party/crypto-bip32-cpp/src
            third_party/crypto-ecies-cpp/src
            /usr/local/include
    )
    link_directories(/usr/local/lib)

    add_subdirectory(third_party/crypto-bn-cpp/test)
    add_subdirectory(third_party/crypto-hash-cpp/test)
    add_subdirectory(third_party/crypto-encode-cpp/test)
    add_subdirectory(third_party/crypto-curve-cpp/test)
    add_subdirectory(third_party/crypto-commitment-cpp/test)
    add_subdirectory(third_party/crypto-paillier-cpp/test)
    add_subdirectory(third_party/crypto-sss-cpp/test)
    add_subdirectory(third_party/crypto-zkp-cpp/test)
    add_subdirectory(third_party/crypto-mta-cpp/test)
    add_subdirectory(third_party/crypto-bip32-cpp/test)
    add_subdirectory(third_party/crypto-ecies-cpp/test)
endif()

include("cmake/setup_installation.cmake")