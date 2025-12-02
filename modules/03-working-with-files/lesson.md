# Module 3: Working with Files and Directories

## 🎯 Learning Goal
By the end of this module, you will be able to create, copy, move, rename, and delete files and directories. You'll also learn to use wildcards for batch operations and find files anywhere on your system.

---

## Prerequisites
Before starting this module, make sure you've completed:
- ✅ Module 1: Introduction to Linux
- ✅ Module 2: Navigating the File System

You should be comfortable with:
- Opening the terminal
- Using `cd` to navigate directories
- Using `ls` to list files
- Understanding absolute and relative paths

---

## 1. Creating Files

### The `touch` Command

The `touch` command is the simplest way to create empty files. It actually has two purposes:
1. Create a new empty file if it doesn't exist
2. Update the timestamp of an existing file

```bash
# Create a single empty file
touch myfile.txt

# Create multiple files at once
touch file1.txt file2.txt file3.txt

# Update timestamp of existing file (doesn't modify content)
touch existing-file.txt
```

💡 **Tip:** The name "touch" comes from the idea of "touching" a file to update when it was last accessed, like touching something leaves your fingerprint with a timestamp.

### Creating Files with Redirection

You can also create files using output redirection:

```bash
# Create an empty file (or overwrite existing!)
> newfile.txt

# Create a file with content
echo "Hello World" > greeting.txt

# Append to a file (adds to end, doesn't overwrite)
echo "Another line" >> greeting.txt
```

⚠️ **Warning:** Using `>` will **overwrite** the file if it already exists! Use `>>` to append safely.

### Checking Your Work

```bash
# List the files you created
ls -l

# View file content
cat greeting.txt
```

---

## 2. Creating Directories

### The `mkdir` Command

Use `mkdir` (make directory) to create new folders:

```bash
# Create a single directory
mkdir projects

# Create multiple directories at once
mkdir documents pictures music

# Verify creation
ls -l
```

### Creating Nested Directories

Want to create a directory structure all at once? Use the `-p` flag (parent):

```bash
# This FAILS if parent directories don't exist
mkdir projects/web/css
# Error: No such file or directory

# This WORKS - creates all parent directories
mkdir -p projects/web/css

# Create a complete project structure in one command
mkdir -p myapp/{src,docs,tests,config}
```

💡 **Tip:** The `-p` flag has two superpowers:
1. Creates parent directories as needed
2. Doesn't complain if the directory already exists

### Practical Example

```bash
# Create a typical project structure
mkdir -p my-website/{css,js,images,pages}

# Verify the structure
ls -R my-website
```

Output:
```
my-website:
css  images  js  pages

my-website/css:

my-website/images:

my-website/js:

my-website/pages:
```

---

## 3. Copying Files and Directories

### The `cp` Command

The `cp` (copy) command duplicates files and directories.

### Copying Files

```bash
# Basic syntax: cp source destination
cp original.txt copy.txt

# Copy file to another directory
cp report.txt documents/

# Copy file to directory with new name
cp report.txt documents/report-backup.txt

# Copy multiple files to a directory
cp file1.txt file2.txt file3.txt backup/
```

### Copying Directories

To copy directories, you **must** use the `-r` (recursive) flag:

```bash
# Copy a directory and all its contents
cp -r projects projects-backup

# Without -r, you get an error
cp projects projects-backup
# Error: omitting directory 'projects'
```

### Useful cp Options

| Option | Description |
|--------|-------------|
| `-r` | Recursive (required for directories) |
| `-i` | Interactive - ask before overwriting |
| `-v` | Verbose - show what's being copied |
| `-n` | No clobber - don't overwrite existing files |
| `-u` | Update - only copy if source is newer |

```bash
# Interactive mode - asks before overwriting
cp -i important.txt backup/important.txt
# cp: overwrite 'backup/important.txt'? y

# Verbose mode - shows progress
cp -rv projects/ projects-backup/
# 'projects/file1.txt' -> 'projects-backup/file1.txt'
# 'projects/file2.txt' -> 'projects-backup/file2.txt'

# Combine options
cp -riv source/ destination/
```

💡 **Tip:** When in doubt, use `-i` to avoid accidentally overwriting important files!

---

## 4. Moving and Renaming

### The `mv` Command

The `mv` (move) command serves two purposes:
1. Move files/directories to a new location
2. Rename files/directories

### Moving Files

```bash
# Move file to a directory
mv report.txt documents/

# Move multiple files
mv file1.txt file2.txt file3.txt archive/

# Move a directory
mv old-projects/ archive/
```

### Renaming Files

```bash
# Rename a file
mv oldname.txt newname.txt

# Rename a directory
mv old-folder new-folder

# Move AND rename at the same time
mv draft.txt documents/final-report.txt
```

### How mv Decides: Move or Rename?

