# Module 2: Navigating the File System

🎯 **Learning Goal**: Master the Linux file system structure and learn to navigate efficiently using the command line.

---

## Introduction

Now that you're comfortable with the terminal from Module 1, it's time to learn how Linux organizes files and how to move around the system. Understanding the file system is fundamental for any system administrator.

In this module, you will learn:
- How Linux organizes files (the file system hierarchy)
- The difference between absolute and relative paths
- Essential navigation commands (`pwd`, `cd`, `ls`)
- Time-saving techniques (Tab completion, command history)

---

## 1. The Linux File System Hierarchy

🎯 **Learning Goal**: Understand the structure of the Linux file system and the purpose of each main directory.

Unlike Windows, which uses drive letters (C:, D:), Linux has a single unified file system that starts from one point: the **root directory** (`/`).

### Visual Overview of the Linux File System

```
/                           ← Root directory (everything starts here!)
├── home/                   ← User home directories
│   ├── kali/              ← Your home folder (kali user)
│   └── john/              ← Another user's home
├── root/                   ← Root user's home directory
├── etc/                    ← System configuration files
├── var/                    ← Variable data (logs, databases)
│   ├── log/               ← System log files
│   └── www/               ← Web server files
├── tmp/                    ← Temporary files
├── usr/                    ← User programs and data
│   ├── bin/               ← User commands
│   └── share/             ← Shared data
├── bin/                    ← Essential user commands
├── sbin/                   ← System administration commands
├── dev/                    ← Device files
├── mnt/                    ← Temporary mount points
├── media/                  ← Removable media (USB, CD)
├── opt/                    ← Optional software
├── proc/                   ← Process information (virtual)
└── boot/                   ← Boot loader files
```

### Important Directories Explained

| Directory | Purpose | Example Contents |
|-----------|---------|------------------|
| `/` | Root - the top of the file system | All other directories |
| `/home` | User home directories | `/home/kali`, `/home/john` |
| `/root` | Home directory for root user | Root's personal files |
| `/etc` | System configuration files | `passwd`, `hostname`, `network` |
| `/var` | Variable data that changes | Logs, mail, databases |
| `/tmp` | Temporary files | Deleted on reboot |
| `/usr` | User programs and utilities | Most installed software |
| `/bin` | Essential user commands | `ls`, `cp`, `mv`, `cat` |
| `/sbin` | System administration commands | `fdisk`, `ifconfig` |
| `/dev` | Device files | `sda` (hard disk), `tty` (terminal) |
| `/mnt` | Manual mount points | Where you mount drives |
| `/media` | Automatic mount points | USB drives, CDs |
| `/opt` | Optional/third-party software | Custom applications |
| `/proc` | Virtual filesystem for processes | Process information |
| `/boot` | Boot loader files | Linux kernel |

### The Home Directory (~)

Your **home directory** is your personal workspace where you store your files. It's located at `/home/username`.

```bash
# For the kali user, the home directory is:
/home/kali

# You can use ~ as a shortcut:
~     # Means /home/kali (your home directory)
```

💡 **Tip**: The `~` symbol (tilde) is a shortcut for your home directory. You'll use it very often!

### Why This Structure Matters

Understanding where things are stored helps you:
- Find configuration files quickly (`/etc`)
- Check system logs for problems (`/var/log`)
- Know where to install custom software (`/opt` or `/usr/local`)
- Understand what's safe to modify (your `/home`) vs. system files

⚠️ **Warning**: Never modify files outside your home directory without understanding what they do. System configuration files in `/etc` and binaries in `/bin` are critical for the system to function!

---

## 2. Understanding Paths

🎯 **Learning Goal**: Understand the difference between absolute and relative paths, and when to use each.

A **path** is the address of a file or directory in the file system. There are two types of paths:

### Absolute Paths

An **absolute path** starts from the root directory (`/`) and gives the complete location.

```bash
# Examples of absolute paths:
/home/kali/Documents
/etc/passwd
/var/log/syslog
```

**Characteristics:**
- Always starts with `/`
- Works from anywhere in the system
- Shows the complete location

### Relative Paths

A **relative path** starts from your current directory.

```bash
# If you're in /home/kali:
Documents          # Same as /home/kali/Documents
Downloads/file.txt # Same as /home/kali/Downloads/file.txt
```

**Characteristics:**
- Does NOT start with `/`
- Depends on where you currently are
- Shorter to type

### Special Path Symbols

| Symbol | Meaning | Example |
|--------|---------|---------|
| `.` | Current directory | `./script.sh` (run script in current dir) |
| `..` | Parent directory (one level up) | `cd ..` (go up one directory) |
| `~` | Home directory | `cd ~` (go to home directory) |
| `-` | Previous directory | `cd -` (go back to where you were) |

