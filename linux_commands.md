# Linux Commands

## Touchpad not working

If touchpad does not work in kali, run the following
```bash
rmmod psmouse
modprobe psmouse proto=auto
```

## Shell Shortcuts
* `CTRL + A / CTRL + E` - move the cursor to the start/end of the line
* `CTRL + U` - delete all characters before the cursor (a great alternative
  to spamming backspace whenever you mistype your password).
* `CTRL + R` - search through your command history
* `CTRL + Z` - suspend the current app and return to the terminal shell. It
  is useful when editing files with Vim. To resume an app, type `fg`.
* `ALT + .` - inserts the last argument from the previous command. The other
  way is to use the variable `$_` in your command. ex: `mkdir test && cd $_`
* `ESCAPE + ENTER` - in `zsh` will insert a new line instead of running a command

## Help with commands
To see documentation for commands `man command_name`. If you want documentation for
a built-in command, do `man zshbuiltins`

## Linux Commands
* `pstree -T` - shows a nice and clean overview of processes running on your system
* `ps` - process status
* `mv /path/to/something/{initialFileName, fileNameToRenameTo}` - rename file without
  typing the same file path twice. This shell feature is called Pathname expansion and
  works with most of the shell commands in a similar manner.
* `screencapture` - capture images from the screen and save them to a file or the clipboard
* `say` - convert text to audible speech
* `open` - open files or directories
* `echo` - write arguments to the standard output
* `cat` - concatenate and print files
* `touch` - change file access and modification times
* `file` - determine file type
* `pwd` - return working directory name
* `rmdir` - remove a directory
* `nano` - Nano's ANOther directory, an enchanced free Pico clone
* `du` - display disk usage stats
* `find` - walk a file hierarchy
* `ls` - list directory contents
* `sort` - sort or merge records (lines) of text and binary files
* `strings` - find the printable strings in an object, or other binary, file
* `uniq` - report or filter out repeated lines in a file
* `base64` - encode and decode using Base64 representation
* `tr` - translate characters
* `cp` - copy files
* `mv` - move files
* `mkdir` - make directories
* `xxd` - make a hex dump or do the reverse
* `rm` - remove directory entries
* `gzip` - compression/decompression tool using Lempel-Ziv coding (LZ77)
* `bzip2`, `bunzip2` - a block sorting file compressor
* `bzcat` - decompresses files to stdout
* `bzip2recover` - recovers data from damaged bzip2 files
* `tar` - manipulate tape archives
* `crontab` - maintain crontab files for individual users (v3)
* `whoami` - display effective user id
* `more` / `less`
* `id` - return user id
* `su` - substitute user identity
* `chpass`, `chfn`, `chsh` - add or change user database info
* `chown` - change file owner and group
* `hexdump` - ASCII, decimal, hexadecimal, octal dump
* `pcalc` - programmer's calculator
* `seq` - prints sequences of numbers
* `lsof` - list open files, files can be anything
  * `-i` - selects the listing of files any of whose Internet address matches the address specified in `i`.
    If no address is specified, this option selects the listing of all Internet and x.25 (HP-UX) network
    files.
    * `[46][protocol][@hostname|hostaddr][:service|port]`
      * `46` - specifies the IP version, IPv4 or IPv6 that applies to the following address. '6' may be
        specified only if the UNIX dialect supports IPv6. If neither '4' nor '6' is specified, the following
        address applies to all IP versions.
      * `protocol` - is a protocol name, TCP, UDP
      * `hostname` - is an Internet host name. Unless a specific IP version is specified, open network files
        associated with host names of all versions will be selected.
      * `hostaddr` - is a numeric Internet IPv4 address in dot form; or an IPv6 numeric address in colon
        form, enclosed in brackets, if the UNIX dialect supports IPv6. When an IP version is selected,
        only its numeric addresses may be specified
      * `service` - is an `/etc/services` name, e.g. `smtp` or a list of them
      * `port` - port number or a list of them
  * to check processes on a port you can do `lsof -i :8080`
* `kill` - terminate or signal a processs
  * to kill a process `kill -9 PID`
* `bg` - put each specified job in the background, or the current job if none is specified.
* `jobs` - lists info about each given job, or all jobs if job is omitted.
* `top` - display sorted information about processes
* `grep`, `egrep`, `fgrep`, `zgrep`, `zegrep`, `zfgrep` - file pattern searcher
* `useradd` - create a new user or update default new user information 
* `tee` - pipe fitting, copies standard input to standard output, making a copy in
  0 or more files. ex: `ls /home/user | tee my_directories.txt`
* `ln`, `link` - make links. The `ln` utility creates a new directory entry (linked file) which has the
  same modes as the original file.

### Symbols
* `>` - used to send information somewhere, ex: `echo hello world > file1.txt`
* `<` - will insert information somewhere, ex: `tr '[A-Z]''[a-z]' < file1.txt > file2.txt`
* `>>` - appends info to the end of a file, or creates a new if it doesn't exist
* `<<_ur_word` - used for shell scripting. The command allows u to type anything until you
  type ur word `ur_word` and it terminates and takes in input
* `2>` - redirects error output
* `&>` - redirects standard output and error output to a specific location
* `|` - allows output of 1 command to be sent to another
* `;` - add at the end of command to append another command

## Linux commands (network purposes)
* `curl -o outputFile.iso file:///home/username/largeFile.iso` - in case you are copying
  large files and would like to see progress, you can use `curl`.
* `nmap` - designed to allow system admins and curious individuals to scan large networks to
  determine which hosts are up and what services they are offering.
  * to scan a range of ports ex: `nmap -p 2220-2500 localhost`
* `nc` - arbitrary TCP and UDP connections and listens
* `netstat` - show network status
* If you need to open ports on some system -- probably the best tool for that is
  `nmap`. If this is not available, you can try using `nc` (netcat). Ex:
  * `nmap -p 1-65535 my.hostname.com` - scan whole ports range, can also be a comma
    seperated list, 
  * `nc -zvw3 my.hostname.com 587` - check if port 587 is open
  * `netstat -tlpn` - list open local ports
* `ifconfig` - configure network interface parameters
* `ping` - send ICMP ECHO_REQUEST packets to network hosts
* `host` - DNS lookup utility
* `ssh` - OpenSSH SSH client (remote login program)
* `ssh-keygen` - authentication key generation, management and conversion
  * when a deadlock between hosts happen in ssh, you must remove that host
    `ssh-keygen -R _host_`. If it says it doesn't exist, `cat` the file,
    and try to find the exact host name.
* `telnet` - used for interactive communication with another host using the TELNET protocol
* `openssl` - OpenSSL command line tool
* `s_client` - this command implements a generic SSL/TLS client which connects to a remote
  host using SSL/TLS. It is a very useful diagnostic tool for SSL servers.
  * might need this flag if it doesn't work `-ign_eof`
  * ex: `openssl s_client -connect localhost:30001 -ign_eof`
* `wget` - the non-interactive network downloader.


### curl - transfer a URL
`curl` is a tool to transfer data from or to a server, using one of the supported
protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS,
POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET, and TFTP). The
command is designed to work without user interaction.

`curl` offers a busload 
