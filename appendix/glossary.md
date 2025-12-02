# Linux Glossary

> Alphabetical reference of common Linux terms and concepts.

---

## A

**Absolute Path**  
A complete path from the root directory (`/`) to a file or directory. Example: `/home/user/documents/file.txt`

**Alias**  
A shortcut for a command or series of commands. Created with `alias name='command'`.

**APT (Advanced Package Tool)**  
Package management system used by Debian-based distributions (Debian, Ubuntu, Kali).

**Argument**  
Information passed to a command or script. In `cp file1 file2`, both files are arguments.

---

## B

**Bash (Bourne Again Shell)**  
The default command-line shell on most Linux distributions. An enhanced version of the original Bourne Shell (sh).

**Binary**  
An executable program file compiled from source code. Stored in directories like `/bin`, `/usr/bin`.

**Boot**  
The process of starting up a computer and loading the operating system.

**Boot Loader**  
Software that loads the operating system kernel (e.g., GRUB).

---

## C

**CLI (Command Line Interface)**  
Text-based interface for interacting with the operating system using typed commands.

**Command**  
An instruction given to the shell to perform a specific task.

**Compiler**  
A program that translates source code into executable binary code.

**Configuration File (Config File)**  
A file containing settings for programs or the system. Often found in `/etc/` or as dotfiles in home directories.

**Cron**  
A time-based job scheduler in Linux. Uses crontab files to schedule recurring tasks.

**Crontab**  
The file or command used to manage cron jobs. Edit with `crontab -e`.

---

## D

**Daemon**  
A background process that runs continuously, typically providing services. Examples: `sshd`, `httpd`, `cron`.

**Debian**  
A popular Linux distribution known for stability. The basis for Ubuntu, Kali, and many others.

**Dependency**  
A package or library required by another package to function properly.

**Directory**  
A container for files and other directories (equivalent to "folder" in other operating systems).

**Distribution (Distro)**  
A version of Linux that includes the kernel plus additional software. Examples: Ubuntu, Fedora, Kali, Debian.

**DNS (Domain Name System)**  
System that translates domain names (google.com) into IP addresses.

**Dotfile**  
A hidden configuration file whose name starts with a period (.). Example: `.bashrc`, `.vimrc`.

---

## E

**Environment Variable**  
A named value available to processes, used for configuration. Example: `PATH`, `HOME`, `USER`.

**Executable**  
A file with execute permission (`x`) that can be run as a program.

**Exit Code (Return Code)**  
A number returned by a command indicating success (0) or failure (non-zero).

---

## F

**File Descriptor**  
A number representing an open file. Standard: 0=stdin, 1=stdout, 2=stderr.

**File System**  
The structure and organization of files on a storage device. Common types: ext4, xfs, btrfs.

**Flag**  
See "Option"

**FOSS (Free and Open Source Software)**  
Software that is freely available and whose source code can be viewed and modified.

**FHS (Filesystem Hierarchy Standard)**  
The standard directory structure for Linux systems.

---

## G

**GID (Group ID)**  
A unique number identifying a group. Stored in `/etc/group`.

**Glob (Globbing)**  
Pattern matching using wildcards (`*`, `?`, `[]`) to match filenames.

**GNU**  
"GNU's Not Unix" - The project that created many Linux utilities. Linux is often called GNU/Linux.

**Group**  
A collection of users. Used for managing permissions. Every file has an associated group.

**GRUB (Grand Unified Bootloader)**  
The most common boot loader for Linux systems.

**GUI (Graphical User Interface)**  
Visual interface with windows, icons, and mouse interaction.

---

## H

**Hard Link**  
A directory entry that points to the same inode as another file. Created with `ln file link`.

**Hash (Hashbang/Shebang)**  
The `#!` at the start of a script indicating which interpreter to use. Example: `#!/bin/bash`

**Home Directory**  
User's personal directory, typically `/home/username`. Represented by `~`.

**Host**  
A computer on a network.

**Hostname**  
The name assigned to a computer on a network.

---

## I

**Init System**  
The first process started by the kernel, responsible for starting other processes. Modern systems use `systemd`.

**Inode**  
A data structure storing metadata about a file (permissions, ownership, location) but not its name.

**Input**  
Data provided to a command or program, often through stdin or arguments.

**Interpreter**  
A program that executes scripts line by line (e.g., bash, python).

**IP Address**  
A numerical address identifying a device on a network. IPv4 (192.168.1.1) or IPv6.

---

## J

**Job**  
A process running in the shell, either in foreground or background. Managed with `jobs`, `fg`, `bg`.

**Journal**  
The systemd logging system, accessed via `journalctl`.

---

## K

**Kernel**  
The core of the operating system that manages hardware, memory, and processes.

**Kill**  
To terminate a process, using commands like `kill`, `killall`, or `pkill`.

---

## L

**Link**  
A reference to another file. Can be hard link or symbolic (soft) link.

**Linux**  
An open-source Unix-like operating system kernel, or the family of operating systems based on it.

**Localhost**  
The local computer, accessible via IP address 127.0.0.1.

**Log**  
A file recording system or application events. Usually stored in `/var/log/`.

---

## M

**Man Page (Manual Page)**  
Documentation for commands, accessed with `man command`.