| Command | Destination | Result |
|---------|-------------|--------|
| `mv file.txt newname.txt` | Doesn't exist | Rename |
| `mv file.txt existing-dir/` | Is a directory | Move into directory |
| `mv file.txt existing-dir/newname.txt` | Directory + filename | Move and rename |

### Useful mv Options

```bash
# Interactive - ask before overwriting
mv -i myfile.txt documents/

# Verbose - show what's happening
mv -v *.txt documents/
# renamed 'file1.txt' -> 'documents/file1.txt'
# renamed 'file2.txt' -> 'documents/file2.txt'

# No clobber - never overwrite
mv -n source.txt destination.txt
```

💡 **Tip:** Unlike `cp`, the `mv` command doesn't need `-r` to move directories. It works on directories by default!

---

## 5. Deleting Files and Directories

### ⚠️ CRITICAL WARNING ⚠️

> **Linux does not have a Trash/Recycle Bin by default!**
> 
> When you delete a file with `rm`, it's **gone forever**. There is no undo. Always double-check your commands before pressing Enter!

### The `rm` Command

```bash
# Delete a single file
rm unwanted-file.txt

# Delete multiple files
rm file1.txt file2.txt file3.txt
```

### Safe Deletion with `-i`

```bash
# Interactive mode - asks for confirmation
rm -i important-file.txt
# rm: remove regular file 'important-file.txt'? y
```

💡 **Tip:** Consider making `rm -i` your default by adding this alias to your `.bashrc`:
```bash
alias rm='rm -i'
```

### Deleting Directories

```bash
# Delete an EMPTY directory only
rmdir empty-folder

# Delete directory and ALL contents (recursive)
rm -r folder-with-contents

# Verbose - see what's being deleted
rm -rv old-project/
```

### The Dangerous `-f` Flag

The `-f` (force) flag suppresses all warnings and confirmations:

```bash
# Force delete - NO confirmations, NO errors for missing files
rm -f file.txt

# Force recursive delete - EXTREMELY DANGEROUS
rm -rf directory/
```

### ☠️ DANGER ZONE ☠️

These commands can destroy your system. **NEVER** run them:

```bash
# NEVER DO THIS - deletes EVERYTHING on the system
sudo rm -rf /

# NEVER DO THIS - deletes all files in root directory
sudo rm -rf /*

# NEVER DO THIS - deletes your entire home directory
rm -rf ~

# NEVER DO THIS - deletes hidden files too
rm -rf .*
```

### Safe Deletion Practices

✅ **DO:**
- Always double-check your path before deleting
- Use `ls` first to see what will be deleted: `ls folder/` then `rm -r folder/`
- Use `-i` for interactive confirmation
- Use `-v` to see what's being deleted
- Start with the most specific path possible

❌ **DON'T:**
- Never use `rm -rf /` or `rm -rf /*`
- Never run `rm` commands from random internet sources
- Never use wildcards with `rm -rf` without checking first
- Never run `rm` with `sudo` unless absolutely necessary

---

## 6. Working with Wildcards (Globbing)

Wildcards (also called "globs") let you match multiple files with patterns. This is incredibly powerful for batch operations!

### The `*` Wildcard (Asterisk)

Matches **any number of characters** (including zero):

```bash
# All text files
ls *.txt

# All files starting with "report"
ls report*

# All files containing "2024"
ls *2024*

# All files (same as ls)
ls *
```

### The `?` Wildcard (Question Mark)

Matches **exactly one character**:

```bash
# file1.txt, file2.txt, but NOT file10.txt
ls file?.txt

# All three-letter text files
ls ???.txt

# report1.pdf, report2.pdf, etc.
ls report?.pdf
```

### The `[]` Wildcard (Brackets)

Matches **any single character** from the set:

```bash
# Matches file1.txt, file2.txt, file3.txt
ls file[123].txt

# Matches a range: file1.txt through file9.txt
ls file[1-9].txt

# Matches letters: fileA.txt through fileZ.txt
ls file[A-Z].txt

# Matches both cases: File.txt, file.txt
ls [Ff]ile.txt

# Negation: anything EXCEPT 1, 2, 3
ls file[!123].txt
```

### Wildcard Examples

| Pattern | Matches | Doesn't Match |
|---------|---------|---------------|
| `*.txt` | file.txt, report.txt | file.pdf |
| `file*` | file, file.txt, file123 | myfile |
| `file?.txt` | file1.txt, fileA.txt | file10.txt |
| `[abc]*` | apple, banana, cat | dog |
| `*[0-9]*` | file1, test2data | file |
| `???` | abc, 123, foo | ab, test |

### Practical Wildcard Usage

```bash
# Copy all images to backup
cp *.jpg *.png images-backup/

# Move all log files to archive
mv *.log logs/

# Delete all temporary files
rm *.tmp

# List all shell scripts
ls *.sh

# Find all configuration files
ls *.conf *.cfg *.ini
```

