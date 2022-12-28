Linux is a member of the [[UNIX]] family of operating systems. In computing terms, [[UNIX]] has a long history. The first part of this chapter provides a brief outline of that history. We begin with a description of the origins of the [[UNIX]] system and the [[C programming language]]
and then consider the two key currents that led to the [[Linux]] system as it exists today: the 
[[GNU]] project and the development of the [[Linux]] kernel.

# 1.1 A Brief History of UNIX and C

The first implementation was developed in 1969 by [[Ken Thompson]] at
[[Bell Laboratories]], It was written in assembler for a Digital [[PDP-7]] mini-computer.
The name UNIX was a pun on [[MULTICS]].
In 1970, [[UNIX]] was rewritten in assembly language for a  newly acquired Digital [[PDP-11]]
minicomputer, then a new and powerful machine.
A short time later, [[dennis ritchie]], one of Thompson's colleagues at Bell Labs and an early 
collaborator on [[UNIX]], designed an implemented the C programming language. This was an evolutionary process; C followed an earlier interpreted language, B. B was initially implemented by Thompson and drew many of its ideas from a still earlier programming language named [[BCPL]], By 1973, C had matured to a point where the [[UNIX]] kernel could be almost entirely rewritten in the new language. [[UNIX]] thus became one of the earliest operating systems to be written in a high-level language, a fact that made subsequent porting to other hardware architectures possible.

Previous widely used languages were designed with other purposes in mind: [[fortran]] for mathematical tasks performed by engineers and scientists; [[cobol]] for commercial systems processing streams of record-oriented data. C filled a hitherto empty niche, and unlike [[fortran]] and [[cobol]](which were designed by large committees), the design of C arose from the ideas and needs of a few individuals working toward a single goal: developing a high-level language for implementing the [[UNIX]] kernel and associated software. Like the UNIX operating system itself, C was designed by professional programmers for their own use. The resulting language was small, efficient, powerful, terse, modular, pragmatic, and coherent in its design.

### **Unix underwent through six editions.**
1. First Edition in 1971: By this time, [[unix]] was running on the [[pdp-11 ]]and already had [[fortran]] compiler and versions of many programs still used today, including ar, cat, chmod, chown, cp, dc, ed, find, ln, ls, mail, mkdir, mv, rm, sh, su, and who.
2. Second Edition in 1972: By this time, [[UNIX]] was installed on ten machines within [[at&t]]
3. Third Edition in 1973: This edition included a C compiler and the first implementation of pipies.
4. Fourth Edition, November 1973: This was the first version to be almost totally written in C.
5. Fifth Edition, June 1974: By this time, UNIX was installed on more than 50 systems.
6. Sixth Edition, May 1975: This was the first edition to be widely used outside [[at&t]].


Over the period of these releases, the use and reputation of [[UNIX]] began to spread, first within [[at&t]], and then beyond. An important contribution to this growing awareness was the publication of a paper on [[UNIX]] in the widely read journal Communications of the ACM([Ritchie& Thompson, 1974]).

At this time, AT&T held a government-sanctioned monopoly on the US tele-
phone system. The terms of AT&T’s agreement with the US government prevented
it from selling software, which meant that it could not sell UNIX as a product.
Instead, beginning in 1974 with Fifth Edition, and especially with Sixth Edition,
AT&T licensed UNIX for use in universities for a nominal distribution fee. The
university distributions included documentation and the kernel source code (about
10,000 lines at the time).

# 1.2 A Brief History of Linux
The term Linux is commonly used to refer to the entire UNIX-like operating sys-
tem of which the Linux kernel forms a part. However, this is something of a misno-
mer, since many of the key components contained within a typical commercial
Linux distribution actually originate from a project that predates the inception
of Linux by several years.

