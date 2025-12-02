# Common Linux Mistakes and How to Avoid Them

> A collection of frequent mistakes made by Linux beginners, with explanations and solutions.

---

## 1. The Dangerous `rm -rf` Command

### The Mistake
```bash
# EXTREMELY DANGEROUS - can destroy your entire system
rm -rf /
rm -rf /*
rm -rf ~
rm -rf ./*    # Dangerous if in wrong directory
```

### Why It's Dangerous
- `rm` = remove files
- `-r` = recursive (include all subdirectories)
- `-f` = force (no confirmation prompts)
- Combined, these can delete everything instantly with no recovery

### How to Avoid
```bash
# Always double-check your current directory first
pwd

# Use interactive mode
rm -ri directory/

# Use verbose mode to see what's being deleted
rm -rv directory/

# Consider using trash-cli instead
trash-put file.txt    # Moves to trash instead of permanent delete

# Never run rm -rf with wildcards without checking first
ls *pattern*          # See what matches first
rm -rf *pattern*      # Then delete
```

### Safe Alternative
```bash
# Create an alias to make rm safer
alias rm='rm -i'    # Always prompt for confirmation

# Or use a safer alternative
alias rm='rm -I'    # Prompt when deleting >3 files
```

---

## 2. Forgetting `sudo`

### The Mistake
```bash
apt update
# Error: Could not open lock file - Permission denied

systemctl restart nginx
# Error: Failed to restart nginx.service: Access denied
```

### Why It Happens
Many system operations require root (administrator) privileges. Regular users cannot modify system files or manage services.

### The Fix
```bash
# Add sudo before administrative commands
sudo apt update
sudo systemctl restart nginx
sudo nano /etc/hosts
```

### How to Avoid
- Learn which operations need sudo (package management, service control, system config)
- If you get "Permission denied," try with sudo
- Don't run everything as sudo - only what needs it

### Important Note
```bash
# DON'T do this (running an interactive shell as root)
sudo su -

# Instead, use sudo for individual commands
sudo command

# If you need multiple commands as root
sudo -i         # Interactive root shell (exit when done)
```

---

## 3. Spaces in Variable Assignments

### The Mistake
```bash
# WRONG - spaces around =
NAME = "John"
# bash: NAME: command not found

# WRONG - unquoted value with spaces
GREETING=Hello World
# bash: World: command not found
```

### The Fix
```bash
# CORRECT - no spaces around =
NAME="John"

# CORRECT - quote values with spaces
GREETING="Hello World"

# CORRECT - using the variable
echo "$NAME"
echo "$GREETING"
```

### How to Avoid
- Never put spaces around `=` in variable assignments
- Always quote strings with spaces
- Always quote variable expansions: `"$VAR"` not `$VAR`

---

## 4. Not Quoting Variables and Strings

### The Mistake
```bash
FILE="my document.txt"

# WRONG - splits on spaces
rm $FILE
# Tries to remove "my" and "document.txt" separately

# WRONG - glob expansion
FILES=*
echo $FILES
# Expands to all files in directory!
```

### The Fix
```bash
# CORRECT - double quotes prevent word splitting
rm "$FILE"

# CORRECT - quotes preserve literal value
FILES="*"
echo "$FILES"  # Prints literal *

# When you WANT expansion, don't quote
for file in *.txt; do
    echo "$file"
done
```

### Rule of Thumb
- Always quote variables: `"$VAR"`
- Only omit quotes when you specifically want expansion or splitting

---

## 5. Running Scripts from Wrong Directory

### The Mistake
```bash
# Script expects to be run from its directory
./scripts/process.sh
# Error: Cannot find config.yml (because config.yml is in scripts/)
```

### Why It Happens
Scripts often use relative paths. If you run from a different directory, the paths don't resolve correctly.

### The Fix
```bash
# Option 1: cd to the script's directory first
cd /path/to/scripts
./process.sh

# Option 2: Use absolute paths in scripts
CONFIG="/opt/myapp/config.yml"

# Option 3: Get script's directory in the script
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
CONFIG="$SCRIPT_DIR/config.yml"
```

### How to Avoid
- Document where scripts should be run from
- Use absolute paths or calculate relative paths from script location
- Check `pwd` before running scripts with relative paths