**Mount**  
The process of making a filesystem accessible at a specific directory.

**Mount Point**  
The directory where a filesystem is attached.

---

## N

**Nano**  
A simple, beginner-friendly text editor.

**Network Interface**  
Hardware or virtual connection to a network (e.g., `eth0`, `wlan0`, `lo`).

---

## O

**Open Source**  
Software whose source code is publicly available for viewing and modification.

**Option (Flag)**  
A modifier that changes command behavior. Can be short (`-l`) or long (`--long`).

**Output**  
Data produced by a command, sent to stdout, stderr, or a file.

**Owner**  
The user who owns a file and has primary control over its permissions.

---

## P

**Package**  
A bundled collection of software files ready for installation.

**Package Manager**  
Software for installing, updating, and removing packages. Examples: `apt`, `yum`, `dnf`, `pacman`.

**Partition**  
A logical division of a storage device.

**Password Hash**  
An encrypted representation of a password, stored in `/etc/shadow`.

**Path**  
The location of a file or directory in the filesystem. Can be absolute or relative.

**PATH**  
An environment variable listing directories searched for executable commands.

**Permission**  
Access rights controlling who can read, write, or execute a file.

**PID (Process ID)**  
A unique number identifying a running process.

**Pipe (|)**  
Connects the output of one command to the input of another. Example: `ls | grep txt`

**Port**  
A numbered endpoint for network communication. Example: 22 (SSH), 80 (HTTP), 443 (HTTPS).

**Process**  
A running instance of a program.

**Prompt**  
The text displayed by the shell indicating it's ready for input. Example: `user@host:~$`

---

## R

**Redirection**  
Sending command output to a file (`>`) or file contents to a command (`<`).

**Regular Expression (Regex)**  
A pattern for matching text, used with tools like `grep`, `sed`, `awk`.

**Relative Path**  
A path relative to the current directory. Example: `./scripts/run.sh` or `../parent/file`

**Repository (Repo)**  
A server or location storing software packages for installation.

**Root**  
1. The superuser account with full system access (UID 0)
2. The top-level directory `/` (root directory)

**Root Directory**  
The top of the filesystem hierarchy, represented by `/`.

---

## S

**Script**  
A text file containing commands to be executed by an interpreter.

**Service**  
A program running in the background providing functionality. Managed with `systemctl`.

**Shell**  
A command interpreter that processes user commands. Examples: bash, zsh, fish.

**Shell Script**  
A script written for execution by a shell (usually bash).

**SIGHUP, SIGKILL, SIGTERM**  
Signals sent to processes. SIGTERM (15) requests termination, SIGKILL (9) forces it.

**SSH (Secure Shell)**  
Protocol for secure remote login and command execution.

**Standard Error (stderr)**  
The output stream for error messages (file descriptor 2).

**Standard Input (stdin)**  
The input stream for commands (file descriptor 0).

**Standard Output (stdout)**  
The output stream for normal output (file descriptor 1).

**Sticky Bit**  
Special permission on directories preventing users from deleting others' files.

**String**  
A sequence of characters (text).

**Subdirectory**  
A directory contained within another directory.

**Sudo (Superuser Do)**  
Command to execute other commands with root privileges.

**SUID (Set User ID)**  
Special permission that runs a program as its owner, not the user executing it.

**Symbolic Link (Symlink)**  
A file that points to another file or directory. Created with `ln -s target link`.

**Syntax**  
The rules for structuring commands correctly.

**Systemd**  
The modern init system and service manager used by most Linux distributions.

---

## T

**Tarball**  
An archive file created with `tar`, often compressed. Common extensions: `.tar`, `.tar.gz`, `.tgz`.

**Terminal**  
An application providing access to the shell. Also called terminal emulator.

**Text Editor**  
A program for editing plain text files. Examples: nano, vim, emacs.

**TTY**  
A terminal device (originally "TeleTYpewriter"). Virtual consoles are accessed with Ctrl+Alt+F1-F6.

---

## U

**UID (User ID)**  
A unique number identifying a user. Root is UID 0.

**Umask**  
A value determining default permissions for new files and directories.

**Unix**  
The operating system family that inspired Linux. Linux is "Unix-like."

**User**  
An account for logging into and using the system. Information stored in `/etc/passwd`.

**User Space**  
The portion of memory where user programs run, separate from the kernel.

**UTC (Coordinated Universal Time)**  
The primary time standard used for system clocks.

---

## V

**Variable**  
A named storage location for data. In bash: `NAME="value"` and `$NAME` to access.

**Vim**  
A powerful, modal text editor. Improved version of vi.

**Virtual Console**  
Text-based login sessions accessible via Ctrl+Alt+F1-F6.

**Virtual Machine (VM)**  
A software-emulated computer running within another operating system.

---

## W

**Wildcard**  
Characters used for pattern matching: `*` (any characters), `?` (single character), `[]` (character set).

**Working Directory**  
The current directory. Shown with `pwd`, changed with `cd`.

---

## X

**X Window System (X11)**  
The graphical windowing system underlying most Linux desktop environments.

---

## Z

**Zombie Process**  
A terminated process whose entry remains in the process table because its parent hasn't collected its exit status.

**Zsh**  
Z Shell - an extended shell with additional features over bash.

---

*Reference: Linux 101 Course - All Modules*