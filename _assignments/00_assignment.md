---
type: assignment
date: 2022-01-11T8:00:00+4:30
enable: yes
title: 'Lab #0 - Setup'
reference: "
 **Reference:**
 - [at&t_asm](https://www.cs.yale.edu/flint/cs421/papers/x86-asm/asm.html) 
 - [GDB tutorial1](http://www.cs.toronto.edu/~krueger/csc209h/tut/gdb_tutorial.html) 
 - [GDB tutorial2](http://beej.us/guide/bggdb/) 
 - [GDB chect-sheet](https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf)
"
due_event: 
    type: due
    date: 2022-01-11T23:59:00+3:30
    description: 'Lab #0 due'
---
You'll use two sets of tools in this class: an x86 emulator: [Qemu](#configuring-qemu) for running your kernel; and 
a [Compiler toolchain](#compiler-tool-chain), including assembler, linker, C compiler, and
debugger, for compiling and testing your kernel.

## Using `ecegrid`
All the required tools are already present on `ecegrid`. You need to configure your `PATH` variable as follows:
```
setenv PATH=~ee469/labs_2022/qemu/build/bin:$PATH
```

**If you are using bash shell**: append the line `export PATH=~ee469/labs_2022/qemu/build/bin:$PATH` at the end of your `~/.bashrc`.

You should be good to go from here.

You will be using `tmux` for debugging and it is better to brush up your `tmux` basics

### Using tmux
`tmux` allows multiple terminals to be opened on the single window.
The benefit of using tmux is that it creates a persistent terminal, which means even if the connection 
is lost or the terminal closed, the session can be recovered.

Here are some helpful tmux commands:

Launching tmux
```
$ tmux new -s <session name>
```
Detaching from a session
Ctrl+b d
Checking for the existing tmux sessions
```
$ tmux ls
```
Attaching to a tmux session
```
$ tmux a -t <session name>
```

You may use this tmux [cheatsheet](https://tmuxcheatsheet.com/) and [man page](https://man7.org/linux/man-pages/man1/tmux.1.html) 
to get a better understanding.

> Follow the below instructions if you want to setup compiler toolchain on your own machine.

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

## GDB

See the [GDB manual](http://sourceware.org/gdb/current/onlinedocs/gdb/) for a full
guide to GDB commands. Here are some particularly useful commands for
CS 444/544, some of which don't typically come up outside of OS development.

**Ctrl-c**

Halt the machine and break in to GDB at the current instruction. If QEMU has multiple virtual CPUs, this halts all of them.

**`c (or continue)`**

Continue execution until the next breakpoint or ``Ctrl-c``.

**`si (or stepi)`**

Execute one machine instruction.

**`b function or b file:line (or breakpoint)`**

Set a breakpoint at the given function or line.

**`b addr (or breakpoint)`**

Set a breakpoint at the EIP *addr*.

**`set print pretty`**

Enable pretty-printing of arrays and structs.

**`info registers`**

Print the general purpose registers, ``eip``, ``eflags``, and the segment selectors.
For a much more thorough dump of the machine register state, see QEMU's own ``info   registers`` command.

**`x N addr`**

Display a hex dump of *N* words starting at virtual address *addr*. 
If *N* is omitted, it defaults to 1. *addr* can be any expression.

**`x N i addr`**

Display the *N* assembly instructions starting at *addr*. Using``$eip`` as *addr* will display the instructions at the current instruction pointer.

**`symbol-file file`**

**(Lab 3+)** Switch to symbol file *file*. When GDB attaches to QEMU, it
has no notion of the process boundaries within the virtual machine,
so we have to tell it which symbols to use. By default, we configure
GDB to use the kernel symbol file, ``obj/kern/kernel``. If the
machine is running user code, say ``hello.c``, you can switch to the
hello symbol file using ``symbol-file obj/user/hello``.


> QEMU represents each virtual CPU as a thread in GDB, so you can use all 
> of GDB's thread-related commands to view or manipulate QEMU's virtual
  CPUs.

**`thread n`**

GDB focuses on one thread (i.e., CPU) at a time. This command switches that focus to thread *n*, numbered from zero.

**`info threads`**

List all threads (i.e., CPUs), including their state (active or halted) and what function they're in.


## QEMU

QEMU includes a built-in monitor that can inspect and modify the machine
state in useful ways. To enter the monitor, press Ctrl-a c in the
terminal running QEMU. Press Ctrl-a c again to switch back to the serial
console.

For a complete reference to the monitor commands, see the [QEMU manual](http://wiki.qemu.org/download/qemu-doc.html#pcsys_005fmonitor).
Here are some particularly useful commands:

**`xp N x paddr`**

Display a hex dump of *N* words starting at *physical* address
*paddr*. If *N* is omitted, it defaults to 1. This is the physical
memory analogue of GDB's ``x`` command.

**`info registers`**

Display a full dump of the machine's internal register state. In
particular, this includes the machine's *hidden* segment state for
the segment selectors and the local, global, and interrupt
descriptor tables, plus the task register. This hidden state is the
information the virtual CPU read from the GDT/LDT when the segment
selector was loaded. Here's the CS when running in the JOS kernel in
lab 1 and the meaning of each field:

```

        CS =0008 10000000 ffffffff 10cf9a00 DPL=0 CS32 [-R-]

    ``CS =0008``
        The visible part of the code selector. We're using segment 0x8.
        This also tells us we're referring to the global descriptor
        table (0x8&4=0), and our CPL (current privilege level) is
        0x8&3=0.
    ``10000000``
        The base of this segment. Linear address = logical address +
        0x10000000.
    ``ffffffff``
        The limit of this segment. Linear addresses above 0xffffffff
        will result in segment violation exceptions.
    ``10cf9a00``
        The raw flags of this segment, which QEMU helpfully decodes for
        us in the next few fields.
    ``DPL=0``
        The privilege level of this segment. Only code running with
        privilege level 0 can load this segment.
    ``CS32``
        This is a 32-bit code segment. Other values include ``DS`` for
        data segments (not to be confused with the DS register), and
        ``LDT`` for local descriptor tables.
    ``[-R-]``
        This segment is read-only.
```

**`info mem`**

**(Lab 2+)** Display mapped virtual memory and permissions. For example,

```
  ef7c0000-ef800000 00040000 urw
  efbf8000-efc00000 00008000 -rw
```

tells us that the 0x00040000 bytes of memory from 0xef7c0000 to
0xef800000 are mapped read/write and user-accessible, while the
memory from 0xefbf8000 to 0xefc00000 is mapped read/write, but only
kernel-accessible.

QEMU also takes some useful command line arguments, which can be passed
into the JOS makefile using the `QEMUEXTRA <#make-qemuextra>`__
variable.

**`make QEMUEXTRA='-d int' ...`**

Log all interrupts, along with a full register dump, to
``qemu.log``. You can ignore the first two log entries, "SMM: enter"
and "SMM: after RMS", as these are generated before entering the
boot loader. After this, log entries look like

```

 4: v=30 e=0000 i=1 cpl=3 IP=001b:00800e2e pc=00800e2e SP=0023:eebfdf28 EAX=00000005
 EAX=00000005 EBX=00001002 ECX=00200000 EDX=00000000
 ESI=00000805 EDI=00200000 EBP=eebfdf60 ESP=eebfdf28
        ...
```

The first line describes the interrupt. The ``4:`` is just a log
record counter. ``v`` gives the vector number in hex. ``e`` gives
the error code. ``i=1`` indicates that this was produced by an
``int`` instruction (versus a hardware interrupt). The rest of the
line should be self-explanatory. See `info registers` for a description of the
register dump that follows.

**Note**: If you're running a pre-0.15 version of QEMU, the log will be
written to ``/tmp`` instead of the current directory.