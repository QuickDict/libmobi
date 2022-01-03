cmake_minimum_required(VERSION 3.16)

project(libmobi VERSION 0.9)

if(UNIX)
    find_package(ZLIB REQUIRED)
    find_package(LIBXML2 REQUIRED)
    include_directories(${ZLIB_INCLUDE_DIRS})
    include_directories(${LIBXML2_INCLUDE_DIRS})
    add_compile_definitions(USE_LIBXML2)
else()
    add_compile_definitions(USE_MINIZ)
endif()
add_compile_definitions(USE_ENCRYPTION)

include_directories(src)

set(LIBMOBI_SRCS
    src/buffer.c
    src/compression.c
    src/debug.c
    src/encryption.c
    src/index.c
    src/memory.c
    src/meta.c
    src/opf.c
    src/parse_rawml.c
    src/read.c
    src/sha1.c
    src/structure.c
    src/util.c
    src/write.c
    tools/common.c
)

if(UNIX)
    set(LIBMOBI_LIBS ${ZLIB_LIBRARIES} ${LIBXML2_LIBRARIES})
else()
    list(APPEND LIBMOBI_SRCS src/miniz.c src/xmlwriter.c)
endif()

add_library(mobi STATIC ${LIBMOBI_SRCS})
target_link_libraries(mobi ${LIBMOBI_LIBS})
