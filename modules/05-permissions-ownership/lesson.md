# Module 5: File Permissions and Ownership

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Understand why file permissions are critical for Linux security
- Read and interpret permission strings from `ls -l` output
- Change permissions using both symbolic and numeric (octal) methods
- Modify file and directory ownership
- Apply appropriate permissions for common scenarios
- Understand special permissions (SUID, SGID, Sticky bit)

---

## Prerequisites

Before starting this module, you should be comfortable with:
- ✅ Module 1: Basic terminal navigation
- ✅ Module 2: Navigating the filesystem
- ✅ Module 3: Working with files and directories
- ✅ Module 4: Viewing and editing text files

---

## 1. Why Permissions Matter

### Security Fundamentals

In Linux, **everything is a file** - regular files, directories, devices, and even processes. Permissions control who can access these resources and what they can do with them.

```
┌─────────────────────────────────────────────────────────────────┐
│                    WHY PERMISSIONS MATTER                       │
├─────────────────────────────────────────────────────────────────┤
│  🔒 Security      - Prevent unauthorized access                 │
│  👥 Multi-user    - Multiple users share the same system        │
│  🛡️  Protection   - Prevent accidental file deletion/changes    │
│  📁 Organization  - Control who can access what                 │
└─────────────────────────────────────────────────────────────────┘
```

### Multi-User Systems

Linux was designed from the ground up as a multi-user operating system. This means:
- Multiple users can log in simultaneously
- Each user has their own files and settings
- Users shouldn't be able to access each other's private files
- System files must be protected from regular users

### Protecting Sensitive Data

Consider what would happen without permissions:
- Anyone could read your passwords stored in configuration files
- Anyone could modify system settings
- Malicious scripts could delete critical files
- Private documents would be accessible to everyone

### Real-World Examples

| File/Directory | Why Permissions Matter |
|----------------|----------------------|
| `/etc/shadow` | Contains encrypted passwords - only root should read |
| `/home/user/.ssh/` | SSH keys must be private - others can't access |
| `/var/log/` | Log files - users can read, only system can write |
| Your scripts | Must be executable to run |
| Config files | Often contain sensitive credentials |

💡 **Tip**: In security-focused distributions like Kali Linux, proper permissions are especially important since you may be working with sensitive penetration testing data.

---

## 2. Understanding Permission Types

Linux has three basic permission types:

### Read (r)

```
┌─────────────────────────────────────────────────────────────┐
│  READ (r) Permission                                        │
├─────────────────────────────────────────────────────────────┤
│  For FILES:       View/read the contents of the file        │
│                   Example: cat file.txt, less file.txt      │
│                                                             │
│  For DIRECTORIES: List the contents (see filenames)         │
│                   Example: ls directory/                    │
└─────────────────────────────────────────────────────────────┘
```

### Write (w)

```
┌─────────────────────────────────────────────────────────────┐
│  WRITE (w) Permission                                       │
├─────────────────────────────────────────────────────────────┤
│  For FILES:       Modify, edit, or delete the file contents │
│                   Example: nano file.txt, echo >> file.txt  │
│                                                             │
│  For DIRECTORIES: Create, delete, or rename files inside    │
│                   Example: touch dir/new.txt, rm dir/old.txt│
└─────────────────────────────────────────────────────────────┘
```

### Execute (x)

```
┌─────────────────────────────────────────────────────────────┐
│  EXECUTE (x) Permission                                     │
├─────────────────────────────────────────────────────────────┤
│  For FILES:       Run the file as a program/script          │
│                   Example: ./script.sh, ./program           │
│                                                             │
│  For DIRECTORIES: Enter the directory (cd into it)          │
│                   Example: cd directory/                    │
└─────────────────────────────────────────────────────────────┘
```

### Permission Differences: Files vs Directories

| Permission | On a File | On a Directory |
|------------|-----------|----------------|
| **r** (read) | View file contents | List directory contents |
| **w** (write) | Modify file contents | Create/delete files in directory |
| **x** (execute) | Run as program | Enter (cd into) directory |

