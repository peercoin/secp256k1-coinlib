# libsecp256k1-coinlib

![Dependencies: None](https://img.shields.io/badge/dependencies-none-success)

This library is a native C library for cryptographic functions required by
coinlib.

It is a fork of [libsecp256k1](https://github.com/bitcoin-core/secp256k1) with
additional adaptor signature support for MuSig2 signatures taken from
[libsecp256k1-zkp](https://github.com/BlockstreamResearch/secp256k1-zkp).

Please see the [upstream libsecp256k1
library](https://github.com/bitcoin-core/secp256k1) for more details.

## Building

It is suggested to build using CMake but autotools is available.

### Building with CMake

To maintain a pristine source tree, CMake encourages to perform an out-of-source
build by using a separate dedicated build tree.

#### Building on POSIX systems

    $ cmake -B build              # Generate a build system in subdirectory "build"
    $ cmake --build build         # Run the actual build process
    $ ctest --test-dir build      # Run the test suite
    $ sudo cmake --install build  # Install the library into the system (optional)

To compile optional modules (such as Schnorr signatures), you need to run
`cmake` with additional flags (such as
`-DSECP256K1_ENABLE_MODULE_SCHNORRSIG=ON`). Run `cmake -B build -LH` or `ccmake
-B build` to see the full list of available flags.

#### Cross compiling

To alleviate issues with cross compiling, preconfigured toolchain files are
available in the `cmake` directory. For example, to cross compile for Windows:

    $ cmake -B build -DCMAKE_TOOLCHAIN_FILE=cmake/x86_64-w64-mingw32.toolchain.cmake

To cross compile for Android with
[NDK](https://developer.android.com/ndk/guides/cmake) (using NDK's toolchain
file, and assuming the `ANDROID_NDK_ROOT` environment variable has been set):

    $ cmake -B build -DCMAKE_TOOLCHAIN_FILE="${ANDROID_NDK_ROOT}/build/cmake/android.toolchain.cmake" -DANDROID_ABI=arm64-v8a -DANDROID_PLATFORM=28

#### Building on Windows

To build on Windows with Visual Studio, a proper
[generator](https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html#visual-studio-generators)
must be specified for a new build tree.

The following example assumes using of Visual Studio 2022 and CMake v3.21+.

In "Developer Command Prompt for VS 2022":

    >cmake -G "Visual Studio 17 2022" -A x64 -B build
    >cmake --build build --config RelWithDebInfo

### Building with Autotools

    $ ./autogen.sh       # Generate a ./configure script
    $ ./configure        # Generate a build system
    $ make               # Run the actual build process
    $ make check         # Run the test suite
    $ sudo make install  # Install the library into the system (optional)

To compile optional modules (such as Schnorr signatures), you need to run
`./configure` with additional flags (such as `--enable-module-schnorrsig`). Run
`./configure --help` to see the full list of available flags.

## Usage examples

Usage examples can be found in the [examples](examples) directory. To compile
them you need to configure with `-DSECP256K1_BUILD_EXAMPLES=ON` for CMake or
`--enable-examples` for autotools.
  * [ECDSA example](examples/ecdsa.c)
  * [Schnorr signatures example](examples/schnorr.c)
  * [Deriving a shared secret (ECDH) example](examples/ecdh.c)
  * [ElligatorSwift key exchange example](examples/ellswift.c)
  * [MuSig2 Schnorr multi-signatures example](examples/musig.c)

To compile the examples, make sure the corresponding modules are enabled.

## Benchmark

If configured with `--enable-benchmark` (which is the default), binaries for
benchmarking the libsecp256k1 functions will be present in the root directory
after the build.

To print the benchmark result to the command line:

    $ ./bench_name

To create a CSV file for the benchmark result :

    $ ./bench_name | sed '2d;s/ \{1,\}//g' > bench_name.csv
