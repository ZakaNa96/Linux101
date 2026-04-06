# Linux 101: A Complete Beginner's Course for System Administration Apprentices

Welcome to Linux 101! This course is designed for apprentices (Azubis) who are starting their journey into Linux system administration with zero prior Linux knowledge. By the end of this course, you will have a solid foundation in Linux fundamentals and be ready to tackle real-world system administration tasks.

## Course Overview

| | |
|---|---|
| **Target Audience** | System Administration apprentices with no Linux experience |
| **Prerequisites** | Basic computer knowledge, Kali Linux installed in VirtualBox |
| **Total Duration** | Approximately 48-60 hours |
| **Format** | Self-paced with hands-on exercises |
| **Language** | English (German-friendly) |

## Learning Environment

You will use **Kali Linux** running in a **VirtualBox** virtual machine. This setup allows you to practice safely without affecting your main computer. Feel free to experiment – if something breaks, you can always restore the VM!

---

## Course Structure

```
┌─────────────────────────────────────────────────────────────────────┐
│                        LINUX 101 CURRICULUM                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Module 1: Introduction to Linux & Terminal                         │
│      ↓                                                              │
│  Module 2: Navigating the File System                               │
│      ↓                                                              │
│  Module 3: Working with Files and Directories                       │
│      ↓                                                              │
│  Module 4: Viewing and Editing Text                                 │
│      ↓                                                              │
│  Module 5: File Permissions and Ownership                           │
│      ↓                                                              │
│  Module 6: User and Group Management                                │
│      ↓                                                              │
│  Module 7: Package Management with APT                              │
│      ↓                                                              │
│  Module 8: Process and Service Management                           │
│      ↓                                                              │
│  Module 9: Basic Networking                                         │
│      ↓                                                              │
│  Module 10: Shell Scripting and System Administration               │
│      ↓                                                              │
│  Module 11: System Monitoring and Log Analysis                      │
│      ↓                                                              │
│  Module 12: Backup, Recovery, and Basic Hardening                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Module Details

### Module 1: Introduction to Linux & Terminal

> **Your first steps into the Linux world**

| | |
|---|---|
| **Duration** | 3-4 hours |
| **Prerequisites** | VirtualBox with Kali Linux installed |
| **Difficulty** | ⭐ Beginner |

#### Learning Objectives

By the end of this module, you will be able to:
- Explain what Linux is and why it is important for system administration
- Open and use the terminal (command line interface)
- Execute basic commands and understand their output
- Use the manual pages (man) to get help
- Understand the difference between GUI and CLI

#### Key Concepts

1. **What is Linux?**
   - History and philosophy of Linux
   - Linux distributions (Debian, Ubuntu, Kali, etc.)
   - Why Linux is used in servers and security

2. **The Terminal**
   - What is a shell (Bash)
   - Terminal vs. GUI (Graphical User Interface)
   - Command prompt and its meaning

3. **First Commands**
   - `whoami` - Display current user
   - `hostname` - Show computer name
   - `date` - Display current date and time
   - `cal` - Show calendar
   - `clear` - Clear the terminal screen
   - `echo` - Print text to screen

4. **Getting Help**
   - `man` - Manual pages
   - `--help` flag
   - `apropos` - Search for commands

#### Practical Exercise: Your First Terminal Session

**Scenario:** Your supervisor asks you to verify the basic configuration of a new Linux server.

**Tasks:**
1. Open the terminal in Kali Linux
2. Find out which user you are logged in as
3. Display the hostname of the system
4. Check the current date and time
5. Use the manual to learn about the `uptime` command
6. Find out how long the system has been running

---

### Module 2: Navigating the File System

> **Finding your way around Linux**

| | |
|---|---|
| **Duration** | 3-4 hours |
| **Prerequisites** | Module 1 completed |
| **Difficulty** | ⭐ Beginner |

#### Learning Objectives

By the end of this module, you will be able to:
- Explain the Linux file system hierarchy
- Navigate between directories using the terminal
- Understand absolute and relative paths
- List and examine directory contents
- Identify important system directories

#### Key Concepts

1. **File System Hierarchy**
   - Root directory (`/`)
   - Important directories: `/home`, `/etc`, `/var`, `/usr`, `/tmp`, `/bin`, `/sbin`
   - Everything is a file in Linux

2. **Navigation Commands**
   - `pwd` - Print working directory
   - `cd` - Change directory
   - `ls` - List directory contents
   - `ls -l` - Long format listing
   - `ls -a` - Show hidden files
   - `ls -la` - Combined options

3. **Path Types**
   - Absolute paths (starting with `/`)
   - Relative paths (starting from current location)
   - Special directories: `.` (current), `..` (parent), `~` (home)

4. **Exploring Directories**
   - `tree` - Display directory tree
   - Tab completion for faster navigation

#### Practical Exercise: Directory Explorer

**Scenario:** A colleague needs help finding configuration files on a Linux server.

**Tasks:**
1. Navigate to the root directory and list its contents
2. Explore the `/etc` directory and find configuration files
3. Find your home directory using both absolute and relative paths
4. List all files (including hidden ones) in your home directory
5. Create a "map" (on paper or in a text file) of the directories you explored

---

### Module 3: Working with Files and Directories

> **Creating, copying, moving, and deleting**

| | |
|---|---|
| **Duration** | 4-5 hours |
| **Prerequisites** | Module 2 completed |
| **Difficulty** | ⭐⭐ Beginner-Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- Create files and directories
- Copy, move, and rename files and directories
- Delete files and directories safely
- Use wildcards to work with multiple files
- Understand file types and extensions

#### Key Concepts

1. **Creating Files and Directories**
   - `touch` - Create empty files or update timestamps
   - `mkdir` - Create directories
   - `mkdir -p` - Create nested directories

2. **Copying and Moving**
   - `cp` - Copy files
   - `cp -r` - Copy directories recursively
   - `mv` - Move or rename files and directories

3. **Deleting Files and Directories**
   - `rm` - Remove files
   - `rm -r` - Remove directories recursively
   - `rm -i` - Interactive mode (ask before deleting)
   - `rmdir` - Remove empty directories
   - ⚠️ **Warning:** There is no recycle bin in Linux!

4. **Wildcards (Globbing)**
   - `*` - Match any characters
   - `?` - Match single character
   - `[abc]` - Match one of the listed characters
   - Examples: `*.txt`, `file?.log`, `[abc]*`

5. **Additional Commands**
   - `file` - Determine file type
   - `stat` - Display file information
   - `ln` - Create links (symbolic and hard)

#### Practical Exercise: File Organization

**Scenario:** You need to organize a messy project folder for a new team member.

**Tasks:**
1. Create a directory structure: `project/docs`, `project/scripts`, `project/config`
2. Create several test files with different extensions
3. Copy all `.txt` files to the `docs` folder
4. Move all `.sh` files to the `scripts` folder
5. Rename a file to follow naming conventions
6. Delete test files you no longer need (carefully!)
7. Create a symbolic link to an important configuration file

---

### Module 4: Viewing and Editing Text

> **Reading and modifying files**

| | |
|---|---|
| **Duration** | 4-5 hours |
| **Prerequisites** | Module 3 completed |
| **Difficulty** | ⭐⭐ Beginner-Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- View file contents using various commands
- Search for text within files
- Edit files using the nano editor
- Understand when to use which viewing tool
- Combine commands to process text efficiently

#### Key Concepts

1. **Viewing File Contents**
   - `cat` - Display entire file
   - `less` - View files page by page (scroll with arrows, quit with `q`)
   - `more` - Older pager (similar to less)
   - `head` - Show first lines of a file
   - `tail` - Show last lines of a file
   - `tail -f` - Follow file changes in real-time

2. **Searching in Files**
   - `grep` - Search for patterns
   - `grep -i` - Case-insensitive search
   - `grep -r` - Recursive search in directories
   - `grep -n` - Show line numbers

3. **Text Editor: nano**
   - Opening files with `nano filename`
   - Navigation within nano
   - Saving files (`Ctrl+O`)
   - Exiting (`Ctrl+X`)
   - Cut, copy, paste operations
   - Search function (`Ctrl+W`)

4. **Text Processing Basics**
   - `wc` - Count words, lines, characters
   - `sort` - Sort lines
   - `uniq` - Remove duplicate lines
   - `diff` - Compare files

#### Practical Exercise: Log File Analysis

**Scenario:** Your supervisor asks you to analyze log files and create a summary report.

**Tasks:**
1. View the contents of `/var/log/syslog` (or another log file)
2. Display only the last 20 lines
3. Search for error messages using grep
4. Use `tail -f` to monitor the log file in real-time
5. Create a new file with nano and write a summary of what you found
6. Count how many times a specific word appears in the log

---

### Module 5: File Permissions and Ownership

> **Who can do what with which files**

| | |
|---|---|
| **Duration** | 5-6 hours |
| **Prerequisites** | Modules 1-4 completed |
| **Difficulty** | ⭐⭐⭐ Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- Read and understand Linux file permissions
- Modify permissions using symbolic and numeric (octal) notation
- Change file ownership
- Explain why permissions are important for security
- Apply appropriate permissions for common scenarios

#### Key Concepts

1. **Understanding Permissions**
   - Three permission types: Read (r), Write (w), Execute (x)
   - Three user categories: Owner (u), Group (g), Others (o)
   - Reading permission strings: `-rwxr-xr--`

2. **Viewing Permissions**
   - `ls -l` - Long listing shows permissions
   - Understanding the output: type, permissions, links, owner, group, size, date, name

3. **Changing Permissions with chmod**
   - Symbolic notation: `chmod u+x file`, `chmod go-w file`
   - Numeric (octal) notation: `chmod 755 file`, `chmod 644 file`
   - Common permission values:
     - `755` - rwxr-xr-x (standard for directories/scripts)
     - `644` - rw-r--r-- (standard for files)
     - `700` - rwx------ (private)
     - `777` - rwxrwxrwx (⚠️ avoid, security risk!)

4. **Changing Ownership**
   - `chown` - Change file owner
   - `chown user:group file` - Change owner and group
   - `chgrp` - Change group only

5. **Special Permissions**
   - SUID (Set User ID)
   - SGID (Set Group ID)
   - Sticky bit (for shared directories)

#### Practical Exercise: Securing a Shared Project

**Scenario:** Your team needs a shared folder where everyone can read files, but only certain users can modify them.

**Tasks:**
1. Create a test file and examine its default permissions
2. Create a script and make it executable
3. Set up a shared directory with appropriate permissions
4. Practice converting between symbolic and numeric notation
5. Change ownership of files to a different user
6. Document which permissions you would use for:
   - A private configuration file with passwords
   - A public script that anyone can run
   - A shared directory for a team

---

### Module 6: User and Group Management

> **Managing who can access the system**

| | |
|---|---|
| **Duration** | 5-6 hours |
| **Prerequisites** | Module 5 completed |
| **Difficulty** | ⭐⭐⭐ Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- Create, modify, and delete user accounts
- Manage groups and group membership
- Understand important user/group files
- Use sudo for administrative tasks
- Implement basic user security practices

#### Key Concepts

1. **Understanding Users**
   - User accounts and UIDs
   - System users vs. regular users
   - The root user (superuser)
   - `/etc/passwd` - User information
   - `/etc/shadow` - Password information

2. **Managing Users**
   - `useradd` - Create new user
   - `usermod` - Modify user account
   - `userdel` - Delete user account
   - `passwd` - Change password
   - `id` - Display user/group information

3. **Understanding Groups**
   - Groups and GIDs
   - Primary vs. secondary groups
   - `/etc/group` - Group information

4. **Managing Groups**
   - `groupadd` - Create new group
   - `groupmod` - Modify group
   - `groupdel` - Delete group
   - `gpasswd` - Administer groups
   - Adding users to groups: `usermod -aG group user`

5. **Sudo and Administrative Access**
   - What is `sudo`
   - `/etc/sudoers` file
   - The `wheel` or `sudo` group
   - Best practices for sudo usage

#### Practical Exercise: New Employee Setup

**Scenario:** A new colleague joins your team and needs a properly configured user account.

**Tasks:**
1. Create a new user account with a home directory
2. Set a secure password for the user
3. Create a group for your team
4. Add the new user to the team group
5. Grant the user sudo privileges (carefully!)
6. Verify the user can log in and access necessary resources
7. Document the steps for your team's procedures handbook

---

### Module 7: Package Management with APT

> **Installing and managing software**

| | |
|---|---|
| **Duration** | 3-4 hours |
| **Prerequisites** | Module 6 completed |
| **Difficulty** | ⭐⭐ Beginner-Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- Explain how package management works in Debian/Ubuntu/Kali
- Update the system and package lists
- Search for, install, and remove software
- Manage package repositories
- Handle package dependencies

#### Key Concepts

1. **Package Management Basics**
   - What are packages?
   - Package managers: APT, dpkg
   - Repositories and sources

2. **APT Commands**
   - `apt update` - Update package lists
   - `apt upgrade` - Upgrade installed packages
   - `apt install` - Install packages
   - `apt remove` - Remove packages
   - `apt purge` - Remove packages with configuration
   - `apt search` - Search for packages
   - `apt show` - Display package information
   - `apt list --installed` - List installed packages

3. **Repository Management**
   - `/etc/apt/sources.list`
   - Adding and removing repositories
   - PPA (Personal Package Archives) concept

4. **Dpkg - Lower Level Tool**
   - `dpkg -i` - Install local .deb files
   - `dpkg -l` - List installed packages
   - `dpkg -r` - Remove packages
   - When to use dpkg vs. apt

5. **Troubleshooting**
   - Fixing broken packages: `apt --fix-broken install`
   - Clearing cache: `apt clean`, `apt autoclean`
   - Removing unused packages: `apt autoremove`

#### Practical Exercise: System Maintenance

**Scenario:** You need to update a system and install software requested by a colleague.

**Tasks:**
1. Update the package lists and upgrade all packages
2. Search for and install a useful tool (e.g., `htop`, `tree`, `neofetch`)
3. Display information about an installed package
4. Remove a package you no longer need
5. Clean up unused packages and cache
6. Check how much disk space was saved

---

### Module 8: Process and Service Management

> **Controlling what runs on your system**

| | |
|---|---|
| **Duration** | 5-6 hours |
| **Prerequisites** | Module 7 completed |
| **Difficulty** | ⭐⭐⭐ Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- View and understand running processes
- Start, stop, and manage processes
- Control system services with systemctl
- Monitor system resources
- Troubleshoot process-related issues

#### Key Concepts

1. **Understanding Processes**
   - What is a process?
   - Process IDs (PIDs)
   - Parent and child processes
   - Process states

2. **Viewing Processes**
   - `ps` - Snapshot of processes
   - `ps aux` - Detailed process list
   - `top` - Real-time process monitor
   - `htop` - Enhanced process monitor (install first)
   - `pgrep` - Find process by name

3. **Managing Processes**
   - `kill` - Send signals to processes
   - `kill -9` - Force kill (SIGKILL)
   - `killall` - Kill processes by name
   - `pkill` - Kill processes by pattern
   - Foreground and background processes (`&`, `fg`, `bg`, `jobs`)
   - `nohup` - Keep processes running after logout

4. **System Services**
   - Init systems: systemd
   - `systemctl status` - Check service status
   - `systemctl start/stop` - Start/stop services
   - `systemctl enable/disable` - Enable/disable at boot
   - `systemctl restart/reload` - Restart services

5. **System Monitoring**
   - `uptime` - System uptime and load
   - `free` - Memory usage
   - `df` - Disk space usage
   - `du` - Directory space usage

#### Practical Exercise: Process Troubleshooting

**Scenario:** A server is running slowly and you need to find and fix the problem.

**Tasks:**
1. Check system uptime and load average
2. Use `top` or `htop` to identify resource-heavy processes
3. Check memory and disk usage
4. Find a specific process by name
5. Practice starting, stopping, and restarting a service
6. Enable a service to start at boot
7. Create a background process and manage it

---

### Module 9: Basic Networking

> **Connecting to the world**

| | |
|---|---|
| **Duration** | 4-5 hours |
| **Prerequisites** | Module 8 completed |
| **Difficulty** | ⭐⭐⭐ Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- Understand basic networking concepts
- View and configure network interfaces
- Test network connectivity
- Troubleshoot common network problems
- Use essential networking commands

#### Key Concepts

1. **Networking Basics**
   - IP addresses (IPv4 and IPv6)
   - Subnet masks and CIDR notation
   - Gateway and DNS
   - Ports and protocols (TCP/UDP)

2. **Network Configuration Commands**
   - `ip addr` / `ip a` - Show IP addresses
   - `ip link` - Show network interfaces
   - `ip route` - Show routing table
   - `ifconfig` - Legacy interface configuration
   - `hostname` - Show/set hostname

3. **Connectivity Testing**
   - `ping` - Test connectivity
   - `traceroute` / `tracepath` - Trace network path
   - `curl` / `wget` - Download from web

4. **DNS Tools**
   - `nslookup` - DNS lookup
   - `dig` - Advanced DNS lookup
   - `host` - Simple DNS lookup
   - `/etc/resolv.conf` - DNS configuration
   - `/etc/hosts` - Local hostname resolution

5. **Network Diagnostics**
   - `ss` - Socket statistics
   - `netstat` - Network statistics (legacy)
   - `nc` (netcat) - Network utility
   - `nmap` - Network scanner (Kali has this!)

#### Practical Exercise: Network Troubleshooting

**Scenario:** A user reports they cannot reach a website. You need to diagnose the problem.

**Tasks:**
1. Check your system's IP configuration
2. Test connectivity to the local gateway
3. Test connectivity to an external server (e.g., 8.8.8.8)
4. Test DNS resolution
5. Trace the route to a remote server
6. Check which ports are listening on your system
7. Document your troubleshooting steps

---

### Module 10: Shell Scripting and System Administration

> **Automating tasks and daily administration**

| | |
|---|---|
| **Duration** | 6-8 hours |
| **Prerequisites** | Modules 1-9 completed |
| **Difficulty** | ⭐⭐⭐⭐ Intermediate-Advanced |

#### Learning Objectives

By the end of this module, you will be able to:
- Write basic shell scripts to automate tasks
- Use variables, conditionals, and loops in scripts
- Schedule tasks with cron
- Perform common system administration tasks
- Understand log files and system monitoring

#### Key Concepts

1. **Shell Scripting Basics**
   - Shebang line: `#!/bin/bash`
   - Making scripts executable
   - Variables and environment variables
   - Command substitution: `$(command)`
   - Exit codes