### The Gnu Project
In 1984, Richard Stallman, an exceptionally talented programmer who had been working at MIT, set to work on creating a ["free"](http://www.gnu.org/philosophy/free-sw.html) [[UNIX]] implementation. and he wanted it to be free in a legal sense, rather than a financial sense. Nevertheless, the legal freedom that Stallman described carried with it the implicit consequence that sotware such as operating systems would be available at no or very low cost.
Stallman started the GNU project (a recursively defined acronym
for “GNU’s not UNIX”) to develop an entire, freely available, UNIX-like system,
consisting of a kernel and all associated software packages, and encouraged others
to join him. In 1985, Stallman founded the Free Software Foundation (FSF), a non-
profit organization to support the GNU project as well as the development of free
software in general.
>When the GNU project was started, [[BSD]] was not free in the sense that Stall-
man meant. Use of BSD still required a license from AT&T, and users could
not freely modify and redistribute the AT&T code that formed part of BSD.

The GNU project did not initially produce a working UNIX kernel, but did
produce a wide range of other programs. Since these programs were designed to
run on a UNIX-like operating system, they could be, and were, used on existing
UNIX implementations and, in some cases, even ported to other operating sys-
tems. Among the more well-known programs produced by the GNU project are the
Emacs text editor, GCC (originally the GNU C compiler, but now renamed the
GNU compiler collection, comprising compilers for C, C++, and other languages),
the bash shell, and glibc (the GNU C library).
By the early 1990s, the GNU project had produced a system that was virtually
complete, except for one important component: a working UNIX kernel. The GNU
project had started work on an ambitious kernel design, known as the GNU/HURD,
based on the Mach microkernel. However, the HURD was far from being in a form
that could be released. (At the time of writing, work continues on the HURD,
which currently runs only on the x86-32 architecture.)
	Because a significant part of the program code that constitutes what is commonly 
	known as the Linux system actually derives from the GNU project, Stall-
	man prefers to use the term GNU/Linux to refer to the entire system. The
	question of naming (Linux versus GNU/Linux) is the source of some debate
	in the free software community. Since this book is primarily concerned with
	the API of the Linux kernel, we’ll generally use the term Linux.
The stage was set. All that was required was a working kernel to go with the other-
wise complete UNIX system already produced by the GNU project.


### The Linux Kernel
In 1991, Linus Torvalds, a Finnish student at the University of Helsinki, was
inspired to write an operating system for his Intel 80386 PC. In the course of his
studies, Torvalds had come into contact with Minix, a small UNIX-like operating
system kernel developed in the mid-1980s by Andrew Tanenbaum, a university pro-
fessor in Holland. Tanenbaum made Minix, complete with source code, available
as a tool for teaching operating system design in university courses. The Minix ker-
nel could be built and run on a 386 system. However, since its primary purpose was
as a teaching tool, it was designed to be largely independent of the hardware archi-
tecture, and it did not take full advantage of the 386 processor’s capabilities.
Torvalds therefore started on a project to create an efficient, full-featured
UNIX kernel to run on the 386. Over a few months, Torvalds developed a basic
kernel that allowed him to compile and run various GNU programs. Then, on
October 5, 1991, Torvalds requested the help of other programmers

The call for support proved effective. Other programmers joined Torvalds in
the development of Linux, adding various features, such as an improved file
system, networking support, device drivers, and multiprocessor support. By
March 1994, the developers were able to release version 1.0. Linux 1.2 appeared
in March 1995, Linux 2.0 in June 1996, Linux 2.2 in January 1999, and Linux 2.4 in
January 2001. Work on the 2.5 development kernel began in November 2001, and
led to the release of Linux 2.6 in December 2003.

### The BSDs
Alongside another free UNIX was already available for the x86-32 during the early 1990s.
Bill and Lynne Jolitz had developed a port of the already mature BSD system for the x86-32, known as 386/BSD.
Main variant's in BSD family:
1. FreeBSD
2. NetBSD
3. OpenBSD
4. DragonFlyBSD.

### Linux kernel version numbers
Linux follows a  release-early, release-often model, so that new kernel revisions appear frequently. As the Linux user base grew, the release model was adapted to decrease disruption to existing users. Specifically, following the release of Linux 1.0, they adopted a kernel version numbering scheme with each release numbered x.y.x
x represents a major version
y represents a minor version
z represents revision of the minor version

### Ports to other hardware architectures
During the initial development of Linux, efficient implementation on the Intel
80386 was the primary goal, rather than portability to other processor architectures. However, with the increasing popularity of Linux, ports to other processor
architectures began to appear, starting with an early port to the Digital Alpha chip.
The list of hardware architectures to which Linux has been ported continues to grow
and includes x86-64, Motorola/IBM PowerPC and PowerPC64, Sun SPARC and
SPARC64 (UltraSPARC), MIPS, ARM (Acorn), IBM zSeries (formerly System/390),
Intel IA-64 (Itanium; see [Mosberger & Eranian, 2002]), Hitachi SuperH, HP
PA-RISC, and Motorola 68000

### Linux distributions
Precisely speaking, the term Linux refers just to the kernel developed by Linus Torvalds
and others. However, the term Linux is commonly used to mean the kernel, plus a
wide range of other software (tools and libraries) that together make a complete
operating system. In the very early days of Linux, the user was required to assemble
all of this software, create a file system, and correctly place and configure all of the
software on that file system. This demanded considerable time and expertise. As a
result, a market opened for Linux distributors, who created packages (distributions)
to automate most of the installation process, creating a file system and installing
the kernel and other required software.
The earliest distributions appeared in 1992, and included MCC Interim Linux
(Manchester Computing Centre, UK), TAMU (Texas A&M University), and SLS
(SoftLanding Linux System). The oldest surviving commercial distribution, Slackware,
appeared in 1993. The noncommercial Debian distribution appeared at around
the same time, and SUSE and Red Hat soon followed. The currently very popular
Ubuntu distribution first appeared in 2004. Nowadays, many distribution compa-
nies also employ programmers who actively contribute to existing free software
projects or initiate new projects