### Path Examples

```bash
# Starting location: /home/kali/Documents

# Absolute paths (always work):
/etc/passwd          # Goes to passwd file in /etc
/home/kali           # Goes to kali's home directory

# Relative paths (depend on current location):
..                   # Goes to /home/kali (parent)
../Downloads         # Goes to /home/kali/Downloads
../../               # Goes to /home (two levels up)
./myfile.txt         # Refers to myfile.txt in current directory
```

💡 **Tip**: Use absolute paths in scripts and when giving instructions to others. Use relative paths for quick navigation when working interactively.

---

## 3. Navigation Commands

🎯 **Learning Goal**: Master the essential commands for moving around the file system.

### 3.1 pwd - Print Working Directory

The `pwd` command shows you where you are right now.

```bash
pwd
```

**Example output:**
```
/home/kali
```

This tells you that you're currently in the `/home/kali` directory.

💡 **Tip**: If you ever feel lost, type `pwd` to see exactly where you are!

### 3.2 cd - Change Directory

The `cd` command lets you move to a different directory.

#### Basic Usage

```bash
# Go to an absolute path
cd /etc

# Go to a relative path
cd Documents

# Go up one directory level
cd ..

# Go up two directory levels
cd ../..

# Go to your home directory
cd ~
# OR simply:
cd

# Go to the previous directory
cd -
```

#### Practical Examples

```bash
# Start in home directory
cd ~
pwd                    # Output: /home/kali

# Go to the root directory
cd /
pwd                    # Output: /

# Go to the configuration directory
cd /etc
pwd                    # Output: /etc

# Go to log directory
cd /var/log
pwd                    # Output: /var/log

# Go up one level
cd ..
pwd                    # Output: /var

# Go back home
cd ~
pwd                    # Output: /home/kali

# Go to Downloads folder (relative path)
cd Downloads
pwd                    # Output: /home/kali/Downloads

# Go back to previous directory (before Downloads)
cd -
pwd                    # Output: /home/kali
```

