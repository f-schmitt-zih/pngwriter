PNGwriter Fork
==============

This repository contains an unoffical fork from the
  http://pngwriter.sourceforge.net/
repository, Version **0.5.4**.

Since PNGwriter did not change since 2009 but contains minor
bugs / flaws, this fork tries to make our lives nicer (wuhu).

Hopefully, the changes will be merged back :)

### Install

- `git clone https://github.com/ax3l/pngwriter.git`
- `mkdir -p build && cd build`
- `cmake ../pngwriter`
- `make install` (creates the libs in `lib/` and a `pngwriter.h` in `include/`)

*Optional*, set a path where to install the libraries to:
- `cmake -DCMAKE_INSTALL_PREFIX=~/myPath/`
- `make install`
  (installs `libpngwriter.a` and `libpngwriter.so` to `~/myPath/lib/`)

### Linking to your project

You can either link manually to the `libpngwriter.a`/`libpngwriter.so` (which can have dependencies to `libpng`, `libz`, `libm`, `libc` and `libfreetype`) or use our CMake module:

```bash
wget https://raw.githubusercontent.com/ComputationalRadiationPhysics/picongpu/dev/src/cmake/FindPNGwriter.cmake
# read its documentation
cmake -DCMAKE_MODULE_PATH=. --help-module FindPNGwriter | less
```

Use the following lines in your projects `CMakeLists.txt`:
```cmake
# this example requires at least CMake 2.8.5
cmake_minimum_required(VERSION 2.8.5)

# add path to FindPNGwriter.cmake, e.g. in the directory in cmake/
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake/)

# find PNGwriter installation
#   optional: prefer static libraries over shared ones (but do not force them)
#set(PNGwriter_USE_STATIC_LIBS ON)

#   optional: specifiy (minimal) version / require to find it
#           (PNGwriter 0.5.4 REQUIRED)
find_package(PNGwriter)

if(PNGwriter_FOUND)
  # where to find the pngwriter.h header file (-I include for your compiler)
  include_directories(SYSTEM ${PNGwriter_INCLUDE_DIRS})
  # additional compiler flags (e.g. -DNO_FREETYPE)
  add_definitions(${PNGwriter_DEFINITIONS})
  # libraries to link against (including dependencies)
  set(LIBS ${LIBS} ${PNGwriter_LIBRARIES})
endif(PNGwriter_FOUND)

# add_executable(yourBinary ${SOURCES})
# ...
# target_link_libraries(yourBinary ${LIBS})
```

### License

*Paul Blackburn* (kudos!) released this piece of software under **GPLv2+**.

Please see the original [README](README) files.

### Changes

- `CMakeLists.txt`: replace that out-dated make.include stuff
- `FindPNGwriter.cmake`: CMake `find_package` module, [see above](#linking-to-your-project)
- `examples/pngtest.cc:48` and `examples/pngtest.espaniol.cc:47` fix `#include <iostream>`
- build the *static* archive **and** a *shared* library
- fixed compiler warnings for keys in `pngwriter.cc`
- fixed `filleddiamond()` bug reported in [Debian #633405](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=633405)
- fixed memory leak in `pngwriter::readfromfile` reported
  [here](http://sourceforge.net/p/pngwriter/discussion/238247/thread/15ee786c/)
