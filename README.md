# Linux_Notes
Some notes about Linux

## Linux Fundamentals

### What is Linux and What is it Doing?
The term "Linux" is often used to refer to the entire OS, but in
reality, Linux is the OS kernel, which is started by the boot
loader, which is itself started by the BIOS/UEFI. The kernel assumes
a role similar to that of a conductor in an orchestra--it ensures
coordination between hardware and software. This role includes
managing hardware, processes, users, permissions, and the file
system. The kernel provides a common base to all other programs
on the system and typically runs in *ring zero*, also known as
*kernel space*.

**The User Space**

We use the term *user* to lump together everything that happens
outside of the kernel. Among the programs running in user space
are many core utilities from the GNU project (http://www.gnu.org),
most of which are meant to be run from the command line. You can use
them in scripts to automate many tasks.

**Let's quickly review the various tasks handled by the Linux
Kernel**

1. Driving Hardware
  
The kernel is tasked, first and foremost, with controlling the
computer's hardware components. It detects and configures them
when the computer powers on, or when a device is inserted or
removed (for example, a USB device). It also makes them available
to higher-level software, through a simplified programming 
interface, so applications can take advantage of devices without
having to address details such as which extension slot an option
board is plugged into. The programming interface also provides an
abstraction layer; this allows video-conferencing software, for
example, to use a webcam regardless of its maker and model. The
software can use the *Video for Linux* (V4L) interface and the
kernel will translate function calls of the interface into actual
hardware commands needed by the specific webcam in use.

The kernel exports data about detected hardware through the
`/proc/` and `/sys/` virtual file systems. Applications often access
devices by way of files created within `/dev/`. Specific files
represent disk dives (for instance, `/dev/sda`), partitions
(`/dev/sda1`), mice (`/dev/input/mouse0`), keyboards 
(`/dev/input/event0`), sound cards (`/dev/snd/*`), serial ports
(`/dev/ttyS*`), and other components.

There are two types of device files: *block* and *character*. The
former has characteristics of a block of data: It has a finite size,
so you can access bytes at any position in the block. The latter
behaves like a flow of characters. You can read and write
characters, but you cannot seek to a given position and change
arbitrary bytes. To find out the type of a given device file,
inspect the first letter in the output of `ls -l`. It is either
`b`, for block devices, or `c`, for character devices:

```bash
$ ls -l /dev/sda /dev/ttyS0
brw-rw---- 1 root disk 8, 0 Mar 21 08:44 /dev/sda
crw-rw---- 1 root dialout 4, 64 Mar 30 08:59 /dev/ttyS0
```

As you might expect, disk drives and partitions use block devices,
whereas mouse, keyboard, and serial ports use character devices. In
both cases, the programming interface includes device-specific
commands that can be invoked through the `ioctl` system call.

2. Unifying File Systems

File systems are a prominent aspect of the kernel. Unix-like
systems merge all the file stores in a single hierarchy, which
allows users and applications to access data by knowing its
location within that hierarchy.

The starting point of this hierarchical tree is called the root,
represented by the "/" character. This directory can contain
named subdirectories. For instance, the home subdirectory of `/`
is called `/home/`. This subdirectory can, in turn, contain other
subdirectories, and so on. Each directory can also contain files,
where the data will be stored. The kernel translates between this
naming system and the storage location on a disk.

Unlike other systems, Linux possesses only one such hierarchy, and
it can integrate data from several disks. One of these disks
becomes the root, and the others are *mounted* on directories in
the hierarchy (the Linux command is called `mount`). These other
disks are then available under the *mount points*. This allows
storing users' home directories (tradionally stored within 
`/home/`) on a separate hard disk, which will contain the buxy
directroy (along with home directories of other users). Once you
mount the disk on `/home/`, these directories become accessible at
their usual locations, and paths such as 
`/home/buxy/Desktop/hello.txt` keep working.

There are many file systems formats, corresponding to many ways of
physically storing data on disks. The most widely known are *ext2*,
*ext3*, and *ext4*, but others exist. For instance, *VFAT* is the
filesystem that was historically used by DOS and Windows operating
systems. Linux's support for VFAT allows hard disks to be
accessible under Kali as well as under Windows. In any case, you
must prepare a file system on a disk before you can mount it and
this operation is known as *formatting*.

Commands such as `mkfs.ext3` (where `mkfs` stands for 
*MaKe FileSystem*) handle formatting. These commands require, as a
parameter, a device file representing the partition to be formatted
(for instance, `/dev/sda1`, the first partition on the first 
drive). This operation is destructive and should be rnu only once,
unless you want to wipe a filesystem and start fresh.

There are also network filesystems such as NFS, which do not store
data on a local disk. Instead, data is transmitted through the
network to a server that stores and retrieves them on demand.
Thanks to the file system abstraction, you don't have to worry
about how this disk is connected, since the files remain accessible
in their usual hierarchical way.

3. Managing Processes

A process is a running instance of a program, which requires
memory to store both the program itself and its operating data.
The kernel is in charge of creating and tracking processes. When a
program runs, the kernel first sets aside some memory, loads the
executable code from the file system into it, and then starts the
code running. It keeps information about this process, the most
visible of whih is an identification number known as the *process
identifier* (PID).

Like most modern operating systems, those with Unix-like kernels,
including Linux, are capable of multi-tasking. In other words, they
allow the system to run many processes at the same time. There is
actually only one running process at any one time, but the kernel
divides CPU time into small slices and runs each process in turn.
Since these time slices very short (in the millisecond range),
they create the appearance of processes running in parallel,
although they are active only during their time interval and idle
the rest of the time. The kernel's job is to adjust its scheduling
mechanisms to keep that appearance, while maximizing global system
performance. If the time slices are too long, the application may
not appear as responsive as desired. Too short, and the system
loses time by switching tasks too frequently. These decisions can
be refined with process priorities, where high-priority processes
will run for longer periods and with more frequent time slices than
low-priority processes.

Multi-Processor Systems (and Variants)

The limitation described above, of only one process running at a
time, doesn't always apply: the actual restriction is that there
can be only one runnin process *per processor core*. 
Multi-processor, multi-core, or *hyper-threaded* systems allow
several processes to run in parallel. The same time-slicing
system is used, though, to handle cases where there are more active
processes than available processor cores. This is not unusual; a
basic system, even a mostly idle one, almost always has tens of
running processes.

The kernel allows several independent instances of the same program
to run, but each is allowed access only to its own time slices and
memory. Their data thus remains independent.

4. Rights Management

Unix-like systems support multiple users and groups and allow
control of permissions. Most of the time, a process is identified
by the user who started it. That process is only permitted to
take actions permitted for its owner. For instance, opening a file
requires the kernl to check the process identity against access
permissions.

### How to get a command line
































