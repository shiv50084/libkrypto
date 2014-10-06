project(LIBKRYPTO C)

cmake_minimum_required(VERSION 2.6)

set(CMAKE_COLOR_MAKEFILE ON)
set(CMAKE_VERBOSE_MAKEFILE ON)

set(CMAKE_BUILD_TYPE Debug)

if (CMAKE_BUILD_TYPE MATCHES "Debug")
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DDEBUG")
endif (CMAKE_BUILD_TYPE MATCHES "Debug")

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

set(CMAKE_C_FLAGS "-D_BSD_SOURCE -std=c99 -Wall -W -O2")


if (DEFINED LIBKRYPTO_SHARED)
    option(LIBKRYPTO_SHARED "Build shared lib" ${LIBKRYPTO_SHARED})
else()
    option(LIBKRYPTO_SHARED "Build shared lib" ON)
endif()

if (DEFINED LIBKRYPTO_STATIC)
    option(LIBKRYPTO_STATIC "Build static lib" ${LIBKRYPTO_STATIC})
else()
    option(LIBKRYPTO_STATIC "Build static lib" ON)
endif()

# Check header files
include(CheckIncludeFiles)

# Check if sys/select.h exists
check_include_files("sys/select.h" HAVE_SYS_SELECT_H)
if (HAVE_SYS_SELECT_H)
    message("Use of ${CMAKE_SOURCE_DIR}/src/select/select.h2")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/select/select.h"
		  INPUT "${CMAKE_SOURCE_DIR}/src/select/select.h2")
else(HAVE_SYS_SELECT_H)
    message("Use of ${CMAKE_SOURCE_DIR}/src/select/select.h1")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/select/select.h"
		  INPUT "${CMAKE_SOURCE_DIR}/src/select/select.h1")
endif(HAVE_SYS_SELECT_H)

# Check if poll.h exists
check_include_files("poll.h" HAVE_POLL_H)
if (HAVE_POLL_H)
    message("Use of ${CMAKE_SOURCE_DIR}/src/iopause/iopause.h2")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h"
		  INPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h2")
else()
    message("Use of ${CMAKE_SOURCE_DIR}/src/iopause/iopause.h1")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h"
		  INPUT "${CMAKE_SOURCE_DIR}/src/iopause/iopause.h1")
endif()

# Check type sizes
include(CheckTypeSize)

# Check unsigned long size
check_type_size("unsigned long" ULONG_SIZE)
message("Size of unsigned long is ${ULONG_SIZE}")
if (ULONG_SIZE MATCHES "8")
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h"
		  INPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h2")
else()
    file(GENERATE OUTPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h"
		  INPUT "${CMAKE_SOURCE_DIR}/src/uint32/uint32.h1")
endif()


include_directories(src/buffer)
include_directories(src/byte)
include_directories(src/error)
include_directories(src/iopause)
include_directories(src/open)
include_directories(src/select)
include_directories(src/socket)
include_directories(src/str)
include_directories(src/stralloc)
include_directories(src/strerr)
include_directories(src/tai)
include_directories(src/taia)
include_directories(src/uint16)
include_directories(src/uint32)
include_directories(src/uint64)

set(buffer_SRCS
    src/buffer/buffer.h
    src/buffer/buffer.c
    src/buffer/buffer_1.c
    src/buffer/buffer_2.c
    src/buffer/buffer_copy.c
    src/buffer/buffer_get.c
    src/buffer/buffer_put.c
    src/buffer/buffer_read.c
    src/buffer/buffer_write.c
    )

set(byte_SRCS
    src/byte/byte.h
    src/byte/byte_chr.c
    src/byte/byte_copy.c
    src/byte/byte_cr.c
    src/byte/byte_diff.c
    src/byte/byte_rchr.c
    src/byte/byte_zero.c
    )

set(error_SRCS
    src/error/error.h
    src/error/error.c
    src/error/error_str.c
    src/error/error_temp.c
    )

set(iopause_SRCS
    src/iopause/iopause.h1
    src/iopause/iopause.h2
    src/iopause/iopause.c
    )

