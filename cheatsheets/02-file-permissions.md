# File Permissions Cheatsheet

> Quick reference for Linux file permissions and ownership.  
> *For detailed explanations, see Module 5 of this course.*

---

## Understanding Permission Symbols

```
-rwxrwxrwx
│├─┤├─┤├─┤
│ │  │  └── Others (world) permissions
│ │  └───── Group permissions
│ └──────── Owner (user) permissions
└────────── File type (- = file, d = directory, l = link)
```

| Symbol | Permission | For Files | For Directories |
|--------|------------|-----------|-----------------|
| `r` | Read | View contents | List contents |
| `w` | Write | Modify contents | Create/delete files |
| `x` | Execute | Run as program | Enter directory |
| `-` | No permission | No access | No access |

---

## Numeric (Octal) Permissions

| Number | Permission | Binary |
|--------|------------|--------|
| `0` | --- (none) | 000 |
| `1` | --x (execute) | 001 |
| `2` | -w- (write) | 010 |
| `3` | -wx (write+execute) | 011 |
| `4` | r-- (read) | 100 |
| `5` | r-x (read+execute) | 101 |
| `6` | rw- (read+write) | 110 |
| `7` | rwx (all) | 111 |

### Quick Calculation
```
r = 4
w = 2
x = 1

rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
```

---

## Common Permission Patterns

| Numeric | Symbolic | Use Case |
|---------|----------|----------|
| `755` | rwxr-xr-x | Directories, executable scripts |
| `644` | rw-r--r-- | Regular files |
| `700` | rwx------ | Private directories |
| `600` | rw------- | Private files (SSH keys) |
| `777` | rwxrwxrwx | ⚠️ Full access (avoid!) |
| `666` | rw-rw-rw- | ⚠️ All can read/write (avoid!) |
| `750` | rwxr-x--- | Group-shared directories |
| `640` | rw-r----- | Group-readable files |
| `444` | r--r--r-- | Read-only for everyone |

---

## chmod Command (Change Mode)

### Symbolic Mode

| Syntax | Description |
|--------|-------------|
| `chmod u+x file` | Add execute for user (owner) |
| `chmod g+w file` | Add write for group |
| `chmod o-r file` | Remove read for others |
| `chmod a+x file` | Add execute for all |
| `chmod u=rwx file` | Set user to exactly rwx |
| `chmod go= file` | Remove all permissions for group and others |
| `chmod u+x,g-w file` | Multiple changes at once |

#### Symbolic Reference
| Symbol | Meaning |
|--------|---------|
| `u` | User (owner) |
| `g` | Group |
| `o` | Others |
| `a` | All (u+g+o) |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

### Numeric Mode

```bash
chmod 755 script.sh      # rwxr-xr-x
chmod 644 file.txt       # rw-r--r--
chmod 700 private/       # rwx------
chmod 600 id_rsa         # rw------- (SSH private key)
```

### Common Examples

```bash
# Make script executable
chmod +x script.sh

# Make file read-only
chmod 444 important.txt

# Private directory
chmod 700 ~/private

# Shared directory for group
chmod 775 /shared/project

# Recursive (all files in directory)
chmod -R 755 public_html/

# Only directories (not files)
find . -type d -exec chmod 755 {} \;

# Only files (not directories)
find . -type f -exec chmod 644 {} \;
```

---

## chown Command (Change Owner)

| Command | Description |
|---------|-------------|
| `chown user file` | Change owner |
| `chown user:group file` | Change owner and group |
| `chown :group file` | Change group only |
| `chown -R user:group dir/` | Recursive change |

### Examples

```bash
# Change owner to john
sudo chown john file.txt

# Change owner and group
sudo chown john:developers project/

# Recursive ownership change
sudo chown -R www-data:www-data /var/www/html

# Change only group
sudo chown :staff document.pdf
```

---

## chgrp Command (Change Group)

| Command | Description |
|---------|-------------|
| `chgrp group file` | Change group |
| `chgrp -R group dir/` | Recursive change |

### Examples

```bash
# Change group to developers
sudo chgrp developers project/

# Recursive group change
sudo chgrp -R www-data /var/www/
```

---

## Special Permissions

### Overview

| Permission | Numeric | Symbol | Effect |
|------------|---------|--------|--------|
| SUID | 4000 | `s` on user x | Run as file owner |
| SGID | 2000 | `s` on group x | Run as file group / inherit group |
| Sticky Bit | 1000 | `t` on other x | Only owner can delete |

### SUID (Set User ID)
```bash
# View SUID file (note the 's')
ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root ... /usr/bin/passwd

# Set SUID
chmod u+s program
chmod 4755 program

# Remove SUID
chmod u-s program
```

### SGID (Set Group ID)
```bash
# On files: runs as group
# On directories: new files inherit group

# Set SGID
chmod g+s directory/
chmod 2755 directory/

# Remove SGID
chmod g-s directory/
```

### Sticky Bit
```bash
# Common on /tmp - prevents users from deleting others' files
ls -ld /tmp
drwxrwxrwt 1 root root ... /tmp

# Set sticky bit
chmod +t directory/
chmod 1777 directory/

# Remove sticky bit
chmod -t directory/
```

### Combined Special Permissions
```bash
# SUID + SGID + Sticky (rarely used together)
chmod 7755 file    # All three special bits

# SUID only
chmod 4755 file

# SGID only  
chmod 2755 directory/

# Sticky only
chmod 1777 directory/
```

---

## Viewing Permissions

```bash
# List with permissions
ls -l file.txt
-rw-r--r-- 1 user group 1234 Jan 1 12:00 file.txt

# Numeric view (using stat)
stat -c "%a %n" file.txt
644 file.txt

# Full stat information
stat file.txt
```

---

## Default Permissions (umask)

| umask | File Default | Directory Default |
|-------|--------------|-------------------|
| `022` | 644 (rw-r--r--) | 755 (rwxr-xr-x) |
| `077` | 600 (rw-------) | 700 (rwx------) |
| `002` | 664 (rw-rw-r--) | 775 (rwxrwxr-x) |

```bash
# View current umask
umask

# View in symbolic form
umask -S

# Set umask
umask 022

# Set in ~/.bashrc for permanent change
echo "umask 022" >> ~/.bashrc
```

---

## Quick Reference Card

```
Permission Triplets:    Owner | Group | Others
Numeric Values:         r=4, w=2, x=1

Common Combos:
  755 = rwxr-xr-x (directories, scripts)
  644 = rw-r--r-- (files)
  700 = rwx------ (private directory)
  600 = rw------- (private file)

Commands:
  chmod 755 file      # Numeric
  chmod u+x file      # Symbolic
  chown user file     # Change owner
  chown user:grp file # Change both
  chgrp group file    # Change group

Special Permissions:
  SUID (4): chmod u+s or chmod 4755
  SGID (2): chmod g+s or chmod 2755
  Sticky (1): chmod +t or chmod 1755
```

---

*Reference: Module 5 - File Permissions and Ownership*