2. **Script Control Structures**
   - If-else statements
   - Case statements
   - For loops
   - While loops
   - Reading user input

3. **Practical Scripting**
   - Processing command-line arguments
   - Working with files in scripts
   - Error handling
   - Script debugging (`bash -x`)

4. **Task Scheduling**
   - Cron daemon
   - Crontab format: minute hour day month weekday command
   - `crontab -e` - Edit user crontab
   - `crontab -l` - List scheduled tasks
   - System cron directories: `/etc/cron.d/`, `/etc/cron.daily/`

5. **System Administration Tasks**
   - Checking system logs (`/var/log/`)
   - `journalctl` - Systemd journal
   - Disk management basics (`fdisk`, `mount`, `umount`)
   - Backup basics (`tar`, `rsync`)
   - System information: `uname`, `lsb_release`, `hostnamectl`

#### Practical Exercise: Automation Project

**Scenario:** Your team needs automated maintenance scripts and reports.

**Tasks:**
1. Write a script that displays system information (hostname, IP, disk usage, memory)
2. Create a script that backs up a directory to a specified location
3. Write a script that checks if a service is running and restarts it if not
4. Schedule a daily cleanup task using cron
5. Create a log rotation script or configure logrotate
6. Document your scripts for your team

---


### Module 11: System Monitoring and Log Analysis

