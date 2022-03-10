# `ublox_linux` library
A port of Sparkfun's Arduino library for u-blox GPS modules to Linux,
with new CMake config to handle installing the library (and including
it in other projects).

## Installation
1. `git clone --recursive https://github.com/huskyroboticsteam/ublox-linux`

	**NOTE**: It is important you include the `--recursive` as this
    repository contains the Sparkfun library as a submodule.
2. `cd ublox-linux`
3. `mkdir build && cd build`
4. `cmake ..`
5. `sudo cmake --build . --target install`

## Including in a project
To include the library in a CMake project, do the above installation
steps and then add
```cmake
find_package(UbloxLinux REQUIRED)
```
to your `CMakeLists.txt`. Additionally, for every executable you want
to link to the library, you will need to add
```cmake
target_link_libraries(executable_name_here UbloxLinux::ublox_linux)
```

## License
Library was originally written by @sugbuv and @StevenMedusa; CMake
config modified by Husky Robotics Team. The library is under the MIT
license; see `LICENSE` for more details.

Original Arduino library was written by SparkFun Electronics and is
also under the MIT license.
