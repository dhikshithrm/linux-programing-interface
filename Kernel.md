# KERNEL
#kernel #kerneltasks #usermode #kernelmode #processview #kernelview 
The term _operating system_ is commonly used with two different meanings:

* To denote the entire package consisting of the central software managing a computer's resources and all of the accompanying standard software tools, such as command-line interpreters, graphical user interfaces, file utilities, and editors.
* More narrowly, to refer to the central software that manages and allocates computer resources (i.e., the CPU, RAM, and devices).

The term _kernel_ is often used as a synonym for the second meaning, and it is with this meaning of the term _operating system_ that we are concerned in this book.
Although it is possible to run programs on a computer without a kernel, the presence of
a kernel greatly simplifies the writing and use of other programs, and increases the power and flexibility available to programmers. The kernel does this by providing a software layer to manage the limited resources of a computer.

The Linux kernel executable typically resides at the path name /boot/vmlinuz,
or something similar.
### Tasks performed by the kernel
Among other things, the kernel performs the following tasks:
* **Process scheduling**:A computer has one or more CPU's, which execute the instructions of programs. Like other UNIX systems, Linux is a _preemptive multitasking_ operating system, _Multitasking_ means that multiple processes can simultaneously reside in memory and each may receive use of the CPU(s). _Preemptive_ means that the rules governing which processes receive use of the CPU and for how long are determined by the kernel process scheduler (rather than by the processes themselves).
* **Memory management**: RAM remains a limited resource that the kernel must share among processes in an equitable and efficient fashion. Like most modern operating systems, Linux employs virtual memory management, a technique that confers two main advantages:
   - Processes are isolated from one another and from the kernel, so that one process can't read or modify the memory of another process or the kernel.
   - Only part of a process needs to be kept in memory, thereby lowering the memory requirements of each process and allowing more processes to be held in RAM simultaneously. This leads to better CPU utilization, since it increases the likelihood that, at any moment in time, there is at least one process that the CPU(s) can execute.
- **Provision of a file system**: The kernel provides a file system on disk, allowing files to be created, retrieved, updated, deleted, and so on.
- **Creation and termination of processes**: The kernel can load a new program into memory, providing it with the resources that it needs in order to run. Such an instance of a running program is termed a process. Once a process has completed execution, the kernel ensures that the resources it used are freed for subsequent reuse by later programs.
- **Access to devices**: The devices (mouse, monitors, keyboards, disk and tape drives, etc) attached to a computer allow data transfer between the computer and the outside world, permitting input, output or both. The kernel provides programs with an interface that standardizes and simplifies access to devices, while at the same time arbitrating access by multiple processes to each device.
- **Networking**: The kernel transmits and receives network messages on behalf of user processes. This task includes routing of network packets to the target system.
- **Provision of a system call API**: Processes can request the kernel to perform various tasks using kernel entry points known as system calls. The Linux system call API is the primary topic of this book.


In addition to the above features, multi-user operating systems such as Linux generally provide users with the abstraction of a _virtual private computer_; that is, each user can log on to the system and operate largely independently of other users. they may run different or same programs,  each of which gets a share of the CPU and operates in its own virtual address space, and these programs can independently access devices and transfer information over the network. The kernel resolves potential conflicts in accessing hardware resources, so users and processes are generally unaware of the conflicts.

### Kernel mode and User mode
Modern processor architectures typically allow the CPU to operate in at least two different modes: _user mode_ and _kernel mode_. Hardware instructions allow switching from one mode to the other. Correspondingly, areas of virtual memory can be marked as being part of _user space_ or _kernel space_. When running in user mode, the CPU can access only memory that is marked as being in user space; attempts to access memory in kernel space result in a hardware exception. When running in kernel mode, the CPU can access both user and kernel memory space.
Certain operations can be performed only while the processor is operating in kernel mode. Examples include executing the halt instruction to stop the system, accessing the memory-management hardware, and initiating device I/O operations. By taking advantage of this hardware design to place the operating system in kernel space, OS implementer's can ensure that user processes are not able to access the instructions and data structures of the kernel, or to perform operations that would adversely affect the operation of the system.

### Process versus kernel views of the system
In many everyday programming tasks, we are accustomed to thinking about programming in a process-oriented way. However, when considering various topics
covered later in this book, it can be useful to reorient our perspective to consider
things from the kernel’s point of view. To make the contrast clear, we now consider
how things look first from a process viewpoint and then from a kernel viewpoint.
A running system typically has numerous processes. For a process, many things
happen asynchronously. An executing process doesn’t know when it will next time
out, which other processes will then be scheduled for the CPU (and in what order),
or when it will next be scheduled. The delivery of signals and the occurrence of
inter-process communication events are mediated by the kernel, and can occur at
any time for a process. Many things happen transparently for a process. A process
doesn’t know where it is located in RAM or, in general, whether a particular part of
its memory space is currently resident in memory or held in the swap area (a
reserved area of disk space used to supplement the computer’s RAM). Similarly, a
process doesn’t know where on the disk drive the files it accesses are being held; it
simply refers to the files by name. A process operates in isolation; it can’t directly
communicate with another process. A process can’t itself create a new process or
even end its own existence. Finally, a process can’t communicate directly with the
input and output devices attached to the computer.
By contrast, a running system has one kernel that knows and controls every-
thing. The kernel facilitates the running of all processes on the system. The kernel
decides which process will next obtain access to the CPU, when it will do so, and for
how long. The kernel maintains data structures containing information about all
running processes and updates these structures as processes are created, change
state, and terminate. The kernel maintains all of the low-level data structures that
enable the filenames used by programs to be translated into physical locations on
the disk. The kernel also maintains data structures that map the virtual memory of
each process into the physical memory of the computer and the swap area(s) on
disk. All communication between processes is done via mechanisms provided by
the kernel. In response to requests from processes, the kernel creates new processes and terminates existing processes. Lastly, the kernel (in particular, device
drivers) performs all direct communication with input and output devices, transfer-
ring information to and from user processes as required.
Later in this book we’ll say things such as “a process can create another process,” “a process can create a pipe,” “a process can write data to a file,” and “a process can terminate by calling exit().” Remember, however, that the kernel mediates
all such actions, and these statements are just shorthand for “a process can request
that the kernel create another process,” and so on

### Further information
Modern texts covering operating systems concepts and design, with particular ref-
erence to UNIX systems, include [Tanenbaum, 2007], [Tanenbaum & Woodhull,
2006], and [Vahalia, 1996], the last of these containing much detail on virtual mem-
ory architectures. [Goodheart & Cox, 1994] provide details on System V Release 4.
[Maxwell, 1999] provides an annotated listing of selected parts of the Linux 2.2.5
kernel. [Lions, 1996] is a detailed exposition of the Sixth Edition UNIX source
code that remains a useful introduction to UNIX operating system internals.
[Bovet & Cesati, 2005] describes the implementation of the Linux 2.6 kernel.