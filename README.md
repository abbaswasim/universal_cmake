# Universal build CMake Template
Simplest cmake C++ project template that that you can use to build universal binaries

## Build 

`rf build && cmake -H. -Bbuild -DCMAKE_OSX_ARCHITECTURES="arm64;x86_64;x86_64h" && cmake --build build -- -j$NUMCPUS`

## Test using Lipo

`lipo build/libuniversal.1.0.0.0.dylib -detailed_info`

Results in:

```
Fat header in: build/libuniversal.1.0.0.0.dylib
fat_magic 0xcafebabe
nfat_arch 3
architecture x86_64
    cputype CPU_TYPE_X86_64
    cpusubtype CPU_SUBTYPE_X86_64_ALL
    capabilities 0x0
    offset 16384
    size 16456
    align 2^14 (16384)
architecture x86_64h
    cputype CPU_TYPE_X86_64
    cpusubtype CPU_SUBTYPE_X86_64_H
    capabilities 0x0
    offset 49152
    size 16456
    align 2^14 (16384)
architecture arm64
    cputype CPU_TYPE_ARM64
    cpusubtype CPU_SUBTYPE_ARM64_ALL
    capabilities 0x0
    offset 81920
    size 16753
    align 2^14 (16384)
```
