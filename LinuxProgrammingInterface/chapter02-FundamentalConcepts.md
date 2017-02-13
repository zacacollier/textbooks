### * 2.1 The Core Operating System: The Kernel *
The term _operating system_ commonly refers to _2_ connotations:
- *The whole shebang*: all system-critical resource management software, as well as all utilities and tools like CLI, GUI, file utils, text editors
- OR: the *central software* - core resource management and allocation

    ####_"kernel"_
refers to the second meaning
  - the kernel executable usually resides at `/boot/vmlinuz`
      - in early UNIX, it was called `unix`
      - later UNIX, which implemented _virtual memory_, renamed it `vmunix`
      - in Linux, the filename mirrors the system name - hence, the `z` indicating that the kernel is a compressed executable

  - common _kernel_ tasks :
      - **Process Scheduling** : Linux is a _preemptive multitasking_ operating system:
        - *multitasking* referring to *multiple processes* which can simultaneously reside in memory and each receive use of the CPU(s)
        - *preemptive* meaning there is a predetermined order to which certain processes receive use of the CPU, determined by the *kernel process scheduler*
      - **Memory management** : Linux employs *virtual memory management* - advantages are:
        - processes are *isolated from one another and from the kernel* - one process can't read or modify the memory of another, or of the kernel
        - only *part of a process* needs to be kept in memory - each process requires less memory, and more processes can be held in RAM simultaneously
      - **Provision of a file system** : the kernel provides a file system on disk, and allows files to be created and manipulated
      - **Creation and termination of processes** : the kernel can load a execute and terminate processes
      - **Access to devices** : the kernel provides programs with a common interface to standardize and simplify access to devices such as mice, monitors, keyboards, USB drives, etc
      - **Networking** : the kernel can send and receive network _packets_ on behalf of user processes
      - **Provision of a *system call API* ** : the kernel can receive requests to perform various tasks utilizing _kernel entry points_ called _system calls_.
        - this is what we're mostly concerned with in the text

  * kernel space and user space:
      - areas of *virtual memory* can be marked as user space or kernel space
      - kernel space is accessed in supervisor mode
      - user space exists to make sure that the underlying constructs of the kernel cannot be accessed in such a way as to adversely affect the operation of the system

  - _process view_ vs _kernel view_ :
    - **Processes execute asynchronously** - a process doesn't know when it will time out, where it lies in the execution stack, etc. This is because *the delivery of signals and the facilitation of interprocess communication events* are *mediated by the kernel* - all processes are ultimately governed by the whims of the kernel
    - **A process can't itself create a new process or terminate itself**
    - **Processes execute in isolation**
    - **A process can't directly communicate with the input / output devices attached to the computer**
    - **So when we say** 'a process can do _x_', we're really stating 'a process can request _that the kernel do x_'

#### 2.2 The Shell (eg `bash`, `ksh`, `zsh`)
  - a special purpose program designed to read commands and subsequently execute programs

    #### 2.3 _Users and Groups_
    - **Users** defined in `/etc/passwd`:
      - denoted by *Group ID*, *home directory* and *login shell*
    - **Groups** defined in `/etc/group`:
      - denoted by *Group ID*, *Group Name (unique)* and *users listed by login name*
    - **Superuser** aka `root`:
      - has *user ID 0*
    #### 2.4 _Single Directory Hierarchy, Directories, Links and Files_ - all files and directories are children of `/`
    - **File Types** - each file is marked with a type to denote *what kind of file it is* - ordinary data files are called *regular* or *plain* files to distinguish them accordingly
      - other file types include *devices, pipes, sockets, directories, and symlinks*
    - **Directories and Links (hard links)** - a *directory* is a special file whose contents take the form of a *table of filenames coupled with references to corresponding files*
      - the _filename plus reference_ association is called a *link*
        - every directory contains at least:
          - `.` - a link to the directory itself
          - `..` - a link to its parent directory
            - for the root directory, which has no parent `/..` simply equates to `/`
    - **Symbolic Links (soft links)** - a *specially marked file* which simply contains the name of another *target* file (which it shall link to)
      - when a pathname is specified in a system call, the kernel automatically *dereferences* each symlink in the pathname
    - **File naming** - should follow the SUSv3 *portable filename character set* `[-._a-zA-Z0-9]` to avoid conflicts
    - **File ownership and permissions** - each file has an associated user ID and group ID to define *the owner of the file* and the *group to which it belongs*
      - *permission bits* - categories are (OWNER)(GROUP)(OTHER)
        - within each category are 3 *bits* - *read* *write* and *execute*

    #### 2.5 _File I/O Model_
    - **Universality of I/O** - the same system calls (*open(), read(), write(), close(), etc*) are used to perform I/O on all types of files, including devices
    - **File descriptors** - a small non-negative integer, typically obtained by a call to `open()`, which takes a *pathname argument specifying a call upon which I/O is about to be performed*
      - *descriptor 0* is *standard input*
      - *descriptor 1* is *standard output* - the file to which the process writes its output
      - *descriptor 2* is *standard error* - the file to which the process writes error messages and warnings
    - **`stdio`** - the standard I/O functions contained in the C library.
    #### 2.6 _Programs_ - exist in 2 forms: human-readable source code and machine-language instructions
    - **Filters** - programs that read from `stdin`, perform some transformation, and write the mutated data to `stdout` (include `cat`, `grep`, `sort`, `sed`, `awk`)
    - **Command-line arguments** - C programs can access the *command-line arguments* with `main()` written as:
      `int main(int argc, char *argv[])`
      - `argc` contains the total number of command-line arguments
      - `argv` is an array of strings pointing to individual arguments
          `argv[0]` is the name of the program itself
      
