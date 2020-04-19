cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

project(CMockaExample VERSION 1.0 LANGUAGES C)

include(cmake/FetchCMocka.cmake)

add_executable(CMockaExample main.c)
target_compile_features(CMockaExample PRIVATE c_std_99)
target_link_libraries(CMockaExample PRIVATE cmocka-static)

enable_testing()
add_test(NAME CMockaExample COMMAND CMockaExample)