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

## Linux Commands
* `pstree -T` - shows a nice and clean overview of processes running on your system
* `mv /path/to/something/{initialFileName, fileNameToRenameTo}` - rename file without
  typing the same file path twice. This shell feature is called Pathname expansion and
  works with most of the shell commands in a similar manner.
* `curl -o outputFile.iso file:///home/username/largeFile.iso` - in case you are copying
  large files and would like to see progress, you can use `curl`.
* If you need to open ports on some system -- probably the best tool for that is
  `nmap`. If this is not available, you can try using `nc` (netcat). Ex:
  * `nmap -p 1-65535 my.hostname.com` - scan whole ports range, can also be a comma
    seperated list, 
  * `nc -zvw3 my.hostname.com 587` - check if port 587 is open
  * `netstat -tlpn` - list open local ports

### curl - transfer a URL
`curl` is a tool to transfer data from or to a server, using one of the supported
protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS,
POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET, and TFTP). The
command is designed to work without user interaction.

`curl` offers a busload 
