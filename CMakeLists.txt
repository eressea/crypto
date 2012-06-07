cmake_minimum_required(VERSION 2.8)
project (crypto C)

IF(CMAKE_COMPILER_IS_GNUCC)
    SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pedantic -Wall -Werror -Wno-unknown-pragmas -Wstrict-prototypes -Wpointer-arith -Wno-char-subscripts -Wno-long-long")
ELSE(CMAKE_COMPILER_IS_GNUCC)
    MESSAGE(STATUS "Unknown compiler ${CMAKE_C_COMPILER_ID}")
ENDIF(CMAKE_COMPILER_IS_GNUCC)

FILE (GLOB LIB_HDR *.h)
SET (LIB_SRC mt19937ar.c md5.c)
add_library (${PROJECT_NAME} ${LIB_SRC} ${LIB_HDR})

add_executable (md5main md5main.c)
target_link_libraries (md5main crypto)
find_library(M_LIB m)
if (EXISTS ${M_LIB})
    target_link_libraries (md5main ${M_LIB})
endif (EXISTS ${M_LIB})

enable_testing()
add_test (md5 md5main --tests)

IF (MSVC)
    find_package (MSVC MODULE)
    MSVC_CRT_SECURE_NO_WARNINGS (md5main)
ENDIF (MSVC)

set (CRYPTO_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR} CACHE INTERNAL "Crypto headers")
set (CRYPTO_LIBRARIES crypto CACHE INTERNAL "crypto libraries")
