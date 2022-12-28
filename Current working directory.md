# Current working directory
#pwd 
Each process has a current working directory (sometimes just referred to as the pro-
cess’s working directory or current directory). This is the process’s “current location”
within the single directory hierarchy, and it is from this directory that relative path-
names are interpreted for the process.
A process inherits its current working directory from its parent process. A
login shell has its initial current working directory set to the location named in the
home directory field of the user’s password file entry. The shell’s current working
directory can be changed with the cd command.