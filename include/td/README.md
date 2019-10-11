# TDLib

TDLib (Telegram Database library) is a cross-platform library for building [Telegram](https://telegram.org) clients. It can be easily used from almost any programming language.

## Table of Contents
- [Features](#features)
- [Examples and documentation](#usage)
- [Dependencies](#dependencies)
- [Building](#building)
- [Installing dependencies](#installing-dependencies)
- [Using in CMake C++ projects](#using-cxx)
- [Using in Java projects](#using-java)
- [Using in .NET projects](#using-dotnet)
- [Using with other programming languages](#using-json)
- [License](#license)

<a name="features"></a>
## Features

`TDLib` has many advantages. Notably `TDLib` is:

* **Cross-platform**: `TDLib` can be used on Android, iOS, Windows, macOS, Linux, FreeBSD, OpenBSD, NetBSD, Windows Phone, WebAssembly, watchOS, tvOS, Tizen, Cygwin. It should also work on other *nix systems with or without minimal effort.
* **Multilanguage**: `TDLib` can be easily used with any programming language that is able to execute C functions. Additionally it already has native Java (using `JNI`) bindings and .NET (using `C++/CLI` and `C++/CX`) bindings.
* **Easy to use**: `TDLib` takes care of all network implementation details, encryption and local data storage.
* **High-performance**: in the [Telegram Bot API](https://core.telegram.org/bots/api), each `TDLib` instance handles more than 24000 active bots simultaneously.
* **Well-documented**: all `TDLib` API methods and public interfaces are fully documented.
* **Consistent**: `TDLib` guarantees that all updates are delivered in the right order.
* **Reliable**: `TDLib` remains stable on slow and unreliable Internet connections.
* **Secure**: all local data is encrypted using a user-provided encryption key.
* **Fully-asynchronous**: requests to `TDLib` don't block each other or anything else, responses are sent when they are available.

<a name="usage"></a>
## Examples and documentation
Take a look at our [examples](https://github.com/tdlib/td/blob/master/example/README.md#tdlib-usage-and-build-examples).

See our [Getting Started](https://core.telegram.org/tdlib/getting-started) tutorial for a description of basic TDLib concepts.

See a [TDLib build instructions generator](https://tdlib.github.io/td/build.html) for detailed instructions on how to build TDLib.

See description of our [JSON](#using-json), [C++](#using-cxx), [Java](#using-java) and [.NET](#using-dotnet) interfaces.

See the [td_api.tl](https://github.com/tdlib/td/blob/master/td/generate/scheme/td_api.tl) scheme or the automatically generated [HTML documentation](https://core.telegram.org/tdlib/docs/td__api_8h.html)
for a list of all available `TDLib` [methods](https://core.telegram.org/tdlib/docs/classtd_1_1td__api_1_1_function.html) and [classes](https://core.telegram.org/tdlib/docs/classtd_1_1td__api_1_1_object.html).

<a name="dependencies"></a>
## Dependencies
`TDLib` depends on:

* C++14 compatible compiler (Clang 3.4+, GCC 4.9+, MSVC 19.0+ (Visual Studio 2015+), Intel C++ Compiler 17+)
* OpenSSL
* zlib
* gperf (build only)
* CMake (3.0.2+, build only)
* PHP (optional, for documentation generation)
* Doxygen (optional, for documentation generation)

<a name="building"></a>
## Building

The simplest way to build `TDLib` is to use our [TDLib build instructions generator](https://tdlib.github.io/td/build.html).
You need only to choose your programming language and target operating system to receive complete build instructions.

In general, you need to install all `TDLib` [dependencies](#dependencies) as described in [Installing dependencies](#installing-dependencies).
Then enter directory containing `TDLib` sources and compile them using CMake:

```
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
```

To build `TDLib` on low memory devices you can run [SplitSource.php](https://github.com/tdlib/td/blob/master/SplitSource.php) script
before compiling main `TDLib` source code and compile only needed targets:
```
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --target prepare_cross_compiling
cd ..
php SplitSource.php
cd build
cmake --build . --target tdjson
cmake --build . --target tdjson_static
cd ..
php SplitSource.php --undo
```
In our tests clang 6.0 with libc++ required less than 500 MB of RAM per file and GCC 4.9/6.3 used less than 1 GB of RAM per file.

<a name="installing-dependencies"></a>
### Installing dependencies

<a name="macos"></a>
#### macOS
* Install the latest Xcode command line tools, for example, via `xcode-select --install`.
* Install other [dependencies](#dependencies), for example, using [Homebrew](https://brew.sh):
```
brew install gperf cmake openssl
```
* Build `TDLib` with CMake as explained in [building](#building). You will likely need to manually specify path to the installed OpenSSL to CMake, e.g.,
```
cmake -DCMAKE_BUILD_TYPE=Release -DOPENSSL_ROOT_DIR=/usr/local/opt/openssl/ ..
```

<a name="windows"></a>
#### Windows
* Download and install Microsoft Visual Studio 2015 or later.
* Download and install [gperf](https://sourceforge.net/projects/gnuwin32/files/gperf/3.0.1/). Add the path to gperf.exe to the PATH environment variable.
* Install [vcpkg](https://github.com/Microsoft/vcpkg#quick-start).
* Run the following commands to install `TDLib` dependencies using vcpkg:
```
cd <path to vcpkg>
.\vcpkg.exe install openssl:x64-windows openssl:x86-windows zlib:x64-windows zlib:x86-windows
```
* Download and install [CMake](https://cmake.org/download/); choose "Add CMake to the system PATH" option while installing.
* Build `TDLib` with CMake as explained in [building](#building), but instead of `cmake -DCMAKE_BUILD_TYPE=Release ..` use
```
cmake -DCMAKE_TOOLCHAIN_FILE=<path to vcpkg>\scripts\buildsystems\vcpkg.cmake ..
```

To build 32-bit/64-bit `TDLib` using MSVC, you will need to additionally specify parameter `-A Win32`/`-A x64` to CMake.
To build `TDLib` in Release mode using MSVC, you will need to additionally specify parameter `--config Release` to the `cmake --build .` command.

<a name="linux"></a>
#### Linux
* Install all [dependencies](#dependencies) using your package manager.

<a name="using-cxx"></a>
## Using in CMake C++ projects
For C++ projects that use CMake, the best approach is to build `TDLib` as part of your project or to use a prebuilt installation.

There are several libraries that you could use in your CMake project:

* Td::TdJson, Td::TdJsonStatic — dynamic and static version of a JSON interface. This has a simple C interface, so it can be easily used with any programming language that is able to execute C functions.
  See [td_json_client](https://core.telegram.org/tdlib/docs/td__json__client_8h.html) and [td_log](https://core.telegram.org/tdlib/docs/td__log_8h.html) documentation for more information.
* Td::TdStatic — static library with C++ interface for general usage.
  See [Client](https://core.telegram.org/tdlib/docs/classtd_1_1_client.html) and [Log](https://core.telegram.org/tdlib/docs/classtd_1_1_log.html) documentation for more information.
* Td::TdCoreStatic — static library with low-level C++ interface intended mostly for internal usage.
  See [ClientActor](https://core.telegram.org/tdlib/docs/classtd_1_1_client_actor.html) and [Log](https://core.telegram.org/tdlib/docs/classtd_1_1_log.html) documentation for more information.

For example, part of your CMakeLists.txt may look like this:
```
add_subdirectory(td)
target_link_libraries(YourTarget PRIVATE Td::TdStatic)
```

Or you could install `TDLib` and then reference it in your CMakeLists.txt like this:
```
find_package(Td 1.5.0 REQUIRED)
target_link_libraries(YourTarget PRIVATE Td::TdStatic)
```
See [example/cpp/CMakeLists.txt](https://github.com/tdlib/td/tree/master/example/cpp/CMakeLists.txt).

<a name="using-java"></a>
## Using in Java projects
`TDLib` provides native Java interface through JNI. To enable it, specify option `-DTD_ENABLE_JNI=ON` to CMake.

See [example/java](https://github.com/tdlib/td/tree/master/example/java) for example of using `TDLib` from Java and detailed build and usage instructions.

<a name="using-dotnet"></a>
## Using in .NET projects
`TDLib` provides native .NET interface through `C++/CLI` and `C++/CX`. To enable it, specify option `-DTD_ENABLE_DOTNET=ON` to CMake.
.NET Core doesn't support `C++/CLI`, so if .NET Core is used, then `TDLib` JSON interface should be used through P/Invoke instead.

See [example/csharp](https://github.com/tdlib/td/tree/master/example/csharp) for example of using `TDLib` from C# and detailed build and usage instructions.
See [example/uwp](https://github.com/tdlib/td/tree/master/example/uwp) for example of using `TDLib` from C# UWP application and detailed build and usage instructions for Visual Studio Extension "TDLib for Universal Windows Platform".

When `TDLib` is built with `TD_ENABLE_DOTNET` option enabled, `C++` documentation is removed from some files. You need to checkout these files to return `C++` documentation back:
```
git checkout td/telegram/Client.h td/telegram/Log.h td/tl/TlObject.h
```

<a name="using-json"></a>
## Using from other programming languages
`TDLib` provides efficient native C++, Java, and .NET interfaces.
But for most use cases we suggest to use the JSON interface, which can be easily used with any programming language that is able to execute C functions.
See [td_json_client](https://core.telegram.org/tdlib/docs/td__json__client_8h.html) and [td_log](https://core.telegram.org/tdlib/docs/td__log_8h.html) documentation for detailed JSON interface description,
the [td_api.tl](https://github.com/tdlib/td/blob/master/td/generate/scheme/td_api.tl) scheme or the automatically generated [HTML documentation](https://core.telegram.org/tdlib/docs/td__api_8h.html) for a list of
all available `TDLib` [methods](https://core.telegram.org/tdlib/docs/classtd_1_1td__api_1_1_function.html) and [classes](https://core.telegram.org/tdlib/docs/classtd_1_1td__api_1_1_object.html).

`TDLib` JSON interface adheres to semantic versioning and versions with the same major version number are binary and backward compatible, but the underlying `TDLib` API can be different for different minor and even patch versions.
If you need to support different `TDLib` versions then you can use a value of the `version` option to find exact `TDLib` version and to use appropriate API then.

See [example/python/tdjson_example.py](https://github.com/tdlib/td/tree/master/example/python/tdjson_example.py) for an example of such usage.

<a name="license"></a>
## License
`TDLib` is licensed under the terms of the Boost Software License. See [LICENSE_1_0.txt](http://www.boost.org/LICENSE_1_0.txt) for more information.