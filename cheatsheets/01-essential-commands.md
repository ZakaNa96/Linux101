# Essential Linux Commands Cheatsheet

> Quick reference for the most commonly used Linux commands.  
> *For detailed explanations, see Modules 1-3 of this course.*

---

## Navigation Commands

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print working directory | `pwd` |
| `cd` | Change directory | `cd /home/user` |
| `cd ..` | Go up one directory | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd -` | Go to previous directory | `cd -` |
| `ls` | List directory contents | `ls` |
| `ls -l` | Long format listing | `ls -l` |
| `ls -a` | Show hidden files | `ls -a` |
| `ls -la` | Long format with hidden | `ls -la` |
| `ls -lh` | Human-readable sizes | `ls -lh` |

---

## File Operations

| Command | Description | Example |
|---------|-------------|---------|
| `touch` | Create empty file | `touch file.txt` |
| `mkdir` | Create directory | `mkdir dirname` |
| `mkdir -p` | Create nested directories | `mkdir -p dir1/dir2/dir3` |
| `cp` | Copy file | `cp source.txt dest.txt` |
| `cp -r` | Copy directory recursively | `cp -r dir1 dir2` |
| `mv` | Move or rename file | `mv old.txt new.txt` |
| `rm` | Remove file | `rm file.txt` |
| `rm -r` | Remove directory recursively | `rm -r dirname` |
| `rm -i` | Interactive remove (confirm) | `rm -i file.txt` |
| `rmdir` | Remove empty directory | `rmdir emptydir` |

### ⚠️ Danger Zone
```bash
# NEVER run these without being absolutely sure:
rm -rf /        # Deletes entire system
rm -rf *        # Deletes everything in current directory
rm -rf ~        # Deletes entire home directory
```

---

## Viewing File Contents

| Command | Description | Example |
|---------|-------------|---------|
| `cat` | Display entire file | `cat file.txt` |
| `cat -n` | Display with line numbers | `cat -n file.txt` |
| `less` | View file with pagination | `less file.txt` |
| `head` | Show first 10 lines | `head file.txt` |
| `head -n 20` | Show first 20 lines | `head -n 20 file.txt` |
| `tail` | Show last 10 lines | `tail file.txt` |
| `tail -n 20` | Show last 20 lines | `tail -n 20 file.txt` |
| `tail -f` | Follow file changes live | `tail -f /var/log/syslog` |

### less Navigation
| Key | Action |
|-----|--------|
| `Space` | Next page |
| `b` | Previous page |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next search result |
| `N` | Previous search result |
| `q` | Quit |

---

## Finding Files and Text

| Command | Description | Example |
|---------|-------------|---------|
| `find` | Find files by name/attributes | `find /home -name "*.txt"` |
| `find -type f` | Find only files | `find . -type f` |
| `find -type d` | Find only directories | `find . -type d` |
| `find -mtime -7` | Modified in last 7 days | `find . -mtime -7` |
| `find -size +10M` | Files larger than 10MB | `find . -size +10M` |
| `locate` | Fast search using database | `locate filename` |
| `which` | Find command location | `which python` |
| `whereis` | Find binary, source, manual | `whereis bash` |

### grep - Search Inside Files

| Command | Description | Example |
|---------|-------------|---------|
| `grep` | Search for pattern | `grep "error" file.txt` |
| `grep -i` | Case insensitive | `grep -i "error" file.txt` |
| `grep -r` | Recursive search | `grep -r "TODO" .` |
| `grep -n` | Show line numbers | `grep -n "error" file.txt` |
| `grep -v` | Invert match (exclude) | `grep -v "debug" file.txt` |
| `grep -c` | Count matches | `grep -c "error" file.txt` |
| `grep -l` | List files with matches | `grep -l "error" *.log` |

---

## Getting Help

| Command | Description | Example |
|---------|-------------|---------|
| `man` | Full manual page | `man ls` |
| `--help` | Quick help | `ls --help` |
| `-h` | Short help (some commands) | `df -h` |
| `whatis` | One-line description | `whatis grep` |
| `apropos` | Search manual descriptions | `apropos "copy files"` |
| `info` | Detailed documentation | `info coreutils` |

### man Page Navigation
| Key | Action |
|-----|--------|
| `Space` | Next page |
| `b` | Previous page |
| `/pattern` | Search |
| `n` | Next match |
| `q` | Quit |

---

## File Information

| Command | Description | Example |
|---------|-------------|---------|
| `file` | Determine file type | `file document.pdf` |
| `stat` | Detailed file information | `stat file.txt` |
| `wc` | Word/line/character count | `wc file.txt` |
| `wc -l` | Line count only | `wc -l file.txt` |
| `du -sh` | Directory size | `du -sh dirname` |
| `df -h` | Disk space usage | `df -h` |

---

## Wildcards (Globbing)

| Pattern | Matches | Example |
|---------|---------|---------|
| `*` | Any characters | `ls *.txt` |
| `?` | Single character | `ls file?.txt` |
| `[abc]` | Any character in brackets | `ls file[123].txt` |
| `[a-z]` | Range of characters | `ls file[a-z].txt` |
| `[!abc]` | Not in brackets | `ls file[!0-9].txt` |

---

## Quick Tips

```bash
# Combine commands with pipes
cat file.txt | grep "error" | wc -l

# Use tab completion
cd /ho[TAB] → cd /home/

# Use history
history          # Show command history
!123             # Run command #123
!!               # Run last command
!grep            # Run last grep command

# Clear terminal
clear            # or Ctrl+L
```

---

*Reference: Modules 1-3 of Linux 101 Course*