⚠️ **Warning**: For directories, you need **both** read (r) AND execute (x) to fully navigate and list contents. Execute alone lets you enter but not list; read alone lets you list names but not access file details.

---

## 3. Understanding User Categories

Permissions are assigned to three categories of users:

```
┌─────────────────────────────────────────────────────────────────┐
│                     USER CATEGORIES                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│    ┌───────────┐    ┌───────────┐    ┌───────────┐             │
│    │  OWNER    │    │   GROUP   │    │  OTHERS   │             │
│    │    (u)    │    │    (g)    │    │    (o)    │             │
│    └───────────┘    └───────────┘    └───────────┘             │
│         │                │                │                     │
│         ▼                ▼                ▼                     │
│    The user who     Users who are    Everyone else             │
│    created the      members of the   on the system             │
│    file             file's group                               │
│                                                                 │
│    ┌─────────────────────────────────────────────┐             │
│    │              ALL (a)                        │             │
│    │    Refers to all three categories           │             │
│    └─────────────────────────────────────────────┘             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Owner (u)
- The user who created the file
- Usually has the most permissions
- Can change permissions on their own files
- Shown as the first name in `ls -l` output

### Group (g)
- A collection of users
- Files belong to one group
- All group members share the same permissions
- Useful for team collaboration

### Others (o)
- Everyone else on the system
- Not the owner AND not in the file's group
- Often called "world" permissions
- Usually the most restricted

### All (a)
- A shorthand for owner + group + others
- Used when changing permissions for everyone

💡 **Tip**: You can see what groups you belong to by running the `groups` command.

---

## 4. Reading Permissions

### The `ls -l` Output Explained

When you run `ls -l`, you see detailed file information:

```bash
$ ls -l
-rw-r--r-- 1 kali kali  1234 Dec  1 10:30 document.txt
drwxr-xr-x 2 kali kali  4096 Dec  1 10:30 my_folder
-rwxr-xr-x 1 kali kali   512 Dec  1 10:30 script.sh
lrwxrwxrwx 1 kali kali    11 Dec  1 10:30 link -> target.txt
```

Let's break down each column:

```
-rw-r--r-- 1 kali kali  1234 Dec  1 10:30 document.txt
│├──┼──┼──┤ │ │    │     │    │           │
││  │  │  │ │ │    │     │    │           └── Filename
││  │  │  │ │ │    │     │    └── Modification date/time
││  │  │  │ │ │    │     └── File size (bytes)
││  │  │  │ │ │    └── Group owner
││  │  │  │ │ └── User owner
││  │  │  │ └── Number of hard links
││  │  │  └── Others permissions (r--)
││  │  └── Group permissions (r--)
││  └── Owner permissions (rw-)
│└── File type (- = regular file)
└── Permission string start
```

### The Permission String

The permission string has 10 characters:

```
    Position:  1    2 3 4    5 6 7    8 9 10
               │    └─┬─┘    └─┬─┘    └─┬─┘
               │      │        │        │
               │      │        │        └── Others: r w x
               │      │        └── Group: r w x
               │      └── Owner: r w x
               └── File Type
```

### File Type Character (Position 1)

| Character | Meaning |
|-----------|---------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |
| `s` | Socket |
| `p` | Named pipe |

### Permission Characters (Positions 2-10)

| Character | Meaning |
|-----------|---------|
| `r` | Read permission granted |
| `w` | Write permission granted |
| `x` | Execute permission granted |
| `-` | Permission denied |

### Examples with Explanations

**Example 1: `-rw-r--r--` (Common for text files)**
```
- rw- r-- r--
│ │   │   │
│ │   │   └── Others: read only
│ │   └── Group: read only
│ └── Owner: read and write
└── Regular file

Meaning: Owner can read/write. Everyone else can only read.
```

**Example 2: `drwxr-xr-x` (Common for directories)**
```
d rwx r-x r-x
│ │   │   │
│ │   │   └── Others: read and execute (can enter and list)
│ │   └── Group: read and execute
│ └── Owner: full access (read, write, execute)
└── Directory

