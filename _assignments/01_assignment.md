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
The [PC Assembly Language Book](/static_files/read/pcasm-book.pdf) is an excellent place
to start. Hopefully, the book contains mixture of new and old material
for you.

*Warning:* Unfortunately the examples in the book are written for the
NASM assembler, whereas we will be using the GNU assembler. NASM uses
the so-called *Intel* syntax while GNU uses the *AT&T* syntax. While
semantically equivalent, an assembly file will differ quite a lot, at
least superficially, depending on which syntax is used. Luckily the
conversion between the two is pretty simple, and is covered in
[Brennan's Guide to Inline Assembly](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html).

:question: <span style="color:red">Exercise 1</span>
Familiarize yourself with the assembly language materials
available on `the cs444 reference page <../ref.html>`__. You
don't have to read them now, but you'll almost certainly want to refer
to some of this material when reading and writing x86 assembly.

We do recommend reading the section "The Syntax" in
[Brennan's Guide to Inline Assembly](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html).
It gives a good (and quite brief) description of the AT&T assembly
syntax we'll be using with the GNU assembler in JOS.