> **Observability, troubleshooting, and incident investigation**

| | |
|---|---|
| **Duration** | 4-6 hours |
| **Prerequisites** | Modules 1-10 completed |
| **Difficulty** | ⭐⭐⭐ Intermediate |

#### Learning Objectives

By the end of this module, you will be able to:
- Monitor system health in real time using CLI tools
- Investigate service issues with `journalctl` and `/var/log`
- Identify high resource usage and bottlenecks
- Follow a repeatable troubleshooting workflow
- Produce short incident reports with technical evidence

#### Key Concepts

1. **Live Monitoring**
   - `top` / `htop`
   - `free`, `vmstat`, `iostat`
   - `df` and `du` for storage visibility

2. **Service and Process Diagnostics**
   - `systemctl status`
   - `ps aux --sort` and process inspection
   - Failed services and restart analysis

3. **Log Analysis**
   - `/var/log` essentials
   - `journalctl` filtering by service/time/severity
   - Error pattern hunting with `grep`

4. **Network-State Verification**
   - `ip a`, `ip route`, `ss -tulpen`
   - Basic connectivity and DNS checks

5. **Operational Workflow**
   - Symptom confirmation
   - Evidence collection
   - Root-cause hypothesis and validation

---

### Module 12: Backup, Recovery, and Basic Hardening