---

## 6. Permission Denied Errors

### The Mistake
```bash
./script.sh
# bash: ./script.sh: Permission denied
```

### Why It Happens
The file doesn't have execute permission.

### The Fix
```bash
# Add execute permission
chmod +x script.sh
./script.sh

# Or run with interpreter directly
bash script.sh
python script.py
```

### Common Permission Issues and Solutions

| Problem | Solution |
|---------|----------|
| Can't execute script | `chmod +x script.sh` |
| Can't read file | `chmod +r file.txt` or check ownership |
| Can't write to file | `chmod +w file.txt` or check ownership |
| Can't enter directory | `chmod +x directory/` |
| Can't list directory contents | `chmod +r directory/` |
| Need root access | `sudo command` |

---

## 7. PATH Issues

### The Mistake
```bash
myprogram
# bash: myprogram: command not found

# Even though the program exists
ls ~/bin/myprogram
# /home/user/bin/myprogram
```

### Why It Happens
The shell only searches directories listed in `$PATH` for commands.

### The Fix
```bash
# Option 1: Use full or relative path
/home/user/bin/myprogram
./myprogram    # If in current directory

# Option 2: Add directory to PATH (temporary)
export PATH="$PATH:$HOME/bin"

# Option 3: Add to PATH permanently (in ~/.bashrc)
echo 'export PATH="$PATH:$HOME/bin"' >> ~/.bashrc
source ~/.bashrc
```

### Checking PATH
```bash
# View current PATH
echo $PATH

# Find where a command is located
which python
whereis python

# Check if command exists
command -v myprogram && echo "Found" || echo "Not found"
```

---

## 8. Tab vs Spaces in Scripts

### The Mistake
```bash
# Heredoc with tabs (works)
cat <<-EOF
	indented line
EOF

# Makefile with spaces (FAILS!)
target:
    command    # Must be tab, not spaces!
```

### Why It Matters
- **Makefiles**: Commands MUST start with a tab, not spaces
- **Heredocs**: `<<-` requires tabs for indentation stripping
- **YAML**: Typically spaces only, no tabs
- **Python**: Consistent indentation (spaces recommended)

### The Fix
```bash
# For Makefiles - ensure tabs (not visible difference)
# Use cat -A to check
cat -A Makefile
# ^I = tab, spaces show as spaces

# Configure your editor to:
# - Show whitespace characters
# - Use tabs for Makefiles
# - Use spaces for most other files
```

---

## 9. Incorrect Shebang

### The Mistake
```bash
# Script fails or behaves unexpectedly
./script.sh
# Various errors or wrong interpreter used
```

### Common Shebang Issues
```bash
# WRONG - missing shebang (uses current shell, may not be bash)
echo "Hello"

# WRONG - hardcoded path that may not exist
#!/usr/local/bin/bash

# WRONG - typo
#!/bin/bassh

# WRONG - Windows line endings (invisible \r)
#!/bin/bash\r
```

### The Fix
```bash
# CORRECT - portable bash shebang
#!/usr/bin/env bash

# CORRECT - for POSIX shell scripts
#!/bin/sh

# CORRECT - for Python
#!/usr/bin/env python3

# Fix Windows line endings
dos2unix script.sh
# Or
sed -i 's/\r$//' script.sh
```

---

## 10. Confusing Redirection

### The Mistake
```bash
# Trying to redirect AND see output (doesn't work)
command > output.txt
# No output on screen!

# Overwriting when meaning to append
echo "line 2" > log.txt    # Overwrites!
echo "line 3" > log.txt    # Overwrites again!
```

### The Fix
```bash
# Use tee to see AND save output
command | tee output.txt

# Use >> to append, not overwrite
echo "line 2" >> log.txt
echo "line 3" >> log.txt

# Redirect stderr separately
command 2> errors.txt      # stderr to file
command > out.txt 2>&1     # both to file
command &> all.txt         # both to file (bash)
```

### Redirection Cheat Sheet
| Syntax | Description |
|--------|-------------|
| `>` | Redirect stdout (overwrite) |
| `>>` | Redirect stdout (append) |
| `2>` | Redirect stderr |
| `2>>` | Redirect stderr (append) |
| `&>` | Redirect both |
| `2>&1` | stderr to stdout |
| `\| tee file` | stdout to file AND screen |