⚠️ **Common Mistakes**:
- Forgetting the space: `cd/etc` ❌ should be `cd /etc` ✓
- Using backslashes: `cd \etc` ❌ (that's Windows!) should be `cd /etc` ✓
- Typos in directory names (use Tab completion to avoid this!)

### 3.3 ls - List Directory Contents

The `ls` command shows you what's inside a directory.

#### Basic ls Options

```bash
# Simple list
ls

# Long format (details)
ls -l

# Show hidden files
ls -a

# Long format + hidden files
ls -la

# Human-readable file sizes
ls -lh

# List specific directory
ls /etc

# List with full details and human-readable sizes
ls -lah
```

#### Understanding ls -l Output

```bash
ls -l
```

**Example output:**
```
total 36
drwxr-xr-x 2 kali kali 4096 Dec  2 09:00 Desktop
drwxr-xr-x 2 kali kali 4096 Dec  2 09:00 Documents
drwxr-xr-x 2 kali kali 4096 Dec  2 09:00 Downloads
-rw-r--r-- 1 kali kali  220 Dec  2 08:00 .bashrc
```

**Breaking down a line:**

```
drwxr-xr-x  2  kali  kali  4096  Dec 2 09:00  Desktop
│└───┬───┘  │   │     │     │       │           │
│    │      │   │     │     │       │           └─ Name
│    │      │   │     │     │       └─ Date modified
│    │      │   │     │     └─ Size in bytes
│    │      │   │     └─ Group owner
│    │      │   └─ User owner
│    │      └─ Number of links
│    └─ Permissions (rwx = read, write, execute)
└─ Type (d = directory, - = file, l = link)
```

#### Useful ls Combinations

```bash
# List all files with sizes in KB/MB/GB
ls -lah

# List only directories
ls -d */

# List files sorted by modification time (newest first)
ls -lt

# List files sorted by size (largest first)
ls -lS

# List files in reverse order
ls -lr

# List files recursively (including subdirectories)
ls -R
```

💡 **Tip**: Create an alias for your favorite `ls` options! (You'll learn about aliases later)

---

## 4. Tab Completion

🎯 **Learning Goal**: Use Tab completion to save time and avoid typos.

Tab completion is one of the most useful features of the Linux terminal. It automatically completes commands, file names, and directory names.

### How to Use Tab Completion

1. **Start typing** a command or path
2. **Press Tab** to auto-complete
3. If multiple options exist, **press Tab twice** to see all possibilities

### Examples

```bash
# Type this and press Tab:
cd /et
# Result: cd /etc/

# Type this and press Tab:
cd /home/ka
# Result: cd /home/kali/

# Type this and press Tab twice:
cd /e
# Shows: etc/ 

# Type this and press Tab:
ls /var/lo
# Result: ls /var/log/
```

### Tab Completion Benefits

1. **Speed**: Type less, navigate faster
2. **Accuracy**: Avoid typos in long paths
3. **Discovery**: See what files/directories exist
4. **Confirmation**: If Tab completes, the path exists!

💡 **Tip**: If Tab doesn't complete anything, the file or directory might not exist, or there might be multiple options. Press Tab twice to see what's available!

---

## 5. Command History

🎯 **Learning Goal**: Use command history to work more efficiently.

The terminal remembers your previous commands. This saves time when you need to repeat or modify commands.

### Basic History Navigation

| Key/Command | Action |
|-------------|--------|
| `↑` (Up arrow) | Previous command |
| `↓` (Down arrow) | Next command |
| `history` | Show all previous commands |
| `history 10` | Show last 10 commands |
| `!number` | Run command number from history |
| `!!` | Run the last command again |
| `!string` | Run the last command starting with 'string' |
| `Ctrl + R` | Search through history |

### Practical Examples

```bash
# View your command history
history

# Output might look like:
#  1  cd /etc
#  2  ls -la
#  3  cat passwd
#  4  cd ~
#  5  pwd

# Run command #3 from history
!3
# This runs: cat passwd

# Run the last command again
!!

# Run the last command that started with 'cd'
!cd

# Search history for a command
# Press Ctrl+R, then type part of the command
# Press Ctrl+R again to find the next match
# Press Enter to execute, or Ctrl+C to cancel
```

### Using Ctrl+R (Reverse Search)

This is a powerful way to find commands you've used before:

1. Press `Ctrl + R`
2. Start typing part of the command you're looking for
3. The terminal shows matching commands
4. Press `Ctrl + R` again to see older matches
5. Press `Enter` to execute the found command
6. Press `Ctrl + C` to cancel and exit search

**Example:**
```
(reverse-i-search)`var': cd /var/log
```

💡 **Tip**: Ctrl+R is especially useful when you've typed a long command and need to run it again!

---

## 6. Putting It All Together

Here's a typical workflow demonstrating these skills:

```bash
# 1. Check where you are
pwd
# Output: /home/kali

# 2. Go to the configuration directory
cd /etc

# 3. List files to see what's there
ls

# 4. Check a specific configuration file
ls -l hostname

# 5. Go to the log directory
cd /var/log

# 6. List log files with details
ls -lh

# 7. Go back to the previous directory (/etc)
cd -

# 8. Go home quickly
cd ~

# 9. Use Tab completion to navigate
cd Doc<Tab>
# Becomes: cd Documents/

# 10. Use history to repeat a command
!ls
# Runs the last ls command
```

---

## ✅ What You Learned

In this module, you learned:

| Topic | Key Points |
|-------|------------|
| File System Hierarchy | Linux has a single root `/`, with `/home` for users, `/etc` for config, `/var` for logs |
| Paths | Absolute paths start with `/`, relative paths start from current directory |
| `pwd` | Shows your current location |
| `cd` | Navigate with `cd path`, `cd ..`, `cd ~`, `cd -` |
| `ls` | List files with `-l` (details), `-a` (hidden), `-h` (human-readable) |
| Tab Completion | Press Tab to auto-complete paths and commands |
| History | Use Up/Down arrows, `history` command, `Ctrl+R` to search |

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                 NAVIGATION QUICK REFERENCE                  │
├─────────────────────────────────────────────────────────────┤
│  pwd              → Show current directory                  │
│  cd /path         → Go to absolute path                     │
│  cd folder        → Go to relative path                     │
│  cd ..            → Go up one level                         │
│  cd ~  or  cd     → Go to home directory                    │
│  cd -             → Go to previous directory                │
├─────────────────────────────────────────────────────────────┤
│  ls               → List files                              │
│  ls -l            → Long format (details)                   │
│  ls -a            → Show hidden files                       │
│  ls -la           → Long format + hidden                    │
│  ls -lh           → Human-readable sizes                    │
│  ls /path         → List specific directory                 │
├─────────────────────────────────────────────────────────────┤
│  Tab              → Auto-complete                           │
│  Tab Tab          → Show all options                        │
│  ↑ / ↓            → Navigate history                        │
│  history          → Show command history                    │
│  Ctrl + R         → Search history                          │
│  !!               → Repeat last command                     │
│  !n               → Run command number n                    │
└─────────────────────────────────────────────────────────────┘
```

---

## Next Steps

In **Module 3**, you will learn how to work with files and directories:
- Creating files and directories
- Copying, moving, and deleting files
- Viewing file contents
- File permissions

But first, complete the **exercises** to practice what you've learned in this module!