> **Protecting systems and restoring operations with confidence**

| | |
|---|---|
| **Duration** | 4-6 hours |
| **Prerequisites** | Modules 1-11 completed |
| **Difficulty** | ⭐⭐⭐⭐ Intermediate-Advanced |

#### Learning Objectives

By the end of this module, you will be able to:
- Build a practical backup strategy for Linux hosts
- Automate backups with scripts and cron
- Verify backups and run restore drills
- Apply basic hardening controls for Linux systems
- Write a recovery runbook for common incidents

#### Key Concepts

1. **Backup Strategy**
   - 3-2-1 backup principle
   - What to include/exclude
   - Retention and storage planning

2. **Backup Tooling**
   - `tar` archives
   - `rsync` synchronization
   - Checksums (`sha256sum`) for integrity

3. **Restore and Validation**
   - Test restore to safe directories
   - Compare restored content
   - Recovery time estimation

4. **Security Baseline Hardening**
   - SSH best practices
   - least privilege (`sudo`, users/groups)
   - open-port review and update hygiene

5. **Recovery Operations**
   - Incident runbooks
   - Verification checklist
   - Escalation paths and documentation

---

## Folder Structure for Course Materials

```
Linux101/
│
├── README.md                      # This file - Course overview
│
├── modules/                       # Main course content
│   ├── 01-introduction/
│   │   ├── lesson.md              # Lesson content
│   │   ├── exercises.md           # Practice exercises
│   │   ├── quiz.md                # Self-assessment questions
│   │   └── resources/             # Additional materials
│   │       ├── screenshots/
│   │       └── cheatsheet.md
│   │
│   ├── 02-file-system-navigation/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 03-files-and-directories/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 04-viewing-editing-text/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 05-permissions-ownership/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 06-user-group-management/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 07-package-management/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 08-process-service-management/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 09-networking/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 10-scripting-administration/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   ├── 11-system-monitoring-logging/
│   │   ├── lesson.md
│   │   ├── exercises.md
│   │   ├── quiz.md
│   │   └── resources/
│   │
│   └── 12-backup-recovery-hardening/
│       ├── lesson.md
│       ├── exercises.md
│       ├── quiz.md
│       └── resources/
│
├── exercises/                     # Standalone practice materials
│   ├── practice-files/            # Files for exercises
│   └── solutions/                 # Exercise solutions
│
├── cheatsheets/                   # Quick reference guides
│   ├── commands-quick-reference.md
│   ├── permissions-reference.md
│   ├── networking-reference.md
│   └── troubleshooting-guide.md
│
├── projects/                      # Capstone projects
│   ├── project-01-system-setup/
│   ├── project-02-user-management/
│   └── final-project/
│
└── appendix/                      # Additional resources
    ├── glossary.md                # Linux terminology
    ├── further-learning.md        # Links to additional resources
    └── common-mistakes.md         # Common errors and how to fix them
```