---

## 11. Case Sensitivity Mistakes

### The Mistake
```bash
# Linux is case-sensitive!
cd /home/User    # Wrong
cd /home/user    # Correct

cat README.txt   # Wrong
cat readme.txt   # Correct (if that's the actual name)
```

### How to Avoid
- Use tab completion (Tab key) to autocomplete filenames correctly
- Use `ls` to see exact filenames
- Be consistent with naming conventions

```bash
# Tab completion helps!
cat READ[TAB]    # Autocompletes to actual filename
```

---

## 12. Forgetting to Escape Special Characters

### The Mistake
```bash
# Searching for literal characters
grep "cost: $100" file.txt     # $100 is interpreted as variable!
grep "[ERROR]" file.txt        # [ ] are regex brackets!
```

### The Fix
```bash
# Escape special characters with backslash
grep "cost: \$100" file.txt
grep "\[ERROR\]" file.txt

# Or use single quotes (no interpretation)
grep 'cost: $100' file.txt
grep '[ERROR]' file.txt        # Still an issue - [ is regex

# Use -F for fixed strings (no regex)
grep -F "[ERROR]" file.txt
grep -F '$100' file.txt
```

### Special Characters to Watch
| Character | Meaning | Escape |
|-----------|---------|--------|
| `$` | Variable | `\$` |
| `*` | Glob/regex | `\*` |
| `?` | Glob/regex | `\?` |
| `[ ]` | Character class | `\[ \]` |
| `&` | Background | `\&` |
| `\|` | Pipe | `\|` |
| `;` | Command separator | `\;` |
| `!` | History expansion | `\!` |
| `~` | Home directory | `\~` |

---

## 13. Using `echo` for File Contents

### The Mistake
```bash
# Trying to view file with echo
echo file.txt
# Output: file.txt (just prints the filename!)
```

### The Fix
```bash
# Use cat to view file contents
cat file.txt

# Or for large files
less file.txt
head file.txt
tail file.txt
```

---

## 14. Misunderstanding `&&` and `||`

### The Mistake
```bash
# Thinking && just separates commands
mkdir test && cd test && touch file
# If mkdir fails, the rest still... doesn't run (which is correct!)

# But not understanding || 
command || echo "failed"
# Runs echo only if command fails
```

### The Fix - Understanding Operators

```bash
# && = AND = run next only if previous succeeds
mkdir test && cd test && touch file

# || = OR = run next only if previous fails
mkdir test || echo "mkdir failed"

# Combining them
command && echo "success" || echo "failure"

# Use ; to always run next command regardless
command1 ; command2    # command2 always runs
```

---

## 15. Not Reading Error Messages

### The Mistake
Ignoring or not understanding error messages.

### How to Fix
Error messages tell you exactly what's wrong:

```bash
# "Permission denied" = need sudo or chmod
# "No such file or directory" = wrong path or typo
# "command not found" = not installed or not in PATH
# "Is a directory" = expected file, got directory
# "Not a directory" = expected directory, got file
# "File exists" = trying to create something that exists
# "Device or resource busy" = file/device in use
```

### Good Practices
1. Read the entire error message
2. Note the file/line number if provided
3. Search the exact error message online
4. Check the man page for the command

---

## Quick Reference: Avoiding Common Mistakes

```
1.  rm -rf     → Always double-check path, use -i flag
2.  sudo       → Know when you need it (system operations)
3.  VAR=value  → No spaces around =
4.  "$VAR"     → Always quote variables
5.  ./script   → Check pwd first, or use absolute paths
6.  Permission → chmod +x, check ownership
7.  PATH       → Add directory or use full path
8.  Tabs       → Required in Makefiles, check with cat -A
9.  Shebang    → Use #!/usr/bin/env bash
10. Redirect   → Use >> for append, tee for both
11. Case       → Linux is case-sensitive
12. Escape     → Quote or escape special characters
13. cat        → Use cat, not echo, for file contents
14. && ||      → Understand command chaining
15. Errors     → Read and understand error messages
```

---

*Reference: Linux 101 Course - All Modules*