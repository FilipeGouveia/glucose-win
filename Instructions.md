# Compile

## Using MinGW GCC on Windows


To build the makefiles, Use something like the following, changing the relative paths accordingly:
```
cmake.exe -D CMAKE_C_COMPILER="C:/msys64/mingw64/bin/gcc.exe" -D CMAKE_CXX_COMPILER="C:/msys64/mingw64/bin/g++.exe" -D ZLIB_LIBRARY="C:/msys64/mingw64/lib/libz.a" -D ZLIB_INCLUDE_DIR="C:/msys64/mingw64/include" -D CMAKE_CXX_STANDARD="11" -G "MinGW Makefiles" -S . -B build
```

check:  https://github.com/jakublevy/glucose-win for errors in windows



