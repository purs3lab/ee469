---
layout: page
title: Materials
permalink: /materials/
---


Reference Materials
===================

C Programming
-------------

-   [The C programming  language](http://www.amazon.com/The-Programming-Language-Brian-Kernighan/dp/0131103628)
-   [Modern C](http://icube-icps.unistra.fr/img_auth.php/d/db/ModernC.pdf)
-   [Learn C The Hard Way](http://c.learncodethehardway.org/book/)

UNIX
----

-   [The UNIX Operating System](https://www.youtube.com/watch?v=tc4ROCJYbm0), AT&T Archives at YouTube
-   [The UNIX Time-Sharing System](https://www.cs.berkeley.edu/~brewer/cs262/UNIX-annotated.pdf), Dennis M. Ritchie and Ken L.Thompson, Bell System Technical Journal
-   [The Evolution of the Unix Time-sharing System](http://www.read.seas.harvard.edu/~kohler/class/aosref/ritchie84evolution.pdf), Dennis M. Ritchie
-   [A Commentary on the Sixth Edition UNIX Operating System](static_files/read/unix6.pdf), John Lions

x86 Emulation
-------------

-   [QEMU](http://www.qemu.org/) ([manual](http://wiki.qemu.org/Qemu-doc.html))
-   [Bochs](http://bochs.sourceforge.net) ([manual](http://bochs.sourceforge.net/doc/docbook/user/index.html), [debugging](http://bochs.sourceforge.net/doc/docbook/user/internal-debugger.html))

x86 Assembly Language
---------------------

-   [PC Assembly Language](http://www.drpaulcarter.com/pcasm/), Paul A. Carter, November 2003.
-   [Intel 80386 Programmer's Reference Manual](http://www.logix.cz/michal/doc/i386/), 1987 (HTML).
    Much shorter than the full current Intel Architecture manuals below, but
    describes all processor features used in EE469.
-   [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html),

    > -   [Volume I: Basic Architecture](static_files/read/ia32/IA32-1.pdf)
    > -   [Volume 2A: Instruction Set Reference, A-M](static_files/read/ia32/IA32-2A.pdf)
    > -   [Volume 2B: Instruction Set Reference, N-Z](static_files/read/ia32/IA32-2B.pdf)
    > -   [Volume 2C: Instruction Set Reference, Safer mode extensions](static_files/read/ia32/IA32-2C.pdf)
    > -   [Volume 3A: System Programming Guide, Part 1](static_files/read/ia32/IA32-3A.pdf)
    > -   [Volume 3B: System Programming Guide, Part 2](static_files/read/ia32/IA32-3B.pdf)
    > -   [Volume 3C: System Programming Guide, Part 3](static_files/read/ia32/IA32-3C.pdf)

-   Multiprocessor references:

    > -   [MP specification](static_files/read/ia32/MPspec.pdf)
    > -   [IO APIC](static_files/read/ia32/ioapic.pdf)

-   [AMD64 Architecture Programmer's
    Manual](https://developer.amd.com/resources/developer-guides-manuals/).

    > -   Covers both the "classic" 32-bit x86 architecture and the new
          >     64-bit extensions supported by the latest AMD and Intel
          >     processors.

-   Writing inline assembly language with GCC:

    > -   [Brennan's Guide to Inline Assembly](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html)
    > -   [Inline assembly for x86 in Linux](http://www.ibm.com/developerworks/linux/library/l-ia.html)
    > -   [GCC-Inline-Assembly-HOWTO](http://www.ibiblio.org/gferg/ldp/GCC-Inline-Assembly-HOWTO.html)

-   Loading x86 executables in the ELF format:

    > -   [Wikipedia: ELF](http://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
    > -   [Tool Interface Standard (TIS) Executable and Linking Format (ELF)](static_files/read/elf.pdf)

PC Hardware Programming
-----------------------

-   General PC architecture information:

    > -   [Phil Storrs PC Hardware book](http://web.archive.org/web/20040603021346/http://members.iweb.net.au/~pstorr/pcbook/),
          >     Phil Storrs, December 1998.
    > -   [Bochs technical hardware specifications directory](http://bochs.sourceforge.net/techdata.html).

-   General BIOS and PC bootstrap:

    > -   [Wikipedia: BIOS](https://en.wikipedia.org/wiki/BIOS_interrupt_call)
    > -   [BIOS Services and Software Interrupts](http://www.htl-steyr.ac.at/~morg/pcinfo/hardware/interrupts/inte1at0.htm),
          >     Roger Morgan, 1997.
    > -   ["El Torito" Bootable CD-ROM Format Specification](static_files/read/boot-cdrom.pdf), Phoenix/IBM, January 1995.

-   VGA display - kern/console.c

    > -   [OS-Dev](http://wiki.osdev.org/VGA_Hardware), [Wikipedia: VGA](https://en.wikipedia.org/wiki/Video_Graphics_Array),
          >     [Wikipedia: CGA](https://en.wikipedia.org/wiki/Color_Graphics_Adapter)
    > -   [Wikipedia: VESA](https://en.wikipedia.org/wiki/VESA_BIOS_Extensions),
          >     [VESA BIOS Extension (VBE) 3.0](http://web.archive.org/web/20080302090304/http://www.vesa.org/public/VBE/vbe3.pdf),
          >     [Video Electronics Standards Association](http://www.vesa.org/), September 1998. [(local copy)](static_files/read/hardware/vbe3.pdf)
    > -   VGADOC, Finn ThÃ¸gersen, 2000. [(local copy - text)](static_files/read/hardware/vgadoc/), [(local copy - ZIP)](static_files/read/hardware/vgadoc4b.zip)
    > -   [Free VGA Project](http://www.osdever.net/FreeVGA/home.htm).

-   Keyboard and Mouse - kern/console.c

    > -   [Adam Chapweske's resources](http://www.computer-engineering.org/index.html).

-   8253/8254 Programmable Interval Timer (PIT) - inc/timerreg.h

    > -   [82C54 CHMOS Programmable Interval Timer](http://www.intel.com/design/archives/periphrl/docs/23124406.htm),
          >     Intel, October 1994. [(local copy)](static_files/read/hardware/82C54.pdf)
    > -   [Data Solutions 8253/8254 Tutorial](http://www.decisioncards.com/io/tutorials/8254_tut.html)

-   8259/8259A Programmable Interrupt Controller (PIC) - kern/picirq.\*

    > -   [8259A Programmable Interrupt Controller](static_files/read/hardware/8259A.pdf), Intel, December 1988.

-   Real-Time Clock (RTC) - kern/kclock.\*
    > -   [Wikipedia:Nonvolatile BIOS memory](https://en.wikipedia.org/wiki/Nonvolatile_BIOS_memory)
    > -   [Phil Storrs PC Hardware book](http://web.archive.org/web/20040603021346/http://members.iweb.net.au/~pstorr/pcbook/)
    >  - [Understanding the CMOS](http://web.archive.org/web/20040603021346/http://members.iweb.net.au/~pstorr/pcbook/book5/cmos.htm)
    > - [A list of what is in the CMOS](http://web.archive.org/web/20040603021346/http://members.iweb.net.au/~pstorr/pcbook/book5/cmoslist.htm)
    > - [CMOS Map Table](http://www.bioscentral.com/misc/cmosmap.htm),
    > - [CMOS Map](http://bochs.sourceforge.net/techspec/CMOS-reference.txt)
    > - [M48T86 PC Real-Time Clock](static_files/read/hardware/M48T86.pdf)


-   16550 UART Serial Port - kern/console.c

    > -   [Wikipedia: Serial port](https://en.wikipedia.org/wiki/Serial_port)
    > -   [PC16550D Universal Asynchronous Receiver/Transmitter with FIFOs](http://www.ti.com/lit/ds/symlink/pc16550d.pdf)
    > -   [Technical Data on 16550](http://byterunner.com/16550.html)
    > -   [Interfacing the Serial / RS232 Port](http://www.beyondlogic.org/serial/serial.htm)

-   IEEE 1284 Parallel Port - kern/console.c

    > -   [Parallel Port Central](http://janaxelson.com/parport.htm)
    > -   [Parallel Port Background](http://www.fapo.com/porthist.htm)
    > -   [IEEE 1284 - Updating the PC Parallel Port](http://zone.ni.com/devzone/cda/tut/p/id/3466)
    > -   [Interfacing the Standard Parallel Port](http://www.beyondlogic.org/spp/parallel.htm)

-   IDE hard drive controller - fs/ide.c

    > -   [ATA8-ACS]: [AT Attachment 8 - ATA/ATAPI Command Set](static_files/read/hardware/ATA8-ACS.pdf)
    > -   [Programming Interface for Bus Master IDE Controller](static_files/read/hardware/IDE-BusMaster.pdf)
    > -   [The Guide to ATA/ATAPI documentation](http://www.cs.utexas.edu/~dahlin/Classes/UGOS/reading/ide.html)

-   Sound cards: (not supported in EE469 kernel, but you're welcome to do it as a challenge problem!)

    > -   [Sound Blaster Series Hardware Programming Guide](static_files/read/hardware/SoundBlaster.pdf)
    > -   [8237A High Performance Programmable DMA Controller](static_files/read/hardware/8237A.pdf)
    > -   [Sound Blaster 16 Programming Document](http://homepages.cae.wisc.edu/~brodskye/sb16doc/sb16doc.html)
    > -   [Sound Programming](http://www.inversereality.org/tutorials/sound%20programming/soundprogramming.html)

-   E100 Network Interface Card:

    > -   [Intel 8255x 10/100 Mbps Ethernet Controller Family Open Source Software Developer Manual](static_files/read/hardware/8255X_OpenSDM.pdf)
    > -   [82559ER Fast Ethernet PCI Controller Datasheet](static_files/read/hardware/82559ER_datasheet.pdf)

-   E1000 Network Interface Card:

    > -   [PCI/PCI-X Family of Gigabit Ethernet Controllers Software Developer's Manual](static_files/read/hardware/8254x_GBe_SDM.pdf)

-   Booting:

    > -   [Multiboot](http://www.gnu.org/software/grub/manual/multiboot/multiboot.html)
    > -   [Minimal Intel Architecture Boot Loader](static_files/read/miniboot.pdf)