Meaning: Owner has full control. Others can enter and view but not modify.
```

**Example 3: `-rwx------` (Private executable)**
```
- rwx --- ---
│ │   │   │
│ │   │   └── Others: no access
│ │   └── Group: no access
│ └── Owner: full access
└── Regular file

Meaning: Only the owner can read, write, and execute. Nobody else can access.
```

**Example 4: `-rw-------` (Private file)**
```
- rw- --- ---
│ │   │   │
│ │   │   └── Others: no access
│ │   └── Group: no access
│ └── Owner: read and write
└── Regular file

Meaning: Only the owner can read and write. Used for sensitive files like SSH keys.
```

---

## 5. Changing Permissions with chmod

The `chmod` (change mode) command modifies file permissions. There are two methods: **symbolic** and **numeric (octal)**.

### Symbolic Mode

Symbolic mode uses letters and symbols to change permissions:

```
chmod [who][operator][permission] filename
```

**Who:**
- `u` = user (owner)
- `g` = group
- `o` = others
- `a` = all (u, g, and o)

**Operators:**
- `+` = add permission
- `-` = remove permission
- `=` = set exact permission

**Permissions:**
- `r` = read
- `w` = write
- `x` = execute

#### Symbolic Mode Examples

```bash
# Add execute permission for the owner
chmod u+x script.sh

# Remove write permission from group
chmod g-w document.txt

# Set others to read-only (removes any other permissions)
chmod o=r file.txt

# Add execute permission for everyone
chmod a+x program

# Remove all permissions for others
chmod o-rwx private.txt

# Add read and execute for group
chmod g+rx file.txt

# Multiple changes at once
chmod u+x,g-w,o-rwx script.sh

