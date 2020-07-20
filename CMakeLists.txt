cmake_minimum_required(VERSION 3.10)

if ( WIN32 AND NOT CYGWIN AND NOT ( CMAKE_SYSTEM_NAME STREQUAL "WindowsStore" ) AND NOT ANDROID)
	set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /MD /O2" CACHE STRING "")
	set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /MDd /Od /Zi" CACHE STRING "")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MD /O2 /Bv" CACHE STRING "")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MDd /Od /Zi /Bv" CACHE STRING "")
endif ()

if(NOT DEFINED LUA_VER)
set(LUA_VER lua53)
endif()

project(${LUA_VER})

set (LUA_SRC_PATH ${CMAKE_CURRENT_SOURCE_DIR}/src)
file(GLOB LUA_CORE_HEADERS "${LUA_SRC_PATH}/*.h")
file(GLOB LUA_CORE_FILES "${LUA_SRC_PATH}/*.c")
list(REMOVE_ITEM LUA_CORE_FILES "${LUA_SRC_PATH}/lua.c")
list(REMOVE_ITEM LUA_CORE_FILES "${LUA_SRC_PATH}/luac.c")
    
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

# Lua core
if(MSVC)
    add_library(${LUA_VER} SHARED ${LUA_CORE_HEADERS};${LUA_CORE_FILES})
    add_definitions(-DLUA_BUILD_AS_DLL)
else()
    add_library(${LUA_VER} STATIC ${LUA_CORE_HEADERS};${LUA_CORE_FILES})
endif()

# Lua byte code compiler
add_executable(luac "${LUA_SRC_PATH}/luac.c"
                    ${LUA_CORE_FILES})

# Lua interpreter
add_executable(lua "${LUA_SRC_PATH}/lua.c")
target_link_libraries(lua ${LUA_VER})