---

## Learning Path Recommendations

### For Fastest Progress:
1. Complete modules in order (1 → 12)
2. Do all exercises before moving to the next module
3. Keep a learning journal with notes and questions
4. Practice commands daily, even just for 15 minutes

### For Better Understanding:
- After Module 4: Spend extra time practicing file operations
- After Module 6: Practice user/permission combinations
- After Module 8: Spend time monitoring your system
- After Module 10: Create your own scripts for daily tasks
- After Module 11: Build and use a daily monitoring checklist
- After Module 12: Run monthly backup-restore drills

### Weekly Schedule Suggestion:
- **Week 1:** Modules 1-2 (Terminal basics and navigation)
- **Week 2:** Modules 3-4 (Files and text)
- **Week 3:** Modules 5-6 (Permissions and users)
- **Week 4:** Modules 7-8 (Packages and processes)
- **Week 5:** Modules 9-10 (Networking and scripting)
- **Week 6:** Modules 11-12 (Monitoring, backup, and hardening)
- **Week 7:** Review, practice, and final project

---

## Tips for Success

### Do's ✅
- Practice every day, even if only for 15-30 minutes
- Make mistakes – it's how you learn!
- Use `man` pages when you're unsure about a command
- Keep notes of commands you learn
- Create your own cheatsheets
- Ask questions when stuck

