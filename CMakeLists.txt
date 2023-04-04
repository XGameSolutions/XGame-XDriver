cmake_minimum_required(VERSION 2.8)

project(XDriver)

set(LUA ${PROJECT_SOURCE_DIR}/include/lua)
set(LIBUV ${PROJECT_SOURCE_DIR}/include/libuv)
set(RECASTNAVIGATION ${PROJECT_SOURCE_DIR}/include/recastnavigation)

set(CMAKE_BUILD_TYPE Debug)
# set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build)
# set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_subdirectory(${LIBUV})
add_subdirectory(${RECASTNAVIGATION})
#add_subdirectory(${LUA})

set(ROOT_PATH ${PROJECT_SOURCE_DIR})
set(DRIVER_INC_PATH ${ROOT_PATH}/src)
set(DRIVER_SRC_PATH ${ROOT_PATH}/src)

set(LIBUV_INC_PATH ${LIBUV}/include)
set(LIBUV_SRC_PATH ${LIBUV}/src)

set(LUA_INC_PATH ${LUA})
set(LUA_SRC_PATH ${LUA})

set(LUA_SRC_FILES 
    ${LUA_SRC_PATH}/lapi.c
    ${LUA_SRC_PATH}/lauxlib.c
    ${LUA_SRC_PATH}/lbaselib.c
    ${LUA_SRC_PATH}/lbitlib.c
    ${LUA_SRC_PATH}/lcode.c
    ${LUA_SRC_PATH}/lcorolib.c
    ${LUA_SRC_PATH}/lctype.c
    ${LUA_SRC_PATH}/ldblib.c
    ${LUA_SRC_PATH}/ldebug.c
    ${LUA_SRC_PATH}/ldo.c
    ${LUA_SRC_PATH}/ldump.c
    ${LUA_SRC_PATH}/lfunc.c
    ${LUA_SRC_PATH}/lgc.c
    ${LUA_SRC_PATH}/linit.c
    ${LUA_SRC_PATH}/liolib.c
    ${LUA_SRC_PATH}/llex.c
    ${LUA_SRC_PATH}/lmathlib.c
    ${LUA_SRC_PATH}/lmem.c
    ${LUA_SRC_PATH}/loadlib.c
    ${LUA_SRC_PATH}/lobject.c
    ${LUA_SRC_PATH}/lopcodes.c
    ${LUA_SRC_PATH}/loslib.c
    ${LUA_SRC_PATH}/lparser.c
    ${LUA_SRC_PATH}/lstate.c
    ${LUA_SRC_PATH}/lstring.c
    ${LUA_SRC_PATH}/ltable.c
    ${LUA_SRC_PATH}/ltablib.c
    ${LUA_SRC_PATH}/ltm.c
    ${LUA_SRC_PATH}/lua.c
    ${LUA_SRC_PATH}/luac.c
    ${LUA_SRC_PATH}/lundump.c
    ${LUA_SRC_PATH}/lutf8lib.c
    ${LUA_SRC_PATH}/lvm.c
    ${LUA_SRC_PATH}/lzio.c
)

set(DRIVER_SRC_FILES
    ${DRIVER_SRC_PATH}/c2lua.cpp
    ${DRIVER_SRC_PATH}/driver.cpp
    ${DRIVER_SRC_PATH}/lua2c.cpp
    ${DRIVER_SRC_PATH}/main.cpp
    ${DRIVER_SRC_PATH}/nav.cpp
    ${DRIVER_SRC_PATH}/navext.cpp
    ${DRIVER_SRC_PATH}/proto.cpp
    ${DRIVER_SRC_PATH}/socket.cpp
    ${DRIVER_SRC_PATH}/test.cpp
    ${DRIVER_SRC_PATH}/tools.cpp
    ${RECASTNAVIGATION}/RecastDemo/Source/ChunkyTriMesh.cpp
    ${RECASTNAVIGATION}/RecastDemo/Source/InputGeom.cpp
    ${RECASTNAVIGATION}/RecastDemo/Source/MeshLoaderObj.cpp
    ${RECASTNAVIGATION}/RecastDemo/Source/PerfTimer.cpp
)

include_directories(
    ${DRIVER_INC_PATH}
    ${LIBUV_INC_PATH}
    ${LUA_INC_PATH}
    ${RECASTNAVIGATION}/DebugUtils/include
    ${RECASTNAVIGATION}/Detour/include
    ${RECASTNAVIGATION}/DetourCrowd/include
    ${RECASTNAVIGATION}/DetourTileCached/include
    ${RECASTNAVIGATION}/Recast/include
    ${RECASTNAVIGATION}/RecastDemo/include
)

add_library(lua STATIC ${LUA_SRC_FILES})

if(WIN32 AND NOT CYGWIN)
    target_compile_definitions(lua PRIVATE LUA_BUILD_AS_DLL)
elseif(APPLE)
    target_compile_definitions(lua PRIVATE LUA_USE_MACOSX)
    #target_link_libraries(lua -Wl, -E dl readline)
    #target_link_libraries(lua m)
endif()


#link_directories(${ROOT_PATH}/../build)
link_directories(${ROOT_PATH}/../build/include/recastnavigation/DebugUtils)
link_directories(${ROOT_PATH}/../build/include/recastnavigation/Detour)
link_directories(${ROOT_PATH}/../build/include/recastnavigation/DetourCrowd)
link_directories(${ROOT_PATH}/../build/include/recastnavigation/DetourTileCache)
link_directories(${ROOT_PATH}/../build/include/recastnavigation/Recast)

link_libraries(uv_a)
link_libraries(DebugUtils)
link_libraries(Detour)
link_libraries(DetourCrowd)
link_libraries(DetourTileCache)
link_libraries(Recast)


add_executable(driver ${DRIVER_SRC_PATH}/main.cpp ${DRIVER_SRC_FILES})
target_link_libraries(driver lua uv_a DebugUtils Detour Recast)
#target_link_libraries(driver lua m uv_a)