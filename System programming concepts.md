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


The diagram below illustrates above sequence using the example of the execve() system call which runs a different program in a child process. execve() has system call number 11 (\_\_NR\_execve). Thus, in the sys_call_table vector, entry 11 contains the address of sys_execve(), the service routine for this system call. (On Linux, system call service routines typically have names of the form sys_xyz(), where xyz() is the system call in question.)

The information given in the preceding paragraphs is more than we'll usually need to know for the remainder of this book. However, it illustrates the important point that, even for a simple system call, quite a bit of work must be done, and thus system calls have a small bit of overhead.
![[Screenshot from 2022-05-23 22-55-35.png]]

## **3.2 Library Functions**
A _library function_ is simply one of the multitude of functions that constitutes the standard C library. The purposes of these functions are very diverse, including such tasks as opening file, converting a time to a human-readable format, and comparing two character strings.
Many library functions don't make any use of system calls(e.g., the string manipulation functions). On the other hand, some library functions are layered on top of system calls. For example, the _fopen()_ library function uses the _open()_ system call to actually open a file. Often, library function are designed to provide a more caller-friendly interface than the underlying system call. For example the _printf()_ function provides output formatting and data buffering, whereas the _write()_ system call just ouputs a block of bytes. Similarly, the _malloc()_ and _free()_
functions perform various bookkeeping tasks that make them a much easier way to allocate and free memory than the underlying _brk()_ system call.

## **3.3 The Standard C Library; The GNU C Library (_glibc_)**
There are different implementations of the standard C library on the various UNIX
implementations. The most commonly used implementation on Linux is the GNU
C library (_glibc_)

## **3.4 Error Handling for System Calls and Lib Functions**

```
fd = open(pathname, flags, mode);
if(fd == -1){
	/* Code to handle the error */
}
if(close(fd) == -1){
	/* Code to handle the error */
}
```

When a system call fails, it sets the global integer variable _errno_ to a positive value that identifies the specific error. Including the <errno.h> header file provides a declaration of _errno_, as well as a set of constants for the various error numbers. All of these symbolic names commence with E. The section headed ERRORS in each manual page provides a list of possible _eerrno_ values that can be returned by each system call.

```
cnt = read(fd, buf, numbytes);
if(cnt == -1){
	if(errno == EINTR)
		fprintf(stderr, "read was interrupted by a signal\n");
	else{
		/* Some other error occurred */
	}
}
```
Successful system calls and library functions never reset _errno_ to 0, so this variable
may have a nonzero value as a consequence of an error from a previous call.
Furthermore, SUSv3 permits a successful function call to set _errno_ to a nonzero value
(although few functions do this). Therefore, when checking for an error, we should
always first check if the function (in the above case we checked if cnt is -1 and only then checked _errno_ )return value indicates an error, and only then
examine _errno_ to determine the cause of the error.

A few system calls (e.g., _getpriority()_) can legitimately return –1 on success. To
determine whether an error occurs in such calls, we set _errno_ to 0 before the call,
and then check it afterward. If the call returns –1 and _errno_ is nonzero, an error
occurred. (A similar statement also applies to a few library functions.)

A common course of action after a failed system call is to print an error mes-
sage based on the _errno_ value. The _perror()_ and _strerror()_ library functions are provided for this purpose.
The _perror()_ function prints the string pointed to by its _msg_ argument, followed
by a message corresponding to the current value of _errno_.
```
#include <stdio.h>
void perror(const char*msg);
```

A simple way of handling errors from system calls would be as follows:
```
fd = open(pathname, flags, mode);
if(fd == -1){
	perror("open")
	exit(EXIT_FAILURE);
}
```
The _strerror()_ function returns the error string corresponding to the error number
given in its _errnum_ argument.
Returns pointer to error string corresponding to _errnum_
```
#include <string.h>
char *strerror(int errnum);
```

### **Handling errors from library functions**
The various library functions return different data types and different values to
indicate failure. (Check the manual page for each function.) For our purposes,
library functions can be divided into the following categories:
* Some library functions return error information in exactly the same way as system calls: a –1 return value, with _errno_ indicating the specific error. An example of such a function is remove(), which removes a file (using the _unlink()_ system
call) or a directory (using the _rmdir()_ system call). Errors from these functions
can be diagnosed in the same way as errors from system calls.
* Some library functions return a value other than -1 on error, but nevertheless set _errno_ to indicate the specific error condition. For example, _fopen()_ returns a NULL pointer on error, and the setting of _errno_ depends on which underlying system call failed. The _perror()_ and _strerror()_ functions can be used to diagnose these errors.
* Other library functions don't use _errno_ at all. The method for determining the existence and cause of errors depends on the particular function and is documented in the function's manual page. For these functions, it is a mistake to use _errno_, _perror()_ or _strerror()_ to diagnose errors.

## **3.5 Notes on the Example Programs in This Book**
In this section, we describe various conventions and features commonly employed
by the example programs presented in this book.

### **3.5.1 Command-Line Options and Arguments**
Many of the example programs in this book rely on command-line options and arguments to determine their behavior.
Traditional [[UNIX]] command-line options consist of an initial hyphen, a letter that identifies the option, and an optional argument. To parse these options, we use the standard _getopt()_ library function.
Each of our example programs that has a nontrivial command-line syntax provides a simple help facility for the user: if invoked with the _--help_ option, the program displays a 
usage message that indicates the syntax for command-line options and arguments.