💡 **Tip:** Test your wildcards with `ls` before using them with `rm` or `mv`!

```bash
# First, see what matches
ls *.log

# Then, if it looks right, delete
rm *.log
```

---

## 7. Finding Files

### The `find` Command

The `find` command searches for files in a directory hierarchy. It's very powerful but has a specific syntax.

### Basic find Syntax

```bash
find [where to look] [what to find] [what to do]
```

### Finding by Name

```bash
# Find files named exactly "config.txt"
find /home -name "config.txt"

# Case-insensitive search
find /home -iname "readme.txt"

# Find using wildcards (quote them!)
find /home -name "*.txt"
find /etc -name "*.conf"
```

### Finding by Type

```bash
# Find only files (not directories)
find /var -type f -name "*.log"

# Find only directories
find /home -type d -name "projects"

# Find symbolic links
find /usr -type l
```

### Finding by Time

```bash
# Files modified in the last 24 hours
find /home -mtime -1

# Files modified more than 7 days ago
find /home -mtime +7

# Files modified in the last 60 minutes
find /home -mmin -60
```

### Finding by Size

```bash
# Files larger than 100MB
find /home -size +100M

# Files smaller than 1KB
find /home -size -1k

# Files exactly 0 bytes (empty)
find /home -size 0
```

### Combining Conditions

```bash
# Large log files
find /var/log -type f -name "*.log" -size +10M

# Recent text files
find /home -type f -name "*.txt" -mtime -7

# Empty directories
find /home -type d -empty
```

### The `locate` Command

The `locate` command is faster than `find` because it searches a database instead of the actual filesystem:

```bash
# Find files by name (anywhere on system)
locate config.txt

# Case-insensitive
locate -i readme

# Update the database (needs sudo)
sudo updatedb
```

💡 **Tip:** `locate` is faster but may not find recently created files until the database is updated.

### Finding Commands: `which` and `whereis`

```bash
# Find where a command is located
which python
# /usr/bin/python

which ls
# /usr/bin/ls

# Find binary, source, and manual page
whereis bash
# bash: /usr/bin/bash /etc/bash.bashrc /usr/share/man/man1/bash.1.gz

whereis python
# python: /usr/bin/python /usr/lib/python3.11 /usr/share/man/man1/python.1.gz
```

### Quick Reference: Which Search Tool?

| Need | Use | Example |
|------|-----|---------|
| Find file by name | `find` or `locate` | `find /home -name "*.txt"` |
| Find command location | `which` | `which python` |
| Find command + docs | `whereis` | `whereis bash` |
| Fast filename search | `locate` | `locate myfile` |
| Complex criteria | `find` | `find / -size +100M -mtime -7` |

---

## Summary: Command Reference

### File Creation
| Command | Description |
|---------|-------------|
| `touch file` | Create empty file or update timestamp |
| `> file` | Create/overwrite empty file |
| `echo "text" > file` | Create file with content |
| `echo "text" >> file` | Append to file |

### Directory Creation
| Command | Description |
|---------|-------------|
| `mkdir dir` | Create directory |
| `mkdir -p path/to/dir` | Create nested directories |

### Copying
| Command | Description |
|---------|-------------|
| `cp src dest` | Copy file |
| `cp -r src dest` | Copy directory recursively |
| `cp -i src dest` | Copy with overwrite confirmation |

### Moving/Renaming
| Command | Description |
|---------|-------------|
| `mv src dest` | Move or rename |
| `mv -i src dest` | Move with confirmation |

### Deleting
| Command | Description |
|---------|-------------|
| `rm file` | Delete file |
| `rm -i file` | Delete with confirmation |
| `rm -r dir` | Delete directory recursively |
| `rmdir dir` | Delete empty directory only |

### Finding
| Command | Description |
|---------|-------------|
| `find path -name "pattern"` | Find by name |
| `find path -type f` | Find files only |
| `locate pattern` | Fast database search |
| `which command` | Find command location |

---

## ✅ What You Learned

In this module, you learned how to:

- ✅ Create files with `touch` and redirection (`>`, `>>`)
- ✅ Create directories with `mkdir` and nested structures with `-p`
- ✅ Copy files and directories with `cp` and `cp -r`
- ✅ Move and rename files with `mv`
- ✅ Safely delete files with `rm -i` and directories with `rm -r`
- ✅ Use wildcards (`*`, `?`, `[]`) for batch operations
- ✅ Find files with `find`, `locate`, `which`, and `whereis`
- ✅ Understand the dangers of `rm -rf` and how to stay safe

---

## Next Steps

Now that you can manage files and directories, you're ready for:
- **Module 4:** Viewing and Editing File Contents
- Learn to read files with `cat`, `less`, `head`, `tail`
- Edit files with `nano` and `vim`
- Search within files with `grep`