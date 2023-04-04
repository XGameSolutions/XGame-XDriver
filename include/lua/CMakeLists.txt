project ( lua C )
cmake_minimum_required ( VERSION 2.8 )

include_directories ( src ${CMAKE_CURRENT_BINARY_DIR} )
set ( SRC_CORE lapi.c lcode.c lctype.c ldebug.c ldo.c ldump.c lfunc.c lgc.c llex.c
  lmem.c lobject.c lopcodes.c lparser.c lstate.c lstring.c ltable.c
  ltm.c lundump.c lvm.c lzio.c )
set ( SRC_LIB lauxlib.c lbaselib.c lbitlib.c lcorolib.c ldblib.c liolib.c
  lmathlib.c loslib.c lstrlib.c ltablib.c lutf8lib.c loadlib.c linit.c )

set ( SRC_LUA lua.c )
set ( SRC_LUAC luac.c )

add_library ( liblua ${SRC_CORE} ${SRC_LIB} )
set_target_properties ( liblua PROPERTIES OUTPUT_NAME lua )

add_library ( libluadll SHARED ${SRC_CORE} ${SRC_LIB} )
target_compile_definitions ( libluadll PRIVATE _CRT_SECURE_NO_WARNINGS LUA_BUILD_AS_DLL )
set_target_properties ( libluadll PROPERTIES OUTPUT_NAME lua53 )

add_executable ( lua ${SRC_LUA} )
target_link_libraries ( lua libluadll )

add_executable ( luac ${SRC_LUAC} )
target_link_libraries ( luac liblua )