### 3.5.2 Common Functions and Header Files
Most of the example programs include a header file containing commonly required definitions, and they also use a set of common functions. We discuss the header file and functions in this section.
#### Common header file
Listing 3-1 is the header file used by nearly every program in this book. This header
file includes various other header files used by many of the example programs,
defines a Boolean data type, and defines macros for calculating the minimum and
maximum of two numeric values. Using this header file allows us to make the example programs a bit shorter.

Listing 3-1: lib/tlpi_hdr.h
```
#pragma once
#include <sys/types.h>
#include <stdio.h>
#include <stdlib.h>

#include <unistd.h>
#include <errno.h>
#include <string.h>

#include "get_num.h"

#include "error_functions.h"

typedef enum { FALSE, TRUE } Boolean;
#define min(m,n) ((m) < (n) ? (m) : (n))
#define max(m,n) ((m) > (n) ? (m) : (n))
#endif
```

### Error-diagnostic functions
To simplify error handling in our example programs, we use the error-diagnostic
functions whose declarations are shown in Listing 3-2.

Listing 3-2: lib/error_functions.h
```
#pragma once

void errMsg(const char* format, ...);

#ifdef __GNUC__
/* This macro stops 'gcc -Wall' complaining that "control reaches
	end of non-void function" if we use the following functions to
	terminate main() or some other non-void function. */
	
#define NORETURN __attribute__ ((__noreturn__))
#else
#define NORETURN
#endif

void errExit(const char *format, ...) NORETURN ;

void err_exit(const char *format, ...) NORETURN ;

void errExitEN(int errnum, const char *format, ...) NORETURN ;

void fatal(const char *format, ...) NORETURN ;

void usageErr(const char *format, ...) NORETURN ;

void cmdLineErr(const char *format, ...) NORETURN ;

#endif
```
To diagnose errors from system calls and library functions, we use _errMsg()_, _errExit()_, _err_exit()_, and _errExitEN()_.

function definitions:
```
void errMsg(const char* format, ...);
void errExit(const char* format, ...);
void err_exit(const char* format, ...);
void errExitEN(int errnum, const char* format, ...);
```

The errMsg() function prints a message on standard error. Its argument list is the
same as for printf(), except that a terminating newline character is automatically
appended to the output string. The errMsg() function prints the error text corre-
sponding to the current value of errno—this consists of the error name, such as
EPERM, plus the error description as returned by strerror()—followed by the formatted
output specified in the argument list.
The errExit() function operates like errMsg(), but also terminates the program,
either by calling exit() or, if the environment variable EF_DUMPCORE is defined with a
nonempty string value, by calling abort() to produce a core dump file for use in
the debugger. (We explain core dump files in Section 22.1.)
The err_exit() function is similar to errExit(), but differs in two respects:
- It doesn’t flush standard output before printing the error message.
- It terminates the process by calling _exit() instead of exit(). This causes the process to terminate without flushing stdio buffers or invoking exit handlers.
The details of these differences in the operation of err_exit() will become clearer in
Chapter 25, where we describe the differences between \_exit() and exit(), and consider the treatment of stdio buffers and exit handlers in a child created by fork(). For
now, we simply note that err_exit() is especially useful if we write a library function
that creates a child process that needs to terminate because of an error. This termination should occur without flushing the child’s copy of the parent’s (i.e., the calling process’s) stdio buffers and without invoking exit handlers established by the
parent.
The errExitEN() function is the same as errExit(), except that instead of printing
the error text corresponding to the current value of errno, it prints the text corre-
sponding to the error number (thus, the EN suffix) given in the argument errnum.
Mainly, we use errExitEN() in programs that employ the POSIX threads API.
Unlike traditional UNIX system calls, which return –1 on error, the POSIX threads
functions diagnose an error by returning an error number (i.e., a positive number
of the type normally placed in errno) as their function result. (The POSIX threads
functions return 0 on success.)
We could diagnose errors from the POSIX threads functions using code such
as the following:
```
errno = pthread_create(&thread, NULL, func, &arg);
if (errno != 0)
	errExit("pthread_create");
```
However, this approach is inefficient because errno is defined in threaded pro-
grams as a macro that expands into a function call that returns a modifiable lvalue.
Thus, each use of errno results in a function call. The errExitEN() function allows us
to write a more efficient equivalent of the above code:
```
int s;
s = pthread_create(&thread, NULL, func, &arg);
if (s != 0)
	errExitEN(s, "pthread_create");
```
	In C terminology, an lvalue is an expression referring to a region of stor-
	age. The most common example of an lvalue is an identifier for a variable.
	Some operators also yield lvalues. For example, if p is a pointer to astorage
	area, then *p is an lvalue. Under the POSIX threads API, errno is redefined
	to be a function that returns a pointer to a thread-specific storage area
	(see Section 31.3).

To diagnose other types of errors, we use fatal(), usageErr(), and cmdLineErr().
function definitions:
```
void fatal(const char* format, ...);
void usageErr(const char* format, ...);
void cmdLineErr(const char* format, ...);
```
The _fatal()_ function is used to diagnose general errors, including errors from library functions that doesn't set _errno_. 