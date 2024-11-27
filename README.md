# Overview

This is a C/C++ project template that uses `cmake` and `vcpkg` to manage build and dependencies. Fork this repo to bootstrap your project.

# VCPKG

There are 3 ways to use the `vcpkg` program with this project template:

## 1.1 SDK

`vcpkg` supports using a set of previously built and exported dependency libraries without actually installing the program itself. This is useful for sharing prebuilt dependencies during development to avoid building everything from source, which may become time consuming as the number of dependencies grow.

To do this, you will first need to obtain an export of dependent libraries via steps described in [the official docs](https://learn.microsoft.com/en-us/vcpkg/commands/export). This will produce a package of the libraries in a format that you specified.

Then, extract the generated export into the filesystem, and provide the extracted path to `cmake` via the variable `EXPORT_SDK_PATH`:

```bash
cmake -B build -D EXPORT_SDK_PATH=<extracted/path> --preset default
```

## 1.2 Install `vcpkg` on system

If you have installed `vcpkg` on your system and properly configured the `VCPKG_ROOT` environment variable, as described in [the installation guide](https://learn.microsoft.com/en-us/vcpkg/get_started/get-started?pivots=shell-bash), this project can then use the system `vcpkg` in [manifest mode](https://learn.microsoft.com/en-us/vcpkg/concepts/manifest-mode).

The `vcpkg.json` and `vcpkg-configuration.json` is automatically generated (and should be added to version control) when you invoke `cmake` for the first time:

```bash
cmake -B build --preset default
```

Refer to [official docs](https://learn.microsoft.com/en-us/vcpkg/) on how to manage dependencies with `vcpkg`.

## 1.3 Use `vcpkg` as build dependency

If `vcpkg` is not found on the system as described above, this project will automatically fetch `vcpkg` from the [official repo](https://github.com/microsoft/vcpkg) as a build dependency via cmake's `FetchContent` mechanic. The rest is the same as above, except that the `vcpkg` binary will be available in the build directory once the project has been configured via cmake:

```bash
cmake -B build --preset default
./build/vcpkg add fmt spdlog # add any package you want
./build/vcpkg install
cmake --build build -- -j $(nproc)
```
