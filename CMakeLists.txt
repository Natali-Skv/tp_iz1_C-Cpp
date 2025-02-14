cmake_minimum_required(VERSION 3.6)
project(tp_hw1)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDART_REQURED ON)

set(CMAKE_CXX_FLAGS "-g -O0 -Wall -Werror -Wextra -Wno-unused-variable -Wpedantic -fprofile-arcs -ftest-coverage -pthread")
set(CMAKE_C_FLAGS "-g -O0 -Wall -Werror -Wextra -Wno-unused-variable -Wpedantic -fprofile-arcs -ftest-coverage")
set(CMAKE_EXE_LINKER_FLAGS "-fprofile-arcs -ftest-coverage")
set(TESTS_SRC ${CMAKE_CURRENT_SOURCE_DIR}/project/tests/test_main.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/project/tests/test_load_upload.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/project/tests/test_multiply.cpp
        )
add_compile_definitions(PROJECT_PATH="${CMAKE_CURRENT_SOURCE_DIR}/project/")


add_library(matrix_lib STATIC
        project/src/matrix_lib/matrix.c
        )

target_include_directories(matrix_lib
        PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/project/include
        )

add_executable(main
        ${CMAKE_CURRENT_SOURCE_DIR}/project/cmd/main.c
        ${CMAKE_CURRENT_SOURCE_DIR}/project/src/task/task.c)

target_include_directories(main
        PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/project/include
        )

target_link_libraries(main matrix_lib)

# sets GTEST_INCLUDE_DIRS and GTEST_LIBRARIES
find_package(GTest REQUIRED)
# adds the gtest include directory
include_directories(${GTEST_INCLUDE_DIRS})

add_executable(tests ${TESTS_SRC})

# links gtest
target_link_libraries(tests ${GTEST_LIBRARIES})
target_link_libraries(tests matrix_lib)



