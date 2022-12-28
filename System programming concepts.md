## **3.1 System Calls**
A _system call_ is a controlled entry point into the kernel, allowing a process to request kernel to perform some action on it's behalf. The kernel makes a range of services accessible to programs via the system call API. These services include, for example, creating a new process, performing I/O, and creating a pipe for inter-process communication. 
Before going into the details of how a system call works, we note some general points:
- A system call changes the processor state from user mode to kernel mode, so that the CPU can access protected kernel memory.
- The set of system calls is fixed. Each system call is identified by a unique number.
- Each system call may have a set of arguments that specify information to be transferred from user space(i.e., the process's virtual address space) to kernel space and vice versa.
From a programming point of view, invoking a system call looks much like calling a C function. However, behind the scenes, many steps occur during the execution of a system call. To illustrate this, we consider the steps occur during the execution of a system call. To illustrate this, we consider the steps in the order that they occur on a specific hardware implementation, the x86-32.
1. The application program makes a system call by invoking a wrapper function in the C library.
2. The wrapper function must make all of the system call arguments available to the system call trap-handling routine. These arguments are passed to the wrapper via the stack, but the kernel expects them in specific registers. The wrapper function copies the arguments to these registers.
3. Since all system calls enter the kernel in the same way, the kernel needs some method of identifying the system call. To permit this, the wrapper function copies the system call number into a specific CPU register (%eax).
4. The wrapper function executes a trap machine instruction (int 0x80), which
causes the processor to switch from user mode to kernel mode and execute
code pointed to by location 0x80 (128 decimal) of the system’s trap vector. 
5. In response to the trap to location 0x80, the kernel invokes its system_call() routine (located in the assembler file arch/i386/entry.S) to handle the trap. This handler:
   1. Saves register values onto the kernel stack.
   2. Checks the validity of the system call number.
   3. Invokes the appropriate system call service routine, which is found by using the system call number to index a table of all system call service routines. If the system call service routine has any arguments, it first checks their validity; for example, it checks that addresses point to valid locations in user memory. Then the service routines performs the required task, which may involve modifying values at addresses specified in the given arguments and transferring data between user memory and kernel memory. Finally, the service routine returns a result status to the system_call() routine.
   4. Restores register values from the kernel stack and places the system call return value on the stack.
   5. Returns to the wrapper function, simultaneously returning the processor to user mode.
6.  If the return value of the system call service routine indicated an error, the wrapper function sets the global variable _errno_ using this value. The warpper function then returns to the caller, providing an integer return value indication the success or failure of the system call.

![[Screenshot from 2022-05-23 22-55-35.png]]
