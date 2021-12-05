---
type: assignment
date: 2021-09-03T8:00:00+4:30
title: 'Lab #1 - Booting a PC'
due_event: 
    type: due
    date: 2021-09-22T23:59:00+3:30
    description: 'Assignment #1 due'
---
# Introduction

This lab is split into three parts. The first part concentrates on
getting familiarized with x86 assembly language, the QEMU x86 emulator,
and the PC's power-on bootstrap procedure. The second part examines the
boot loader for our kernel, which resides in the ``boot/``
directory of the ``jos/`` tree. Finally, the third part delves into the
initial template for our kernel itself, named JOS, which resides
in the ``kernel/`` directory.

# Software Setup

The files you will need for this and subsequent lab assignments in this
course are distributed using the [Git](http://www.git-scm.com/)
version control system. To learn more about Git, take a look at the [Git user's manual](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html),
or, if you are already familiar with other version control systems, you
may find this [CS-oriented overview of Git](http://eagain.net/articles/git-for-computer-scientists/) useful.

The URL for the lab Git repository is `https://github.com/EE469/lab1.git`

Clone the repository on to the local machine:
```
$ git clone git@github.com:EE469/lab1.git
Cloning into lab1...
$ cd lab1
```

Git allows you to keep track of the changes you make to the code. 
For example, if you are finished with one of the exercises, and want to checkpoint your progress, 
you can commit your changes by running:

```
$ git commit -am 'my solution for lab1 exercise 9'
Created commit 60d2135: my solution for lab1 exercise 9
1 files changed, 1 insertions(+), 0 deletions(-)
$
```

This *commit* will store your progress locally.

You can keep track of your changes by using the git diff command. Running git diff will display the changes to your code 
since your last commit, and git diff `origin/lab1` will display the changes relative to the initial code supplied for this lab. 
Here, `origin/lab1` is the name of the git branch with the initial code you downloaded from our server for this assignment.

We have set up the appropriate compilers and simulators for you on `ecngrid`. 


If you are working on a non-ecngrid machine, you'll need to install qemu and possibly gcc following the directions 
on the [tools](../00_assignment) page. We've made several useful debugging changes to qemu and some of the 
later labs depend on these patches, so you must build your own. 

# Hand-In Procedure
You will turn in your assignments through the BrightSpace (one for a group). 

The lab code comes with GNU Make rules to make submission easier. 
After committing your final changes to the lab, type make handin to submit your lab.

```
$ git commit -am "ready to submit my lab"
[lab1 c2e3c8b] ready to submit my lab
2 files changed, 18 insertions(+), 2 deletions(-)
$ make handin
tar: Removing leading `/' from member names
tar: Removing leading `/' from hard link targets
Please submit lab1-handin.tar.gz to Brightspace for your group.
```

Now upload your `lab1-handin.tar.gz` as the submission for your group.

If use `make handin` and you have either uncomitted changes or untracked files, you will see output similar to the following:

```
M hello.c
?? bar.c
?? foo.pyc
Untracked files will not be handed in.  Continue? [y/N]
```

Inspect the above lines and make sure all files that your lab solution 
needs are tracked i.e. not listed in a line that begins with ??.
In the case that `make handin` does not work properly, try fixing the problem with the curl or Git commands. 

You can run `make grade` to test your solutions with the grading program.
After checking your score from running `make grade`, which will run an autograder script (the score shown is not guaranteed, 
but failure to pass make grade will not get the score; score will be finalized after checking your pushed sourcecode)

## Hint
In this lab, you may want to read the following files:
```
    boot/boot.S
    boot/main.c
    inc/elf.h
    kern/entry.S
    kern/entrypgdir.c
    kern/printf.c
    lib/printfmt.c
    kern/console.h
    kern/console.c
    inc/stab.h
    kern/kdebug.h
    kern/kdebug.c
    kern/monitor.h
    kern/monitor.c
```
And, you must write your code to the following files:
```
    lib/printfmt.c;     vprintfmt() (for %o, the octet part)
    kern/kdebug.c;      debuginfo_eip()
    kern/monitor.c;     commands[] and mon_backtrace()
```

# Part 1: PC Bootstrap

The purpose of the first exercise is to introduce you to x86 assembly
language and the PC bootstrap process, and to get you started with QEMU
and QEMU/GDB debugging. You will not have to write any code for this
part of the lab, but you should go through it anyway for your own
understanding and be prepared to answer the questions posed below.

## Getting Started with x86 assembly

If you are not already familiar with x86 assembly language, you will
quickly become familiar with it during this course! 
The [PC Assembly Language Book](/ee469/static_files/read/pcasm-book.pdf) is an excellent place
to start. Hopefully, the book contains mixture of new and old material
for you.

*Warning:* Unfortunately the examples in the book are written for the
NASM assembler, whereas we will be using the GNU assembler. NASM uses
the so-called *Intel* syntax while GNU uses the *AT&T* syntax. While
semantically equivalent, an assembly file will differ quite a lot, at
least superficially, depending on which syntax is used. Luckily the
conversion between the two is pretty simple, and is covered in
[Brennan's Guide to Inline Assembly](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html).

## <span style="color:blue">Exercise 1</span>
> Familiarize yourself with the assembly language materials
available on [the EE469 reference page](/ee469/materials/). You
don't have to read them now, but you'll almost certainly want to refer
to some of this material when reading and writing x86 assembly.

> We do recommend reading the section "The Syntax" in
[Brennan's Guide to Inline Assembly](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html).
It gives a good (and quite brief) description of the AT&T assembly
syntax we'll be using with the GNU assembler in JOS.

Certainly the definitive reference for x86 assembly language programming
is Intel's instruction set architecture reference, which you can find on
[the EE469 reference page](/ee469/materials/) in two flavors: an
HTML edition of the old [80386 Programmer's Reference Manual](http://www.logix.cz/michal/doc/i386/), which is much shorter and
easier to navigate than more recent manuals but describes all of the x86
processor features that we will make use of in cs444/544; and the full,
latest and greatest [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
from Intel, covering all the features of the most recent processors that we won't need in class but you may be interested in learning about. An
equivalent (and often friendlier) set of manuals is [available from AMD](https://www.amd.com/system/files/TechDocs/24592.pdf).
Save the Intel/AMD architecture manuals for later or use them for reference when you want to look up the 
definitive explanation of a particular processor feature or instruction.


## Simulating the x86

Instead of developing the operating system on a real, physical personal
computer (PC), we use a program that faithfully emulates a complete PC:
the code you write for the emulator will boot on a real PC and you
will be asked to test in the recitation.
Using an emulator simplifies debugging; you can, for example, set break points
inside of the emulated x86, which is difficult to do with the silicon
version of an x86.

In EE469, we will use the [QEMU Emulator](http://www.qemu.org/),
a modern and relatively fast emulator.
While QEMU's built-in monitor provides only limited debugging support,
QEMU can act as a remote debugging target for the
[GNU debugger](http://www.gnu.org/software/gdb/) (GDB),
which we'll use in this lab to step through the early boot process.

To get started, clone the Lab 1 repo into your own directory
as described above in "Software Setup", then type make
(or gmake on BSD systems) in the `jos/` directory to build the minimal EE469 boot loader and kernel you will start with.
(It's a little generous to call the code we're running here a "kernel," but we'll flesh it out throughout the semester.)

```
$ cd jos
$ make
+ as kern/entry.S
+ cc kern/init.c
+ cc kern/console.c
+ cc kern/monitor.c
+ cc kern/printf.c
+ cc lib/printfmt.c
+ cc lib/readline.c
+ cc lib/string.c
+ ld obj/kern/kernel
+ as boot/boot.S
+ cc -Os boot/main.c
+ ld boot/boot
boot block is 414 bytes (max 510)
+ mk obj/kern/kernel.img
```

Now you're ready to run QEMU, supplying the file `obj/kern/kernel.img`, created above, as the contents of the emulated
PC's "virtual hard disk." This hard disk image contains both our boot  loader (`obj/boot/boot`) and our kernel (`obj/kernel`).

```
$ make qemu-nox
```

This executes QEMU with the options required to set the hard disk and
direct serial port output to the terminal. Some text should appear in
the QEMU window:

```
$ make qemu-nox
***
*** Use Ctrl-a x to exit qemu
***
qemu-system-i386 -nographic -drive file=obj/kern/kernel.img,index=0,media=disk,format=raw -serial mon:stdio -gdb tcp::26003 -D qemu.log
444544 decimal is XXX octal!
entering test_backtrace 5
entering test_backtrace 4
entering test_backtrace 3
entering test_backtrace 2
entering test_backtrace 1
entering test_backtrace 0
leaving test_backtrace 0
leaving test_backtrace 1
leaving test_backtrace 2
leaving test_backtrace 3
leaving test_backtrace 4
leaving test_backtrace 5
Welcome to the JOS kernel monitor!
Type 'help' for a list of commands.
K>
```

Everything after "Booting from Hard Disk..." was printed by our
skeletal JOS kernel; the `K>` is the prompt printed by the small
*monitor*, or interactive control program, that we've included in the
kernel. These lines printed by the kernel will also appear in the
regular shell window from which you ran QEMU. This is because for
testing and lab grading purposes we have set up the JOS kernel to write
its console output not only to the virtual VGA display (as seen in the
QEMU window), but also to the simulated PC's virtual serial port, which
QEMU in turn outputs to its own standard output. Likewise, the JOS
kernel will take input from both the keyboard and the serial port, so
you can give it commands in either the VGA display window or the
terminal running QEMU. Alternatively, you can use the serial console
without the virtual VGA by running make qemu-nox. This may be convenient
if you are SSH'd into a remote server.

There are only two commands you can give to the kernel monitor, `help`
and `kerninfo` (`info-kern` in some versions of QEMU).

```
K> help
help - display this list of commands
kerninfo - display information about the kernel
K> kerninfo
Special kernel symbols:
  entry  f010000c (virt)  0010000c (phys)
  etext  f0101a75 (virt)  00101a75 (phys)
  edata  f0112300 (virt)  00112300 (phys)
  end    f0112960 (virt)  00112960 (phys)
Kernel executable memory footprint: 75KB
K>
```

The `help` command is obvious, and we will shortly discuss the meaning
of what the `kerninfo` command prints. Although simple, it's important
to note that this kernel monitor is running "directly" on the "raw
(virtual) hardware" of the simulated PC. This means that you should be
able to copy the contents of `obj/kern/kernel.img` onto the first few
sectors of a *real* hard disk, insert that hard disk into a real PC,
turn it on, and see exactly the same thing on the PC's real screen as
you did above in the QEMU window. (We don't recommend you do this on a
real machine with useful information on its hard disk, though, because
copying `kernel.img` onto the beginning of its hard disk will trash
the master boot record and the beginning of the first partition,
effectively causing everything previously on the hard disk to be lost!)

## The PC's Physical Address Space

We will now dive into a bit more detail about how a PC starts up. A PC's
physical address space is hard-wired to have the following general
layout:

```

    +------------------+  <- 0xFFFFFFFF (4GB)
    |      32-bit      |
    |  memory mapped   |
    |     devices      |
    |                  |
    /\/\/\/\/\/\/\/\/\/\

    /\/\/\/\/\/\/\/\/\/\
    |                  |
    |      Unused      |
    |                  |
    +------------------+  <- depends on amount of RAM
    |                  |
    |                  |
    | Extended Memory  |
    |                  |
    |                  |
    +------------------+  <- 0x00100000 (1MB)
    |     BIOS ROM     |
    +------------------+  <- 0x000F0000 (960KB)
    |  16-bit devices, |
    |  expansion ROMs  |
    +------------------+  <- 0x000C0000 (768KB)
    |   VGA Display    |
    +------------------+  <- 0x000A0000 (640KB)
    |                  |
    |    Low Memory    |
    |                  |
    +------------------+  <- 0x00000000

```

The first PCs, which were based on the 16-bit Intel 8088 processor, were
only capable of addressing 1MB of physical memory. The physical address
space of an early PC would therefore start at ``0x00000000`` but end at
``0x000FFFFF`` instead of ``0xFFFFFFFF``. The 640KB area marked "Low
Memory" was the *only* random-access memory (RAM) that an early PC could
use; in fact the very earliest PCs only could be configured with 16KB,
32KB, or 64KB of RAM!

The 384KB area from ``0x000A0000`` through ``0x000FFFFF`` was reserved
by the hardware for special uses such as video display buffers and firmware
held in non-volatile memory. The most important part of this reserved
area is the Basic Input/Output System (BIOS), which occupies the 64KB
region from ``0x000F0000`` through ``0x000FFFFF``. In early PCs the
BIOS was held in true read-only memory (ROM), but current PCs store the
BIOS in updateable flash memory. The BIOS is responsible for performing
basic system initialization such as activating the video card and checking
the amount of memory installed. After performing this initialization, the
BIOS loads the operating system from some appropriate location such as
floppy disk, hard disk, CD-ROM, or the network, and passes control of
the machine to the operating system.

When Intel finally "broke the one megabyte barrier" with the 80286 and
80386 processors, which supported 16MB and 4GB physical address spaces
respectively, the PC architects nevertheless preserved the original
layout for the low 1MB of physical address space in order to ensure
backward compatibility with existing software. Modern PCs therefore have
a "hole" in physical memory from ``0x000A0000`` to ``0x00100000``,
dividing RAM into "low" or "conventional memory" (the first 640KB)
and "extended memory" (everything else). In addition, some space at
the very top of the PC's 32-bit physical address space, above all
physical RAM, is now commonly reserved by the BIOS for use by 32-bit
PCI devices.

Recent x86 processors can support *more* than 4GB of physical RAM, so
RAM can extend further above ``0xFFFFFFFF``. In this case the BIOS must
arrange to leave a *second* hole in the system's RAM at the top of the
32-bit addressable region, to leave room for these 32-bit devices to be
mapped. Because of design limitations JOS will use only the first 256MB
of a PC's physical memory anyway, so for now we will pretend that all
PCs have "only" a 32-bit physical address space. But dealing with
complicated physical address spaces and other aspects of hardware
organization that evolved over many years is one of the important
practical challenges of OS development.
