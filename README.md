# ps5-payload-sdk
This is an SDK for developing payloads targeted at exploited PS5s. ELF loaders
known to work include:
- [ps5-payload-elfldr][elfldr]
- [ps5-payload-websrv][websrv]
- [bdj-ipv6-hen][bdj-ipv6-hen]
- [elfloader][elfloader] for BD-J [ps5-jar-loader]
- [remote_lua_loader][remote_lua_loader]

Several artifacts in this repository originate from the [PS5SDK][PS5SDK] project.

## Prerequisites
On Debian-flavored operating systems, you can invoke the following commands to
install dependencies used by the SDK.
```console
john@localhost:ps5-payload-dev/sdk$ sudo apt-get update && sudo apt-get upgrade # optional
john@localhost:ps5-payload-dev/sdk$ sudo apt-get install bash clang-18 lld-18 # required
john@localhost:ps5-payload-dev/sdk$ sudo apt-get install socat cmake meson pkg-config # optional
```

If you are using Fedora, you can install dependencies as follows (tested with version 41):
```console
john@localhost:ps5-payload-dev/sdk$ sudo dnf install bash llvm-devel clang lld # required
john@localhost:ps5-payload-dev/sdk$ sudo dnf install socat cmake meson pkg-config # optional
```

If you are using macOS, you can install them using the [Homebrew Package Manager][macos-brew] (tested with macOS Sequoia):
```console
user@localhost:ps5-payload-dev/sdk$ brew install llvm@20 lld@20 socat wget cmake
user@localhost:ps5-payload-dev/sdk$ echo 'export PATH="/opt/homebrew/opt/llvm@20/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
user@localhost:ps5-payload-dev/sdk$ ln -s /opt/homebrew/opt/lld@20/bin/ld.lld /opt/homebrew/opt/llvm@20/bin/ld.lld
user@localhost:ps5-payload-dev/sdk$ ln -s /opt/homebrew/opt/llvm@20/bin/llvm-config /opt/homebrew/opt/llvm@20/bin/llvm-config-20
```

## Quick-start
To download and install a binary distribution for GNU/Linux:
```console
john@localhost:tmp$ wget https://github.com/ps5-payload-dev/sdk/releases/latest/download/ps5-payload-sdk.zip
john@localhost:tmp$ sudo unzip -d /opt ps5-payload-sdk.zip
```

## Building
```console
john@localhost:ps5-payload-dev/sdk$ make
john@localhost:ps5-payload-dev/sdk$ make DESTDIR=/opt/ps5-payload-sdk install
john@localhost:ps5-payload-dev/sdk$ export PS5_PAYLOAD_SDK=/opt/ps5-payload-sdk
john@localhost:ps5-payload-dev/sdk$ ./libcxx.sh # fetch, build, and install libcxx
```

## Usage
```console
john@localhost:ps5-payload-dev/sdk$ export PS5_PAYLOAD_SDK=/opt/ps5-payload-sdk
john@localhost:ps5-payload-dev/sdk$ make -C samples/hello_world
john@localhost:ps5-payload-dev/sdk$ export PS5_HOST=ps5; export PS5_PORT=9021
john@localhost:ps5-payload-dev/sdk$ make -C samples/hello_world test
```

## Adding new SCE Libs
If you have decrypted sprx files that you would like to interact with, you can
build stubs for them as follows:
```console
john@localhost:ps5-payload-dev/sdk$ sudo apt-get install wget python3 python3-pyelftools
john@localhost:ps5-payload-dev/sdk$ ln -s /path/to/sprx/libSceXYZ.sprx sce_stubs/libSceXYZ.sprx
john@localhost:ps5-payload-dev/sdk$ make -C sce_stubs stubs
john@localhost:ps5-payload-dev/sdk$ make DESTDIR=/opt/ps5-payload-sdk install
```

## Reporting Bugs
If you encounter problems with the SDK, please [file a github issue][issues].
If you plan on sending pull requests which affect more than a few lines of code,
please file an issue before you start to work on you changes. This will allow us
to discuss the solution properly before you commit time and effort.

## License
Files in the folder include/freebsd are licenced under BSD licences.
Unless otherwhise explicitly stated inside a file, the rest are licensed under
the GPLv3+.

[issues]: https://github.com/ps5-payload-dev/sdk/issues/new
[elfldr]: https://github.com/ps5-payload-dev/elfldr
[websrv]: https://github.com/ps5-payload-dev/websrv
[bdj-ipv6-hen]: https://github.com/ps5-payload-dev/bdj-ipv6-hen
[remote_lua_loader]: https://github.com/shahrilnet/remote_lua_loader
[elfloader]: https://github.com/cryonumb/elfloader
[ps5-jar-loader]: https://github.com/hammer-83/ps5-jar-loader
[PS5SDK]: https://github.com/PS5Dev/PS5SDK
[macos-brew]: https://brew.sh
