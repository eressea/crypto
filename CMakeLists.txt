project (crypto C)

add_library (crypto mt19937ar.c md5.c)

add_executable (md5main md5main.c)
target_link_libraries (md5main crypto)
find_library(M_LIB m)
if (EXISTS ${M_LIB})
    target_link_libraries (md5main ${M_LIB})
endif (EXISTS ${M_LIB})

enable_testing()
add_test (md5 md5main --tests)

find_package (MSVC QUIET)
if (${MSVC_FOUND})
    MSVC_CRT_SECURE_NO_WARNINGS (md5main)
endif (${MSVC_FOUND})
