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

File systems are a prominent aspect of the kernel. 

