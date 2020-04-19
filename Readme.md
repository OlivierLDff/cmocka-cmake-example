# CMocka with CMake example

A sample project using [CMocka](https://cmocka.org/) with [CMake](https://cmake.org/). This project demonstrate the use of [FetchContent](https://cmake.org/cmake/help/latest/module/FetchContent.html) with `CMake 3.14` to fetch and build cmocka as part of a cmake c project.

## Build and execute

```bash
git clone https://github.com/OlivierLDff/cmocka-cmake-example
cd cmocka-cmake-example
mkdir build && cd build
cmake ..
cmake --build . --config Release
ctest -C "Release"
```

## Fetch CMocka

All the magic happen inside `cmake/FetchCMocka.cmake`.

```cmake
include(FetchContent)

# Declare our target. We want the lastest stable version, not the master.
# Also specify GIT_SHALLOW to avoid cloning branch we don't care about
FetchContent_Declare(
  cmocka
  GIT_REPOSITORY https://git.cryptomilk.org/projects/cmocka.git
  GIT_TAG        cmocka-1.1.5
  GIT_SHALLOW    1
)

# We want to link to cmocka-static, so we need to set this option before calling the FetchContent_MakeAvailable
# We also don't care about example and tests
set(WITH_STATIC_LIB ON CACHE BOOL "CMocka: Build with a static library" FORCE)
set(WITH_CMOCKERY_SUPPORT OFF CACHE BOOL "CMocka: Install a cmockery header" FORCE)
set(WITH_EXAMPLES OFF CACHE BOOL "CMocka: Build examples" FORCE)
set(UNIT_TESTING OFF CACHE BOOL "CMocka: Build with unit testing" FORCE)
set(PICKY_DEVELOPER OFF CACHE BOOL "CMocka: Build with picky developer flags" FORCE)

# Download cmocka, and execute its cmakelists.txt
FetchContent_MakeAvailable(cmocka)
```

And then in your `CMakelists.txt`, simply call `include(cmake/FetchCMocka.cmake)`.

Then the target `cmocka-static` will be available for 

`target_link_libraries(<target> PRIVATE cmocka-static)`.