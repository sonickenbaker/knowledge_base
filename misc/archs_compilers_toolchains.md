# Architectures, compilers and Toolchains

## Compiler flags
https://www.gnu.org/software/make/manual/make.html

- CFLAGS: Extra flags to give to the C compiler.
- CXXFLAGS: Extra flags to give to the C++ compiler.
- CPPFLAGS: Extra flags to give to the C preprocessor and programs that use it (the C and Fortran compilers).

### Compiling C programs
`$(CC) $(CPPFLAGS) $(CFLAGS) -c`

### Compiling C++ programs
`$(CXX) $(CPPFLAGS) $(CXXFLAGS) -c`

## Linker

- LDFLAGS: Extra flags to give to compilers when they are supposed to invoke the linker, ‘ld’, such as -L. Libraries (-lfoo) should be added to the LDLIBS variable instead.
- LDLIBS: Library flags or names given to compilers when they are supposed to invoke the linker, ‘ld’. LOADLIBES is a deprecated (but still supported) alternative to LDLIBS. Non-library linker flags, such as -L, should go in the LDFLAGS variable. 
- `-rpath-link` and `-rpath`
  - Are `ld` options (https://stackoverflow.com/questions/49138195/whats-the-difference-between-rpath-link-and-l)
  - `-rpath`: Set runtime shared library search path.
    - That info is stored into the ELF info of the compiled object and is used to search for libraries. `LD_LIBRARY_PATH` is used first, then the `rpath` and last the system defaults.
  - `-rpath-link`: Set link time shared library search path
- `-Wl,option`: Pass option as an option to the linker. If option contains commas, it is split into multiple options at the commas. You can use this syntax to pass an argument to the option. For example, -Wl,-Map,output.map passes -Map output.map to the linker. When using the GNU linker, you can also get the same effect with -Wl,-Map=output.map. 


### Linking programs
`$(CC) $(LDFLAGS) n.o $(LOADLIBES) $(LDLIBS)`

`n.o` is the name of an object file

## Toolchain naming convention
`arch-vendor-(os-)abi`
Examples:
  - arm-none-linux-gnueabi
    - ARM architecture
    - no vendor
    - linux OS
    - gnueabi ABI.

ABI: Application Binary Interface


## Meaning of "eabi" and "eabihf"
It is just a matter of default parameters.
- "hf" stands for "hard float" (floating-point arguments are passed in FPU registers instead of general-purpose registers, as in "soft float" mode)
- Using an "eabi" you can set "hard float" mode by using "-mfloat-abi=hard" and using an "eabihf" you can set "soft float" mode by using "-mfloat-abi=softfp"

## Check FPU presence (from armv4 to armv7)
`cat /proc/cpuinfo`
and check for "vfp" is present in "Features" line
Complete answer here: https://unix.stackexchange.com/questions/184874/how-do-determine-whether-linux-board-is-using-hardware-fpu-or-not

## CPU info and compile options
Some examples on how to set-up compile options based on the cpu info.

BeagleBone Black:
```
$ cat /proc/cpuinfo
model name  : ARMv7 Processor rev 2 (v7l)
Features    : half thumb fastmult vfp edsp thumbee neon vfpv3 tls vfpd32 

# compile options
-march=armv7-a -mtune=cortex-a8 -mfpu=neon -mfloat-abi=hard
```

Cubie Truck v5:
```
$ cat /proc/cpuinfo
Processor   : ARMv7 Processor rev 5 (v7l)
Features    : swp half thumb fastmult vfp edsp thumbee neon vfpv3 tls vfpv4 

# compile options
-march=armv7-a -mtune=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard
```

Banana Pi Pro:
```
$ cat /proc/cpuinfo
Processor   : ARMv7 Processor rev 4 (v7l)
Features    : swp half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt

# compile options
-march=armv7-a -mtune=cortex-a7 -mfpu=neon-vfpv4 -mfloat-abi=hard
```

Raspberry Pi 3:
```
$ cat /proc/cpuinfo 
model name  : ARMv7 Processor rev 4 (v7l)
Features    : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32

# compile options
-march=armv8-a+crc -mtune=cortex-a53 -mfpu=neon-fp-armv8 -mfloat-abi=hard
# or
-march=armv7-a -mfpu=neon-vfpv4 -mfloat-abi=hard 
```

Odroid C2:
```
$ cat /proc/cpuinfo 
Features    : fp asimd evtstrm crc32

# compile options
-march=armv8-a+crc -mtune=cortex-a53
```
