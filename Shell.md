# The Shell
#shell #commandinterpreter #loginshell #sh #bash #csh #ksh #zsh 
A shell is a special-purpose program designed to read commands typed by a user
and execute appropriate programs in response to those commands. Such a pro-
gram is sometimes known as a command interpreter.
The term login shell is used to denote the process that is created to run a shell
when the user first logs in.

Whereas on some operating systems the command interpreter is an integral
part of the kernel, on UNIX systems, the shell is a user process. Many different
shells exist, and different users (or, for that matter, a single user) on the same com-
puter can simultaneously use different shells. A number of important shells have
appeared over time:
* Bourne shell (sh): This is the oldest of the widely used shells, and was written by
Steve Bourne. It was the standard shell for Seventh Edition UNIX. The Bourne
shell contains many of the features familiar in all shells: I/O redirection, pipe-
lines, filename generation (globbing), variables, manipulation of environment
variables, command substitution, background command execution, and func-
tions. All later UNIX implementations include the Bourne shell in addition to
any other shells they might provide.
* C shell (csh): This shell was written by Bill Joy at the University of California at
Berkeley. The name derives from the resemblance of many of the flow-control
constructs of this shell to those of the C programming language. The C shell
provided several useful interactive features unavailable in the Bourne shell,
including command history, command-line editing, job control, and aliases.
The C shell was not backward compatible with the Bourne shell. Although the
standard interactive shell on BSD was the C shell, shell scripts (described in a
moment) were usually written for the Bourne shell, so as to be portable across
all UNIX implementations.
- Korn shell (ksh): This shell was written as the successor to the Bourne shell by
David Korn at AT&T Bell Laboratories. While maintaining backward compati-
bility with the Bourne shell, it also incorporated interactive features similar to
those provided by the C shell.
- Bourne again shell (bash): This shell is the GNU project’s reimplementation of
the Bourne shell. It supplies interactive features similar to those available
in the C and Korn shells. The principal authors of bash are Brian Fox and Chet
Ramey. Bash is probably the most widely used shell on Linux. (On Linux, the
Bourne shell, sh, is actually provided by bash emulating sh as closely as possible.)

>POSIX.2-1992 specified a standard for the shell that was based on the then cur-
rent version of the Korn shell. Nowadays, the Korn shell and bash both con-
form to POSIX, but provide a number of extensions to the standard, and many
of these extensions differ between the two shells.

The shells are designed not merely for interactive use, but also for the interpretation
of shell scripts, which are text files containing shell commands. For this purpose,
each of the shells has the facilities typically associated with programming lan-
guages: variables, loop and conditional statements, I/O commands, and functions.
Each of the shells performs similar tasks, albeit with variations in syntax. Unless
referring to the operation of a specific shell, we typically refer to “the shell,” with
the understanding that all shells operate in the manner described. Most of the
examples in this book that require a shell use bash, but, unless otherwise noted, the
reader can assume these examples work the same way in other Bourne-type shells.