cmake_minimum_required(VERSION 3.0.0)

set(SRC_LIBLUA
    src/lapi.c
    src/lcode.c
    src/lctype.c
    src/ldebug.c
    src/ldo.c
    src/ldump.c
    src/lfunc.c
    src/lgc.c
    src/llex.c
    src/lmem.c
    src/lobject.c
    src/lopcodes.c
    src/lparser.c
    src/lstate.c
    src/lstring.c
    src/ltable.c
    src/ltm.c
    src/lundump.c
    src/lvm.c
    src/lzio.c
    src/lauxlib.c
    src/lbaselib.c
    src/lbitlib.c
    src/lcorolib.c
    src/ldblib.c
    src/liolib.c
    src/lmathlib.c
    src/loslib.c
    src/lstrlib.c
    src/ltablib.c
    src/lutf8lib.c
    src/loadlib.c
    src/linit.c
)

set(SRC_LUA src/lua.c)
set(SRC_LUAC src/luac.c)

option(BUILD_LUA_INTERPRETER "Build lua interpreter" ON)
option(BUILD_LUA_COMPILTER "Build lua compiler" ON)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(LIBS)
if (NOT WIN32)
    find_library(MATH_LIBRARY NAMES m)
    if (MATH_LIBRARY)
        list(APPEND LIBS ${MATH_LIBRARY})
    endif ()
    find_library(DL_LIBRARY NAMES dl)
    if (DL_LIBRARY)
        list(APPEND LIBS ${DL_LIBRARY})
    endif ()
    find_package(Readline)
    if (READLINE_FOUND)
        list(APPEND LIBS ${READLINE_LIBRARY})
    endif ()
endif ()

add_library(liblua ${SRC_LIBLUA})
set_target_properties(liblua PROPERTIES PREFIX "")
target_link_libraries(liblua ${LIBS})
target_include_directories(liblua PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)

if (BUILD_LUA_INTERPRETER)
    add_executable(lua ${SRC_LUA})
    target_link_libraries(lua liblua)
endif ()
if (BUILD_LUA_COMPILTER)
    add_executable(luac ${SRC_LUAC})
    target_link_libraries(luac liblua)
endif ()
