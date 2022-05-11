cmake_minimum_required(VERSION 3.5)

project(Vector LANGUAGES CXX)

set (headers
        "vector.h"
   )

add_executable(tests ${headers} test.cpp)

set_target_properties(tests PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)

if (MSVC)
    target_compile_options(tests PRIVATE /W3 /WX)
else ()
    target_compile_options(tests PRIVATE
        -Werror
        -Wall
        -Wextra
        -Wpedantic
        -Wcast-align
        -Wcast-qual
        -Wconversion
        -Wctor-dtor-privacy
        -Wenum-compare
        -Wfloat-equal
        -Wnon-virtual-dtor
        -Wold-style-cast
        -Woverloaded-virtual
        -Wredundant-decls
        -Wsign-conversion
        -Wsign-promo
        -pedantic
        -pedantic-errors
        )
endif ()
