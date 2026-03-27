# GarminFIT

Provide the necessary files to compile and install the [GarminFIT](https://github.com/garmin/fit-cpp-sdk) c++ API in Arch Linux. The documentation of the GarminFIT SDK can be found [here](https://developer.garmin.com/fit/overview/).

## Install
Download the [PKGBUILD](https://raw.githubusercontent.com/petrus82/GarminFit/refs/heads/master/PKGBUILD) into a directory and execute makepkg -si inside this directory.

## Howto

Create a CMakeLists.txt which contains 
``` find_package(GarminFit Required)
    target_link_libraries(your_project_name PRIVATE
        Garmin::GarminFit
    )
```