### Don'ts ❌
- Don't just read – type the commands yourself!
- Don't use `rm -rf /` or similar dangerous commands
- Don't work as root unless absolutely necessary
- Don't skip exercises – practice is essential
- Don't memorize – understand what commands do

### Keyboard Shortcuts to Learn Early:
- `Tab` - Autocomplete commands and filenames
- `Ctrl+C` - Cancel current command
- `Ctrl+L` - Clear screen (same as `clear`)
- `Ctrl+D` - Exit shell / End input
- `↑` / `↓` - Navigate command history

---

## Assessment and Certification

### Self-Assessment:
Each module includes a quiz to test your understanding. Complete all quizzes with at least 80% correct answers before moving to the next module.

### Final Project:
After completing all modules, work on the final project that combines skills from all modules. This project simulates a real system administration scenario.

### Skills Checklist:
- [ ] Can navigate the Linux file system confidently
- [ ] Can create, modify, and delete files and directories
- [ ] Can view and edit text files
- [ ] Understands and can modify file permissions
- [ ] Can manage users and groups
- [ ] Can install, update, and remove software packages
- [ ] Can manage processes and services
- [ ] Can perform basic network troubleshooting
- [ ] Can write simple shell scripts
- [ ] Can perform routine system administration tasks

---

## Support and Resources

### Getting Help:
1. Use `man command` for manual pages
2. Use `command --help` for quick help
3. Search online: "Linux how to [task]"
4. Recommended websites:
   - [Linux Journey](https://linuxjourney.com/)
   - [DigitalOcean Community Tutorials](https://www.digitalocean.com/community/tutorials)
   - [Arch Wiki](https://wiki.archlinux.org/) (comprehensive, works for any distro)

### Recommended Tools:
- **Terminator** or **Tilix**: Better terminal emulator
- **tmux**: Terminal multiplexer
- **htop**: Better process monitor
- **tree**: Directory visualization
- **tldr**: Simplified man pages

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | December 2024 | Initial curriculum release |

---

**Good luck on your Linux journey! 🐧**

*Remember: Every expert was once a beginner. Take your time, practice regularly, and don't be afraid to make mistakes – that's how you learn!*