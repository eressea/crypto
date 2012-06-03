project (crypto C)

add_library (crypto mt19937ar.c md5.c)

add_executable (md5main md5main.c)
find_library(M_LIB m)
target_link_libraries (md5main crypto ${M_LIB})

enable_testing()
add_test (md5 md5main --tests)
