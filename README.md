# luac

## Purpose
1. Make lua5.1~5.3 byte code compatible with x86, x64
2. Embedded system compatible: Android/iOS

## CMakeLists.txt
1. Download lua package from https://www.lua.org/versions.html
2. Uncompress lua-5.x.x.tar.gz
3. Copy CMakeLists.txt to root of lua-5.x.x
4. Generate project: ```cmake -Bbuild . -DLUA_VER=<lua5x>```, please specify LUA_VER for project name, such as lua54.


## Notes
lua54 official byte code is compatible both x86, x64, so no need do any patch.
