---
type: assignment
date: 2021-08-23T8:00:00+4:30
title: 'Lab #0 - Setup'
due_event: 
    type: due
    date: 2021-08-25T23:59:00+3:30
    description: 'Lab #0 due'
---
You'll use two sets of tools in this class: an x86 emulator: [Qemu](#configuring-qemu) for running your kernel; and 
a [Compiler toolchain](#compiler-tool-chain), including assembler, linker, C compiler, and
debugger, for compiling and testing your kernel.

## Compiler Tool chain
A "compiler toolchain" is the set of programs, including a C compiler, assemblers, and linkers, that turn code into 
executable binaries. You'll need a compiler toolchain that generates code for 32-bit Intel architectures ("x86" architectures) in the ELF binary format.

### Test Your Compiler Toolchain
Modern Linux and BSD UNIX distributions already provide a toolchain suitable for 6.828. To test your distribution, try the following commands:

```
% objdump -i
```

If you don't have `objdump`, then install `binutils`:

```
sudo apt-get install binutils
```
The second line should say elf32-i386.

```
% gcc -m32 -print-libgcc-file-name
```

The command should print something like `/usr/lib/gcc/i486-linux-gnu/version/libgcc.a` or `/usr/lib/gcc/x86_64-linux-gnu/version/32/libgcc.a`

If both these commands succeed, you're all set, and don't need to compile your own toolchain.

If the gcc command fails, you may need to install a development environment. On Ubuntu Linux, try this:

```
% sudo apt-get install -y build-essential gdb
```

On 64-bit machines (most likely it is), you may need to install a 32-bit support library. The symptom is that linking fails with 
error messages like `"__udivdi3 not found"` and `"__muldi3 not found"`. 
On Ubuntu Linux, try this to fix the problem:

```
% sudo apt-get install gcc-multilib
```

### For MacOS

**Note: We strongly suggest using a Ubuntu 20.04 machine. If you don't have one, you could use a virtual machine.**

Begin by installing developer tools:
```
xcode-select --install
```
## Configuring Qemu

### Install dependencies:

On Ubuntu:

```
sudo apt-get install libfdt-dev libsdl1.2-dev libtool-bin libglib2.0-dev libz-dev libpixman-1-dev python git
```

On MacOS:

```
brew install $(brew deps qemu)
```

Clone QEMU:

```
git clone https://github.com/EE469/ee469-qemu.git ~/qemu
```

### Running Configure:

```
cd ~/qemu
mkdir build
```

On Ubuntu:

```
./configure --disable-kvm --disable-werror --prefix=~/qemu/build --target-list="i386-softmmu x86_64-softmmu"
```

On MacOSX:

```
./configure --disable-kvm --disable-werror --disable-sdl --prefix=~/qemu/build --target-list="i386-softmmu x86_64-softmmu"
```

### Changing your path

Update your path and add `~/qemu/build/bin`.

Open your `~/.bashrc` file and add the following line at the end of it.

```
PATH=~/qemu/build/bin:$PATH
```