set(open_SRCS
    src/open/open.h
    src/open/open_read.c
    src/open/open_trunc.c
    src/open/openreadclose.h
    src/open/openreadclose.c
    )

set(socket_SRCS
    src/socket/socket.h
    src/socket/socket_accept.c
    src/socket/socket_bind.c
    src/socket/socket_conn.c
    src/socket/socket_listen.c
    src/socket/socket_recv.c
    src/socket/socket_send.c
    src/socket/socket_tcp.c
    src/socket/socket_udp.c
    )

set(str_SRCS
    src/str/str.h
    src/str/str_chr.c
    src/str/str_cpy.c
    src/str/str_diff.c
    src/str/str_diffn.c
    src/str/str_len.c
    src/str/str_rchr.c
    src/str/str_start.c
    )

set(stralloc_SRCS
    src/stralloc/stralloc.h
    src/stralloc/stralloc_cat.c
    src/stralloc/stralloc_catb.c
    src/stralloc/stralloc_cats.c
    src/stralloc/stralloc_copy.c
    src/stralloc/stralloc_eady.c
    src/stralloc/stralloc_num.c
    src/stralloc/stralloc_opyb.c
    src/stralloc/stralloc_opys.c
    src/stralloc/stralloc_pend.c
    )

set(strerr_SRCS
    src/strerr/strerr.h
    src/strerr/strerr_die.c
    src/strerr/strerr_sys.c
    )

set(tai_SRCS
    src/tai/tai.h
    src/tai/tai_add.c
    src/tai/tai_now.c
    src/tai/tai_pack.c
    src/tai/tai_sub.c
    src/tai/tai_uint.c
    src/tai/tai_unpack.c
    )

set(taia_SRCS
    src/taia/taia.h
    src/taia/taia_add.c
    src/taia/taia_approx.c
    src/taia/taia_frac.c
    src/taia/taia_less.c
    src/taia/taia_now.c
    src/taia/taia_pack.c
    src/taia/taia_sub.c
    src/taia/taia_tai.c
    src/taia/taia_uint.c
    )

set(uint16_SRCS
    src/uint16/uint16.h
    src/uint16/uint16_pack.c
    src/uint16/uint16_unpack.c
    )

set(uint32_SRCS
    src/uint32/uint32.h1
    src/uint32/uint32.h2
    src/uint32/uint32_pack.c
    src/uint32/uint32_unpack.c
    )


set(LIB_TARGET libkr1pt0)
set(LIB_SRC
    ${buffer_SRCS}
    ${byte_SRCS}
    ${error_SRCS}
    ${iopause_SRCS}
    ${open_SRCS}
    ${socket_SRCS}
    ${str_SRCS}
    ${stralloc_SRCS}
    ${strerr_SRCS}
    ${tai_SRCS}
    ${taia_SRCS}
    ${uint16_SRCS}
    ${uint32_SRCS}
    )


# Shared library
if (LIBKRYPTO_SHARED)
    if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
        set(CMAKE_SHARED_LIBRARY_SUFFIX ".so")
        set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -export-dynamic")
    endif()
    if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
        set(CMAKE_SHARED_LIBRARY_SUFFIX ".dylib")
        set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -flat_namespace -single_module -undefined dynamic_lookup")
    endif()
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/lib")
    add_library(${LIB_TARGET} SHARED ${LIB_SRC})
endif()

#Static library
if (LIBKRYPTO_STATIC)
    set(LIB_STATIC_TARGET "${LIB_TARGET}-static")
    set(CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_STATIC_LIBRARY_SUFFIX})
    set(CMAKE_STATIC_LIBRARY_SUFFIX ".a")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_SOURCE_DIR}/lib")
    add_library(${LIB_STATIC_TARGET} STATIC ${LIB_SRC})
    set_target_properties(${LIB_STATIC_TARGET} PROPERTIES OUTPUT_NAME ${LIB_TARGET})
endif()

link_directories(${PROJECT_SOURCE_DIR}/lib)