# Make file readable and writable by owner only
chmod u=rw,go= secret.txt
```

💡 **Tip**: Symbolic mode is great when you want to change specific permissions without affecting others. For example, `chmod u+x` adds execute without touching read or write permissions.

### Numeric (Octal) Mode

Numeric mode uses numbers to represent permissions:

```
┌────────────────────────────────────────────────────┐
│          PERMISSION VALUES                          │
├────────────────────────────────────────────────────┤
│   Read (r)    = 4                                  │
│   Write (w)   = 2                                  │
│   Execute (x) = 1                                  │
│   None        = 0                                  │
├────────────────────────────────────────────────────┤
│   Add values together for combined permissions:    │
│                                                    │
│   rwx = 4 + 2 + 1 = 7  (full access)              │
│   rw- = 4 + 2 + 0 = 6  (read and write)           │
│   r-x = 4 + 0 + 1 = 5  (read and execute)         │
│   r-- = 4 + 0 + 0 = 4  (read only)                │
│   -wx = 0 + 2 + 1 = 3  (write and execute)        │
│   -w- = 0 + 2 + 0 = 2  (write only)               │
│   --x = 0 + 0 + 1 = 1  (execute only)             │
│   --- = 0 + 0 + 0 = 0  (no permissions)           │
└────────────────────────────────────────────────────┘
```

The full permission number has three digits: **owner | group | others**

```
chmod 754 file.txt
      ││└── Others: r-x (4+0+1 = 5... wait, that's 4 = r--)
      │└── Group: r-x (5)
      └── Owner: rwx (7)
      
Actually: 754 means:
- Owner: 7 = rwx
- Group: 5 = r-x  
- Others: 4 = r--
```

#### Common Permission Patterns

| Numeric | Symbolic | Meaning | Use Case |
|---------|----------|---------|----------|
| `755` | `rwxr-xr-x` | Owner: full, Others: read/execute | Executable scripts, directories |
| `644` | `rw-r--r--` | Owner: read/write, Others: read | Regular files, documents |
| `700` | `rwx------` | Owner only: full access | Private scripts, directories |
| `600` | `rw-------` | Owner only: read/write | Sensitive config files, SSH keys |
| `777` | `rwxrwxrwx` | Everyone: full access | ⚠️ DANGEROUS - avoid! |
| `666` | `rw-rw-rw-` | Everyone: read/write | ⚠️ DANGEROUS - avoid! |
| `750` | `rwxr-x---` | Owner: full, Group: read/execute | Shared team directories |
| `640` | `rw-r-----` | Owner: read/write, Group: read | Shared config files |

#### Numeric Mode Examples

```bash
# Make a script executable by owner, readable by others
chmod 755 script.sh

# Standard file permissions (owner read/write, others read)
chmod 644 document.txt

# Private file - owner only
chmod 600 secrets.txt

# Private directory - owner only
chmod 700 private_folder/

# Shared directory for a group
chmod 750 team_project/
```

⚠️ **Warning**: Never use `chmod 777` unless absolutely necessary! This gives everyone full access to the file, which is a major security risk.

### Calculating Numeric Permissions

To convert from symbolic to numeric:

```
Example: rwxr-xr--

Owner (rwx):  r(4) + w(2) + x(1) = 7
Group (r-x):  r(4) + w(0) + x(1) = 5
Others (r--): r(4) + w(0) + x(0) = 4

Result: 754
```

To convert from numeric to symbolic:

```
Example: 640

Owner (6):  4+2+0 = rw-
Group (4):  4+0+0 = r--
Others (0): 0+0+0 = ---

Result: rw-r-----
```

---

## 6. Understanding Ownership

Every file and directory in Linux has two types of ownership:
1. **User owner** - A single user who owns the file
2. **Group owner** - A group that owns the file

### Viewing Ownership

Use `ls -l` to see ownership:

```bash
$ ls -l
-rw-r--r-- 1 kali kali 1234 Dec  1 10:30 myfile.txt
              │    │
              │    └── Group owner
              └── User owner
```

### Changing Ownership with chown

The `chown` (change owner) command modifies file ownership.

⚠️ **Warning**: Only root can change file ownership to another user. Regular users can only change group ownership to groups they belong to.

#### Basic Syntax

```bash
# Change owner only
sudo chown newowner filename

# Change owner and group
sudo chown newowner:newgroup filename

# Change group only (note the colon)
sudo chown :newgroup filename

# Recursive (for directories and their contents)
sudo chown -R newowner:newgroup directory/
```

#### chown Examples

```bash
# Change owner to 'john'
sudo chown john document.txt

# Change owner to 'john' and group to 'developers'
sudo chown john:developers project/

# Change only the group to 'www-data'
sudo chown :www-data website/

# Recursively change ownership of a directory and all contents
sudo chown -R www-data:www-data /var/www/html/
```

### Changing Group with chgrp

The `chgrp` command changes only the group ownership:

```bash
# Change group
sudo chgrp developers project_file.txt

# Recursively change group
sudo chgrp -R developers project_folder/
```

💡 **Tip**: `chgrp group file` is equivalent to `chown :group file`

### Practical Ownership Scenarios

**Scenario 1: Web Server Files**
```bash
# Web files owned by www-data user and group
sudo chown -R www-data:www-data /var/www/html/
```

**Scenario 2: Shared Project Folder**
```bash
# Create a shared folder for the 'devteam' group
sudo mkdir /projects/webapp
sudo chown :devteam /projects/webapp
sudo chmod 770 /projects/webapp
```

**Scenario 3: Taking Ownership of Downloaded Files**
```bash
# Make yourself the owner of a downloaded project
sudo chown -R $USER:$USER downloaded_project/
```

---

## 7. Special Permissions (Introduction)

Beyond the basic rwx permissions, Linux has three special permission bits:

### SUID (Set User ID)

When set on an executable file, it runs with the permissions of the file's **owner**, not the user running it.

```bash
# Shown as 's' in owner's execute position
-rwsr-xr-x 1 root root 54256 Dec  1 10:30 /usr/bin/passwd
   ^
   └── SUID bit (s instead of x)
```

**Why it exists**: The `passwd` command needs to modify `/etc/shadow`, which only root can write. SUID allows regular users to change their own passwords.

```bash
# Set SUID
chmod u+s program
chmod 4755 program  # 4 = SUID
```

### SGID (Set Group ID)

- On **files**: Runs with the permissions of the file's group
- On **directories**: New files inherit the directory's group

```bash
# Shown as 's' in group's execute position
drwxr-sr-x 2 kali developers 4096 Dec  1 10:30 shared_folder
      ^
      └── SGID bit
```

```bash
# Set SGID
chmod g+s directory
chmod 2755 directory  # 2 = SGID
```

### Sticky Bit

When set on a directory, only the file's owner (or root) can delete or rename files within it, even if others have write permission.

```bash
# Shown as 't' in others' execute position
drwxrwxrwt 10 root root 4096 Dec  1 10:30 /tmp
         ^
         └── Sticky bit
```

**Common use**: The `/tmp` directory has the sticky bit so users can create temporary files but can't delete each other's files.

```bash
# Set sticky bit
chmod +t directory
chmod 1777 directory  # 1 = sticky bit
```

### Special Permission Summary

| Special Bit | Numeric | Symbol | Effect |
|-------------|---------|--------|--------|
| SUID | 4000 | `s` in owner execute | File runs as file owner |
| SGID | 2000 | `s` in group execute | File runs as file group / New files inherit directory group |
| Sticky | 1000 | `t` in others execute | Only owner can delete in directory |

💡 **Tip**: You'll encounter these special permissions, but as a beginner, you won't need to set them often. Understanding what they mean when you see them is most important.

---

## 8. Default Permissions and umask

### What is umask?

The `umask` (user file creation mask) determines the **default permissions** for newly created files and directories.

### How umask Works

The umask **subtracts** permissions from the maximum:
- Maximum for files: 666 (rw-rw-rw-)
- Maximum for directories: 777 (rwxrwxrwx)

```
Default file permissions = 666 - umask
Default directory permissions = 777 - umask
```

### Viewing umask

```bash
$ umask
0022

$ umask -S    # Symbolic format
u=rwx,g=rx,o=rx
```

### Common umask Values

| umask | File Default | Directory Default | Description |
|-------|--------------|-------------------|-------------|
| 0022 | 644 (rw-r--r--) | 755 (rwxr-xr-x) | Standard (others can read) |
| 0027 | 640 (rw-r-----) | 750 (rwxr-x---) | Private (others blocked) |
| 0077 | 600 (rw-------) | 700 (rwx------) | Very private (owner only) |
| 0002 | 664 (rw-rw-r--) | 775 (rwxrwxr-x) | Group-friendly |

### Calculating Default Permissions

**With umask 0022:**

```
New file:      666 - 022 = 644 (rw-r--r--)
New directory: 777 - 022 = 755 (rwxr-xr-x)
```

**With umask 0077:**

```
New file:      666 - 077 = 600 (rw-------)
New directory: 777 - 077 = 700 (rwx------)
```

### Setting umask

```bash
# Set umask for current session
umask 0027

# Verify the change
umask

# Test it
touch newfile.txt
ls -l newfile.txt
```

💡 **Tip**: To make umask permanent, add the `umask` command to your shell's configuration file (`~/.bashrc` or `~/.zshrc`).

---

## 9. Practical Scenarios

### Scenario 1: Making a Script Executable

```bash
# Create a script
echo '#!/bin/bash
echo "Hello, World!"' > hello.sh

# Try to run it (fails - no execute permission)
./hello.sh
# bash: ./hello.sh: Permission denied

# Check current permissions
ls -l hello.sh
# -rw-r--r-- 1 kali kali 32 Dec  1 10:30 hello.sh

# Add execute permission
chmod +x hello.sh
# OR more specifically:
chmod u+x hello.sh

# Now run it
./hello.sh
# Hello, World!
```

### Scenario 2: Securing Configuration Files

```bash
# Config file with sensitive database password
cat config.ini
# [database]
# password=secret123

# Make it readable only by owner
chmod 600 config.ini

# Verify
ls -l config.ini
# -rw------- 1 kali kali 45 Dec  1 10:30 config.ini
```

### Scenario 3: Setting Up a Shared Directory

```bash
# Create directory for team collaboration
sudo mkdir /projects/shared

# Create a group for the team
sudo groupadd devteam

# Add users to the group
sudo usermod -aG devteam alice
sudo usermod -aG devteam bob

# Set group ownership
sudo chown :devteam /projects/shared

# Set permissions: owner and group have full access, others none
sudo chmod 770 /projects/shared

# Enable SGID so new files inherit the group
sudo chmod g+s /projects/shared

# Verify
ls -ld /projects/shared
# drwxrws--- 2 root devteam 4096 Dec  1 10:30 /projects/shared
```

### Scenario 4: Web Server File Permissions

```bash
# For a web server (Apache/Nginx) on Kali

# Navigate to web directory
cd /var/www/html

# Set ownership to web server user
sudo chown -R www-data:www-data /var/www/html/

# Set directory permissions
sudo find /var/www/html -type d -exec chmod 755 {} \;

# Set file permissions
sudo find /var/www/html -type f -exec chmod 644 {} \;

# Make specific scripts executable
sudo chmod 755 /var/www/html/cgi-bin/*.sh
```

---

## Quick Reference

### Permission Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│                    CHMOD QUICK REFERENCE                      │
├──────────────────────────────────────────────────────────────┤
│  SYMBOLIC MODE                                                │
│  ─────────────                                                │
│  chmod u+x file    Add execute for owner                      │
│  chmod g-w file    Remove write from group                    │
│  chmod o=r file    Set others to read only                    │
│  chmod a+x file    Add execute for all                        │
│  chmod u+x,g=rx    Multiple changes                           │
├──────────────────────────────────────────────────────────────┤
│  NUMERIC MODE                                                 │
│  ─────────────                                                │
│  r=4  w=2  x=1                                               │
│                                                              │
│  chmod 755 file    rwxr-xr-x  (standard executable)          │
│  chmod 644 file    rw-r--r--  (standard file)                │
│  chmod 700 file    rwx------  (private directory)            │
│  chmod 600 file    rw-------  (private file)                 │
├──────────────────────────────────────────────────────────────┤
│  OWNERSHIP                                                    │
│  ──────────                                                   │
│  chown user file           Change owner                       │
│  chown user:group file     Change owner and group             │
│  chown :group file         Change group only                  │
│  chgrp group file          Change group                       │
│  Add -R for recursive                                         │
└──────────────────────────────────────────────────────────────┘
```

### Common Permission Patterns

| Use Case | Numeric | Symbolic |
|----------|---------|----------|
| Executable script | 755 | rwxr-xr-x |
| Regular file | 644 | rw-r--r-- |
| Private file | 600 | rw------- |
| Private directory | 700 | rwx------ |
| Shared directory (group) | 770 | rwxrwx--- |
| SSH private key | 600 | rw------- |
| SSH directory | 700 | rwx------ |

---

## ✅ What You Learned

In this module, you learned:

- [x] Why file permissions are essential for Linux security
- [x] The three permission types: read, write, and execute
- [x] The three user categories: owner, group, and others
- [x] How to read and interpret `ls -l` output and permission strings
- [x] How to change permissions using symbolic mode (`chmod u+x`)
- [x] How to change permissions using numeric mode (`chmod 755`)
- [x] How to change file ownership with `chown` and `chgrp`
- [x] What special permissions (SUID, SGID, Sticky) are
- [x] How `umask` controls default permissions
- [x] Real-world scenarios for applying permissions

---

## Next Steps

In the next module, we'll explore **user and group management** - how to create users, manage groups, and understand the user system that these permissions protect.

Practice the exercises to reinforce your understanding of permissions and ownership!