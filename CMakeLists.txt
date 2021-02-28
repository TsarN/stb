cmake_minimum_required(VERSION 3.1)
project("stb"
            DESCRIPTION "stb single-file public domain libraries for C/C++"
            HOMEPAGE_URL "https://github.com/nothings/stb"
            LANGUAGES C
)

cmake_policy(SET CMP0054 NEW)

set(STB_LIBS "")
set(STB_DEFINES "")

list(APPEND STB_LIBS "stb_c_lexer")
list(APPEND STB_DEFINES "STB_C_LEXER_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_divide")
list(APPEND STB_DEFINES "STB_DIVIDE_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_ds")
list(APPEND STB_DEFINES "STB_DS_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_dxt")
list(APPEND STB_DEFINES "STB_DXT_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_easy_font")
list(APPEND STB_DEFINES "")

list(APPEND STB_LIBS "stb")
list(APPEND STB_DEFINES "STB_DEFINE")

list(APPEND STB_LIBS "stb_herringbone_wang_tile")
list(APPEND STB_DEFINES "STB_HERRINGBONE_WANG_TILE_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_image")
list(APPEND STB_DEFINES "STB_IMAGE_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_image_resize")
list(APPEND STB_DEFINES "STB_IMAGE_RESIZE_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_image_write")
list(APPEND STB_DEFINES "STB_IMAGE_WRITE_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_include")
list(APPEND STB_DEFINES "STB_INCLUDE_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_leakcheck")
list(APPEND STB_DEFINES "STB_LEAKCHECK_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_perlin")
list(APPEND STB_DEFINES "STB_PERLIN_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_rect_pack")
list(APPEND STB_DEFINES "STB_RECT_PACK_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_sprintf")
list(APPEND STB_DEFINES "STB_SPRINTF_IMPLEMENTATION")

list(APPEND STB_LIBS "stb_truetype")
list(APPEND STB_DEFINES "STB_TRUETYPE_IMPLEMENTATION")

list(APPEND STB_LIBS "stretchy_buffer")
list(APPEND STB_DEFINES "")

list(LENGTH STB_LIBS STB_LIBS_LEN)
math(EXPR STB_LIBS_LEN_MINUS_ONE "${STB_LIBS_LEN} - 1")

foreach(i RANGE 0 ${STB_LIBS_LEN_MINUS_ONE})
    list(GET STB_LIBS ${i} STB_LIB)
    list(GET STB_DEFINES ${i} STB_DEFINE)

    set(STB_FILE "${CMAKE_CURRENT_SOURCE_DIR}/${STB_LIB}.h")

    if ("${STB_DEFINE}" STREQUAL "")
        add_library("${STB_LIB}" INTERFACE)
    else()
        set(STB_IMPL_FILE "${CMAKE_CURRENT_BINARY_DIR}/${STB_LIB}.c")
        configure_file("${STB_FILE}" "${STB_IMPL_FILE}" COPYONLY)
        add_library("${STB_LIB}" STATIC "${STB_IMPL_FILE}")
        target_compile_definitions("${STB_LIB}" PRIVATE "${STB_DEFINE}")
    endif()

    target_include_directories("${STB_LIB}" INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")
    add_library(stb::${STB_LIB} ALIAS "${STB_LIB}")
endforeach()
