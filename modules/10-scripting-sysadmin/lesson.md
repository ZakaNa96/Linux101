# Module 10: Shell Scripting and System Administration

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Write shell scripts to automate tasks
- Use variables, conditionals, loops, and functions in scripts
- Handle user input and command-line arguments
- Implement error handling and best practices
- Perform system administration tasks using scripts
- Monitor and manage Linux systems effectively

---

## Introduction

Congratulations! You've made it to the capstone module of this Linux course. You've learned how to navigate the filesystem, manage files and permissions, work with users and groups, install packages, control processes and services, and understand networking basics.

Now it's time to bring it all together with **shell scripting** — the art of automating Linux tasks by writing programs that the shell can execute.

---

## 1. Introduction to Shell Scripting

### What is a Shell Script?

A shell script is a text file containing a series of commands that the shell (command interpreter) can execute. Instead of typing commands one by one, you write them in a file and run them all at once.

Think of it like a recipe: instead of remembering every step to bake a cake, you write down all the instructions and follow them each time.

```bash
# Example: A simple script that shows the date and who is logged in
date
whoami
pwd
```

### Why Write Scripts?

1. **Automation**: Run repetitive tasks without manual intervention
2. **Consistency**: The same commands execute the same way every time
3. **Documentation**: Scripts serve as documentation of how tasks are performed
4. **Efficiency**: Save time by automating complex sequences of commands
5. **Error Reduction**: Eliminate human errors in repetitive tasks

### Bash vs Other Shells

There are several shells available in Linux:

| Shell | Description |
|-------|-------------|
| `bash` | Bourne Again Shell - most common, default on most Linux distributions |
| `sh` | Bourne Shell - original Unix shell, more limited |
| `zsh` | Z Shell - feature-rich, default on macOS |
| `fish` | Friendly Interactive Shell - modern, user-friendly |
| `dash` | Debian Almquist Shell - lightweight, POSIX-compliant |

In Kali Linux, **bash** is the default shell, and that's what we'll use throughout this module.

Check your current shell:
```bash
echo $SHELL
# Output: /bin/bash
```

### When to Use Scripts vs Manual Commands

**Use scripts when:**
- You repeat the same task regularly
- The task involves multiple commands
- You need to schedule tasks (with cron)
- You want to share procedures with others
- You need error handling and logging

**Use manual commands when:**
- It's a one-time task
- You're exploring or experimenting
- The task is too simple to justify a script

---

## 2. Creating Your First Script

### The Shebang Line

Every script should start with a **shebang** (also called "hashbang") line that tells the system which interpreter to use:

```bash
#!/bin/bash
```

The `#!` characters are the shebang, followed by the path to the interpreter.

Common shebangs:
- `#!/bin/bash` — Use bash
- `#!/bin/sh` — Use the default system shell
- `#!/usr/bin/env bash` — Find bash in the user's PATH (more portable)

💡 **Tip**: Always include a shebang line. Without it, the script might be interpreted by a different shell than intended.

### Creating a Script File

Let's create your first script:

```bash
# Create a new file
nano hello.sh
```

Add this content:
```bash
#!/bin/bash
# My first shell script
# Author: Your Name
# Date: Today's date

echo "Hello, World!"
echo "Welcome to shell scripting!"
```

Save and exit (Ctrl+X, Y, Enter).

### Making Scripts Executable

By default, a new file isn't executable. You need to add execute permission:

```bash
# Check current permissions
ls -l hello.sh
# Output: -rw-r--r-- 1 user user 89 Dec  2 10:00 hello.sh

# Add execute permission
chmod +x hello.sh

# Check again
ls -l hello.sh
# Output: -rwxr-xr-x 1 user user 89 Dec  2 10:00 hello.sh
```

### Running Scripts

There are several ways to run a script:

**Method 1: Using the path (requires execute permission)**
```bash
./hello.sh
```

**Method 2: Using bash directly (doesn't require execute permission)**
```bash
bash hello.sh
```

**Method 3: Using source (runs in current shell)**
```bash
source hello.sh
# or
. hello.sh
```

💡 **Tip**: The `./` prefix is needed because the current directory usually isn't in your PATH for security reasons.

### Script File Naming Conventions

- Use the `.sh` extension (though not required)
- Use lowercase letters
- Use hyphens or underscores to separate words
- Choose descriptive names

Good names:
- `backup-home.sh`
- `system_info.sh`
- `check-disk-space.sh`

Avoid:
- `script.sh` (not descriptive)
- `my script.sh` (spaces cause problems)
- `BACKUP.SH` (convention is lowercase)

---

## 3. Variables

### Declaring Variables

Variables store data that your script can use:

```bash
#!/bin/bash

# Declaring variables (NO spaces around the =)
name="Alice"
age=25
directory="/home/alice"

# Using variables
echo "Name: $name"
echo "Age: $age"
echo "Home directory: $directory"
```

⚠️ **Warning**: There must be NO spaces around the `=` sign when assigning variables!

```bash
# WRONG - these cause errors
name = "Alice"
name= "Alice"
name ="Alice"

# CORRECT
name="Alice"
```

### Using Variables

To access a variable's value, prefix it with `$`:

```bash
username="bob"

echo $username           # Simple form
echo ${username}         # Braces form (safer)
echo "Hello, $username"  # Inside double quotes
echo "Hello, ${username}!" # Needed when text follows
```

Use braces `${variable}` when:
- Text immediately follows the variable name
- You want to be explicit and clear

```bash
prefix="super"

# Without braces - looks for variable "prefixman"
echo "$prefixman"        # Prints nothing (undefined variable)

# With braces - correctly prints "superman"
echo "${prefix}man"      # Prints: superman
```

### Quoting: Single vs Double Quotes

**Double quotes** (`"..."`) — Variables are expanded:
```bash
name="Alice"
echo "Hello, $name"      # Output: Hello, Alice
```

**Single quotes** (`'...'`) — Everything is literal:
```bash
name="Alice"
echo 'Hello, $name'      # Output: Hello, $name
```

**Backticks** or `$(...)` — Command substitution:
```bash
echo "Today is $(date)"
echo "Current directory: $(pwd)"
```

### Read-Only Variables

Prevent a variable from being changed:

```bash
readonly PI=3.14159
PI=3.14  # Error: PI: readonly variable
```

### Environment Variables

Environment variables are available to child processes:

```bash
# Regular variable (only in current script)
local_var="I'm local"

# Environment variable (available to child processes)
export GLOBAL_VAR="I'm global"

# Or in one line
export MY_APP_DIR="/opt/myapp"
```

Common environment variables:
- `$HOME` — User's home directory
- `$USER` — Current username
- `$PATH` — Directories to search for commands
- `$PWD` — Current working directory
- `$SHELL` — User's default shell

```bash
#!/bin/bash
echo "Hello, $USER!"
echo "Your home directory is: $HOME"
echo "You are in: $PWD"
```

---

## 4. User Input and Arguments

### The `read` Command

Get input from the user:

```bash
#!/bin/bash

echo "What is your name?"
read name
echo "Hello, $name!"
```

**With a prompt on the same line:**
```bash
#!/bin/bash

read -p "Enter your name: " name
echo "Hello, $name!"
```

**Read silently (for passwords):**
```bash
#!/bin/bash

read -p "Username: " username
read -sp "Password: " password    # -s = silent (no echo)
echo                               # Print newline after hidden input
echo "Logging in as $username..."
```

**Read with a timeout:**
```bash
read -t 10 -p "Enter name (10 sec timeout): " name
```

**Read with a default value:**
```bash
read -p "Enter name [Anonymous]: " name
name=${name:-Anonymous}    # Use "Anonymous" if empty
echo "Hello, $name!"
```

### Command-Line Arguments

Scripts can accept arguments when they're run:

```bash
./greet.sh Alice Bob Charlie
```

Access arguments with special variables:

| Variable | Description |
|----------|-------------|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$n` | nth argument |
| `$#` | Number of arguments |
| `$@` | All arguments (as separate strings) |
| `$*` | All arguments (as one string) |

Example script `args.sh`:
```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Number of arguments: $#"
echo "All arguments: $@"
```

Running it:
```bash
./args.sh hello world
# Output:
# Script name: ./args.sh
# First argument: hello
# Second argument: world
# Number of arguments: 2
# All arguments: hello world
```

### The Exit Status (`$?`)

Every command returns an exit status (0-255):
- **0** = Success
- **Non-zero** = Failure (error code)

```bash
ls /home
echo "Exit status: $?"    # Output: 0 (success)

ls /nonexistent
echo "Exit status: $?"    # Output: 2 (failure)
```

Use exit status in scripts:
```bash
#!/bin/bash

cp important.txt backup/
if [ $? -eq 0 ]; then
    echo "Backup successful!"
else
    echo "Backup failed!"
fi
```

---

## 5. Conditional Statements

### Basic If Statement

```bash
if [ condition ]; then
    # Commands to execute if condition is true
fi
```

Example:
```bash
#!/bin/bash

age=18

if [ $age -ge 18 ]; then
    echo "You are an adult."
fi
```

### If-Else Statement

```bash
#!/bin/bash

age=16

if [ $age -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

### If-Elif-Else Statement

```bash
#!/bin/bash

score=75

if [ $score -ge 90 ]; then
    echo "Grade: A"
elif [ $score -ge 80 ]; then
    echo "Grade: B"
elif [ $score -ge 70 ]; then
    echo "Grade: C"
elif [ $score -ge 60 ]; then
    echo "Grade: D"
else
    echo "Grade: F"
fi
```

### Test Conditions: `[ ]` vs `[[ ]]`

**Single brackets `[ ]`** — POSIX compliant, works in any shell:
```bash
if [ "$name" = "Alice" ]; then
    echo "Hello Alice!"
fi
```

**Double brackets `[[ ]]`** — Bash-specific, more features:
```bash
if [[ "$name" == "Alice" ]]; then
    echo "Hello Alice!"
fi
```

Advantages of `[[ ]]`:
- Pattern matching with `==`
- Regular expressions with `=~`
- No word splitting issues
- `&&` and `||` work inside

💡 **Tip**: Use `[[ ]]` in bash scripts for more robust conditions.

### Comparison Operators

**String Comparisons:**

| Operator | Description |
|----------|-------------|
| `=` or `==` | Equal |
| `!=` | Not equal |
| `-z` | String is empty |
| `-n` | String is not empty |

```bash
name="Alice"

if [ "$name" = "Alice" ]; then
    echo "Name is Alice"
fi

if [ -z "$empty_var" ]; then
    echo "Variable is empty"
fi

if [ -n "$name" ]; then
    echo "Variable is not empty"
fi
```

**Numeric Comparisons:**

| Operator | Description |
|----------|-------------|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-lt` | Less than |
| `-le` | Less than or equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |

```bash
count=10

if [ $count -gt 5 ]; then
    echo "Count is greater than 5"
fi

if [ $count -le 10 ]; then
    echo "Count is less than or equal to 10"
fi
```

⚠️ **Warning**: Don't use `<`, `>`, `=` for numeric comparison in `[ ]` — they're for strings!

### File Tests

| Operator | Description |
|----------|-------------|
| `-e` | File exists |
| `-f` | Is a regular file |
| `-d` | Is a directory |
| `-r` | Is readable |
| `-w` | Is writable |
| `-x` | Is executable |
| `-s` | File exists and is not empty |
| `-L` | Is a symbolic link |

```bash
#!/bin/bash

filename="/etc/passwd"

if [ -e "$filename" ]; then
    echo "$filename exists"
fi

if [ -f "$filename" ]; then
    echo "$filename is a regular file"
fi

if [ -r "$filename" ]; then
    echo "$filename is readable"
fi
```

### Logical Operators

**Within test brackets:**
```bash
# AND: -a (or use && with [[ ]])
if [ $age -ge 18 -a $age -lt 65 ]; then
    echo "Working age adult"
fi

# OR: -o (or use || with [[ ]])
if [ $day = "Saturday" -o $day = "Sunday" ]; then
    echo "It's the weekend!"
fi

# NOT: !
if [ ! -d "/backup" ]; then
    echo "Backup directory doesn't exist"
fi
```

**With double brackets (preferred):**
```bash
if [[ $age -ge 18 && $age -lt 65 ]]; then
    echo "Working age adult"
fi

if [[ $day == "Saturday" || $day == "Sunday" ]]; then
    echo "It's the weekend!"
fi
```

### Case Statements

For multiple conditions based on one variable:

```bash
#!/bin/bash

read -p "Enter a fruit: " fruit

case $fruit in
    apple)
        echo "Apples are red or green."
        ;;
    banana)
        echo "Bananas are yellow."
        ;;
    orange|tangerine)
        echo "Citrus fruits are great!"
        ;;
    *)
        echo "Unknown fruit: $fruit"
        ;;
esac
```

Case statement features:
- `|` for multiple patterns
- `*` for default case
- `;;` ends each case block
- `esac` ends the case statement

**Practical example — a simple menu:**
```bash
#!/bin/bash

echo "System Administration Menu"
echo "1) Show disk usage"
echo "2) Show memory usage"
echo "3) Show logged-in users"
echo "4) Exit"

read -p "Enter your choice: " choice

case $choice in
    1)
        df -h
        ;;
    2)
        free -h
        ;;
    3)
        who
        ;;
    4)
        echo "Goodbye!"
        exit 0
        ;;
    *)
        echo "Invalid option"
        exit 1
        ;;
esac
```

---

## 6. Loops

### For Loops

**Loop through a list:**
```bash
#!/bin/bash

for fruit in apple banana cherry; do
    echo "I like $fruit"
done
```

**Loop through a range:**
```bash
#!/bin/bash

# Count from 1 to 5
for i in {1..5}; do
    echo "Number: $i"
done

# Count with step
for i in {0..10..2}; do
    echo "Even: $i"
done
```

**C-style for loop:**
```bash
#!/bin/bash

for ((i=1; i<=5; i++)); do
    echo "Count: $i"
done
```

**Loop through files:**
```bash
#!/bin/bash

# All .txt files in current directory
for file in *.txt; do
    echo "Processing: $file"
done

# All files in a directory
for file in /home/user/documents/*; do
    if [ -f "$file" ]; then
        echo "File: $file"
    fi
done
```

**Loop through command output:**
```bash
#!/bin/bash

# Loop through each line of command output
for user in $(cat /etc/passwd | cut -d: -f1); do
    echo "User: $user"
done
```

### While Loops

Execute while a condition is true:

```bash
#!/bin/bash

counter=1

while [ $counter -le 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done
```

**Infinite loop with break:**
```bash
#!/bin/bash

while true; do
    read -p "Enter 'quit' to exit: " input
    if [ "$input" = "quit" ]; then
        break
    fi
    echo "You entered: $input"
done
```

### Until Loops

Execute until a condition becomes true:

```bash
#!/bin/bash

counter=1

until [ $counter -gt 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done
```

### Loop Control: break and continue

**break** — Exit the loop immediately:
```bash
#!/bin/bash

for i in {1..10}; do
    if [ $i -eq 5 ]; then
        echo "Breaking at 5"
        break
    fi
    echo "Number: $i"
done
```

**continue** — Skip to the next iteration:
```bash
#!/bin/bash

for i in {1..5}; do
    if [ $i -eq 3 ]; then
        echo "Skipping 3"
        continue
    fi
    echo "Number: $i"
done
```

### Reading Files Line by Line

```bash
#!/bin/bash

# Method 1: while read loop
while read -r line; do
    echo "Line: $line"
done < /etc/hosts

# Method 2: for loop (splits on whitespace, not recommended for lines)
for line in $(cat /etc/hosts); do
    echo "Word: $line"
done
```

💡 **Tip**: Use `while read -r` for reading files line by line. The `-r` prevents backslash interpretation.

**Process each line of a file:**
```bash
#!/bin/bash

while IFS= read -r line; do
    # Skip empty lines
    if [ -z "$line" ]; then
        continue
    fi
    # Skip comments
    if [[ "$line" =~ ^# ]]; then
        continue
    fi
    echo "Processing: $line"
done < config.txt
```

---

## 7. Functions

### Defining Functions

Two syntax options:

```bash
# Method 1: function keyword
function greet {
    echo "Hello!"
}

# Method 2: parentheses (more portable)
greet() {
    echo "Hello!"
}
```

### Calling Functions

Just use the function name:

```bash
#!/bin/bash

greet() {
    echo "Hello, World!"
}

# Call the function
greet
```

### Function Arguments

Functions receive arguments like scripts do:

```bash
#!/bin/bash

greet() {
    echo "Hello, $1!"
    echo "You are $2 years old."
}

# Call with arguments
greet "Alice" 25
```

### Return Values

Functions can return an exit status (0-255):

```bash
#!/bin/bash

is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0    # True (success)
    else
        return 1    # False (failure)
    fi
}

number=4

if is_even $number; then
    echo "$number is even"
else
    echo "$number is odd"
fi
```

**Returning actual values** — Use echo and command substitution:

```bash
#!/bin/bash

add() {
    local result=$(($1 + $2))
    echo $result
}

sum=$(add 5 3)
echo "5 + 3 = $sum"
```

### Local Variables

Variables inside functions are global by default. Use `local` to make them local:

```bash
#!/bin/bash

global_var="I'm global"

my_function() {
    local local_var="I'm local"
    global_var="Modified by function"
    echo "Inside function: $local_var"
}

my_function
echo "Outside function: $global_var"
echo "Outside function: $local_var"    # This will be empty
```

### Practical Function Example

```bash
#!/bin/bash

# Function to check if a service is running
check_service() {
    local service_name=$1
    
    if systemctl is-active --quiet "$service_name"; then
        echo "✓ $service_name is running"
        return 0
    else
        echo "✗ $service_name is not running"
        return 1
    fi
}

# Function to get disk usage percentage
get_disk_usage() {
    local mount_point=${1:-/}
    df -h "$mount_point" | awk 'NR==2 {print $5}'
}

# Use the functions
echo "=== Service Check ==="
check_service ssh
check_service apache2

echo ""
echo "=== Disk Usage ==="
echo "Root partition: $(get_disk_usage /)"
```

---

## 8. Working with Output

### Echo and Printf

**echo** — Simple output:
```bash
echo "Hello, World!"
echo -n "No newline at end"    # -n suppresses newline
echo -e "Tab:\tNewline:\n"     # -e enables escape sequences
```

**printf** — Formatted output (like C):
```bash
printf "Name: %s\n" "Alice"
printf "Age: %d\n" 25
printf "Price: $%.2f\n" 19.99
```

Format specifiers:
- `%s` — String
- `%d` — Integer
- `%f` — Floating point
- `%.2f` — Float with 2 decimal places
- `%10s` — Right-aligned, 10 characters wide
- `%-10s` — Left-aligned, 10 characters wide

```bash
#!/bin/bash

# Formatted table
printf "%-15s %10s %10s\n" "Name" "Age" "Score"
printf "%-15s %10d %10.1f\n" "Alice" 25 95.5
printf "%-15s %10d %10.1f\n" "Bob" 30 88.0
```

### Redirecting Output (Recap)

```bash
# Redirect stdout to file
echo "Hello" > output.txt

# Append to file
echo "World" >> output.txt

# Redirect stderr
command 2> errors.txt

# Redirect both stdout and stderr
command > output.txt 2>&1
# Or in bash:
command &> output.txt

# Discard output
command > /dev/null 2>&1
```

### Logging

Create a logging function for your scripts:

```bash
#!/bin/bash

LOG_FILE="/var/log/myscript.log"

log() {
    local level=$1
    local message=$2
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$timestamp] [$level] $message" >> "$LOG_FILE"
}

# Usage
log "INFO" "Script started"
log "WARNING" "Disk space low"
log "ERROR" "Failed to connect to database"
```

### Colors in Output

Add colors to make output more readable:

```bash
#!/bin/bash

# Color codes
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'  # No Color (reset)

echo -e "${RED}Error: Something went wrong${NC}"
echo -e "${GREEN}Success: Task completed${NC}"
echo -e "${YELLOW}Warning: Check your input${NC}"
echo -e "${BLUE}Info: Processing...${NC}"
```

💡 **Tip**: Always reset the color with `${NC}` or your terminal will stay colored.

---

## 9. Error Handling

### Exit Codes

Set exit codes in your scripts:

```bash
#!/bin/bash

if [ ! -f "$1" ]; then
    echo "Error: File not found"
    exit 1
fi

# Process the file...
echo "Success!"
exit 0
```

Common exit codes:
- `0` — Success
- `1` — General error
- `2` — Misuse of command
- `126` — Permission problem
- `127` — Command not found
- `130` — Script terminated by Ctrl+C

### Set Options for Safer Scripts

```bash
#!/bin/bash
set -e    # Exit immediately if a command exits with non-zero status
set -u    # Treat unset variables as an error
set -o pipefail  # Pipeline fails if any command fails

# Common combination:
set -euo pipefail
```

**Explanation:**
- `set -e` — Script stops at first error
- `set -u` — Using undefined variable is an error
- `set -o pipefail` — Catch errors in pipes

### Trap for Cleanup

Run cleanup code when script exits (even on error or interrupt):

```bash
#!/bin/bash

# Cleanup function
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/myscript_*.tmp
    echo "Done."
}

# Set trap to run cleanup on exit
trap cleanup EXIT

# Also handle interrupts
trap 'echo "Interrupted!"; exit 1' INT TERM

# Your script code here
echo "Working..."
touch /tmp/myscript_$$.tmp
sleep 10
echo "Finished."
```

Signals you can trap:
- `EXIT` — Script exits (any reason)
- `INT` — Interrupt (Ctrl+C)
- `TERM` — Termination signal
- `ERR` — Any error (use with `set -e`)

### Checking Command Success

```bash
#!/bin/bash

# Method 1: Check $?
cp file.txt backup/
if [ $? -ne 0 ]; then
    echo "Copy failed!"
    exit 1
fi

# Method 2: Use if directly
if ! cp file.txt backup/; then
    echo "Copy failed!"
    exit 1
fi

# Method 3: Use && and ||
cp file.txt backup/ && echo "Success" || echo "Failed"
```

### Complete Error Handling Example

```bash
#!/bin/bash
set -euo pipefail

# Configuration
BACKUP_DIR="/backup"
LOG_FILE="/var/log/backup.log"

# Logging function
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Error handler
error_exit() {
    log "ERROR: $1"
    exit 1
}

# Cleanup
cleanup() {
    log "Performing cleanup..."
    # Remove temporary files
    rm -f /tmp/backup_*.tmp
}

trap cleanup EXIT
trap 'error_exit "Script interrupted"' INT TERM

# Verify prerequisites
[ -d "$BACKUP_DIR" ] || error_exit "Backup directory not found"
[ -w "$BACKUP_DIR" ] || error_exit "Backup directory not writable"

# Main script
log "Starting backup..."
# ... backup code ...
log "Backup completed successfully"
```

---

## 10. Practical System Administration Tasks

Now let's apply everything to real-world sysadmin scenarios.

### System Information Script

```bash
#!/bin/bash
# sysinfo.sh - Display system information

echo "============================================="
echo "         SYSTEM INFORMATION REPORT"
echo "============================================="
echo ""

echo ">>> HOSTNAME & OS"
echo "Hostname: $(hostname)"
echo "OS: $(cat /etc/os-release | grep PRETTY_NAME | cut -d'"' -f2)"
echo "Kernel: $(uname -r)"
echo ""

echo ">>> DATE & UPTIME"
echo "Date: $(date)"
echo "Uptime: $(uptime -p)"
echo ""

echo ">>> CPU INFORMATION"
echo "CPU: $(lscpu | grep 'Model name' | cut -d':' -f2 | xargs)"
echo "Cores: $(nproc)"
echo ""

echo ">>> MEMORY USAGE"
free -h | grep -E "Mem|Swap"
echo ""

echo ">>> DISK USAGE"
df -h | grep -E "^/dev|Filesystem"
echo ""

echo ">>> LOGGED IN USERS"
who
echo ""

echo ">>> TOP 5 PROCESSES (by CPU)"
ps aux --sort=-%cpu | head -6
echo ""

echo "============================================="
echo "            END OF REPORT"
echo "============================================="
```

### Backup Script

```bash
#!/bin/bash
# backup.sh - Backup a directory with timestamp

set -euo pipefail

# Configuration
SOURCE_DIR="${1:-/home/$USER/documents}"
BACKUP_DIR="${2:-/backup}"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_$(basename "$SOURCE_DIR")_$DATE.tar.gz"
LOG_FILE="$BACKUP_DIR/backup.log"

# Functions
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Validate
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory does not exist: $SOURCE_DIR"
    exit 1
fi

if [ ! -d "$BACKUP_DIR" ]; then
    echo "Creating backup directory: $BACKUP_DIR"
    mkdir -p "$BACKUP_DIR"
fi

# Create backup
log "Starting backup of $SOURCE_DIR"
tar -czf "$BACKUP_DIR/$BACKUP_NAME" -C "$(dirname "$SOURCE_DIR")" "$(basename "$SOURCE_DIR")"

if [ $? -eq 0 ]; then
    log "Backup successful: $BACKUP_NAME"
    log "Size: $(du -h "$BACKUP_DIR/$BACKUP_NAME" | cut -f1)"
else
    log "Backup failed!"
    exit 1
fi

# Cleanup old backups (keep last 5)
cd "$BACKUP_DIR"
ls -t backup_*.tar.gz 2>/dev/null | tail -n +6 | xargs -r rm --
log "Cleanup complete (kept last 5 backups)"
```

### User Management Script

```bash
#!/bin/bash
# user_manager.sh - Simple user management menu

show_menu() {
    echo ""
    echo "=== User Management ==="
    echo "1) List all users"
    echo "2) Show user info"
    echo "3) Check if user exists"
    echo "4) Show logged-in users"
    echo "5) Show user groups"
    echo "6) Exit"
    echo ""
}

list_users() {
    echo "System users:"
    cat /etc/passwd | cut -d: -f1 | sort
}

show_user_info() {
    read -p "Enter username: " username
    if id "$username" &>/dev/null; then
        echo "User information for: $username"
        id "$username"
        echo "Home: $(grep "^$username:" /etc/passwd | cut -d: -f6)"
        echo "Shell: $(grep "^$username:" /etc/passwd | cut -d: -f7)"
    else
        echo "User '$username' not found"
    fi
}

check_user_exists() {
    read -p "Enter username to check: " username
    if id "$username" &>/dev/null; then
        echo "✓ User '$username' exists"
    else
        echo "✗ User '$username' does not exist"
    fi
}

show_logged_in() {
    echo "Currently logged in users:"
    who
}

show_user_groups() {
    read -p "Enter username: " username
    if id "$username" &>/dev/null; then
        echo "Groups for $username:"
        groups "$username"
    else
        echo "User '$username' not found"
    fi
}

# Main loop
while true; do
    show_menu
    read -p "Enter choice [1-6]: " choice
    
    case $choice in
        1) list_users ;;
        2) show_user_info ;;
        3) check_user_exists ;;
        4) show_logged_in ;;
        5) show_user_groups ;;
        6) echo "Goodbye!"; exit 0 ;;
        *) echo "Invalid option" ;;
    esac
done
```

### Disk Space Monitor

```bash
#!/bin/bash
# disk_monitor.sh - Alert when disk space is low

THRESHOLD=80  # Percent
EMAIL="admin@example.com"

check_disk() {
    echo "=== Disk Space Report ===" 
    echo "Date: $(date)"
    echo ""
    
    df -h | grep -E "^/dev" | while read -r line; do
        usage=$(echo "$line" | awk '{print $5}' | sed 's/%//')
        mount=$(echo "$line" | awk '{print $6}')
        
        if [ "$usage" -ge "$THRESHOLD" ]; then
            echo "⚠ WARNING: $mount is ${usage}% full!"
        else
            echo "✓ OK: $mount is ${usage}% full"
        fi
    done
}

check_disk

# To send email (if mail is configured):
# check_disk | mail -s "Disk Space Report" $EMAIL
```

### Log Analysis Script

```bash
#!/bin/bash
# log_analyzer.sh - Analyze auth logs for failed logins

LOG_FILE="/var/log/auth.log"

echo "=== Authentication Log Analysis ==="
echo "Date: $(date)"
echo "Log file: $LOG_FILE"
echo ""

if [ ! -r "$LOG_FILE" ]; then
    echo "Error: Cannot read $LOG_FILE (try running with sudo)"
    exit 1
fi

echo ">>> Failed Login Attempts (last 24 hours)"
grep "Failed password" "$LOG_FILE" | tail -20

echo ""
echo ">>> Failed Login Count by IP"
grep "Failed password" "$LOG_FILE" | \
    grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | \
    sort | uniq -c | sort -rn | head -10

echo ""
echo ">>> Successful Logins Today"
grep "Accepted" "$LOG_FILE" | grep "$(date +%b\ %d)" | wc -l

echo ""
echo ">>> Users with Failed Attempts"
grep "Failed password" "$LOG_FILE" | \
    grep -oE 'for [^ ]+' | cut -d' ' -f2 | \
    sort | uniq -c | sort -rn | head -10
```

### Service Monitor Script

```bash
#!/bin/bash
# service_monitor.sh - Check status of important services

SERVICES="ssh apache2 mysql postgresql nginx"

echo "=== Service Status Check ==="
echo "Date: $(date)"
echo ""

for service in $SERVICES; do
    if systemctl is-active --quiet "$service" 2>/dev/null; then
        echo "✓ $service: RUNNING"
    elif systemctl list-unit-files | grep -q "^$service"; then
        echo "✗ $service: STOPPED"
    else
        echo "- $service: NOT INSTALLED"
    fi
done
```

---

## 11. System Administration Concepts

### Disk Management

**View disk space:**
```bash
df -h                    # Disk free space (human-readable)
df -h /home              # Specific mount point
```

**View directory sizes:**
```bash
du -h /var/log           # Size of /var/log
du -sh /home/*           # Summary of each item in /home
du -h --max-depth=1 /    # One level deep from root
```

**List block devices:**
```bash
lsblk                    # List block devices
lsblk -f                 # Include filesystem info
```

**View partition table:**
```bash
sudo fdisk -l            # List all partitions
sudo fdisk -l /dev/sda   # Specific disk
```

**Mount/unmount filesystems:**
```bash
sudo mount /dev/sdb1 /mnt          # Mount a device
sudo umount /mnt                    # Unmount
mount                               # Show all mounts
cat /etc/fstab                      # Permanent mount config
```

### Log Files and Journalctl

**Important log locations:**
- `/var/log/syslog` — General system logs
- `/var/log/auth.log` — Authentication logs
- `/var/log/kern.log` — Kernel messages
- `/var/log/apache2/` — Apache web server logs
- `/var/log/apt/` — Package manager logs

**Using journalctl (systemd):**
```bash
journalctl                          # All logs
journalctl -b                       # Logs since last boot
journalctl -u ssh                   # Logs for a specific service
journalctl -f                       # Follow logs in real-time
journalctl --since "2 hours ago"    # Time-based filtering
journalctl -p err                   # Only errors
```

### System Information Commands

```bash
# Operating system
uname -a                  # All system info
uname -r                  # Kernel version
cat /etc/os-release       # OS details
lsb_release -a            # Distribution info (if available)

# Hardware
lscpu                     # CPU information
lsmem                     # Memory information
lsblk                     # Block devices
lspci                     # PCI devices
lsusb                     # USB devices
dmidecode                 # BIOS/hardware info (requires root)
```

### Backups with tar

```bash
# Create archive
tar -cvf archive.tar /path/to/dir          # Create tar
tar -czvf archive.tar.gz /path/to/dir      # Create compressed (gzip)
tar -cjvf archive.tar.bz2 /path/to/dir     # Create compressed (bzip2)

# Extract archive
tar -xvf archive.tar                        # Extract tar
tar -xzvf archive.tar.gz                    # Extract gzip
tar -xjvf archive.tar.bz2                   # Extract bzip2

# List contents
tar -tvf archive.tar                        # List files in archive

# Options explained:
# c = create
# x = extract
# v = verbose
# f = file (specify filename)
# z = gzip compression
# j = bzip2 compression
```

### System Monitoring

```bash
# Uptime and load
uptime
# Output: 10:30:01 up 5 days, 3:45, 2 users, load average: 0.15, 0.10, 0.05

# Memory usage
free -h                    # Human-readable
free -m                    # In megabytes

# Virtual memory statistics
vmstat 1 5                 # Update every 1 second, 5 times

# I/O statistics
iostat

# Process monitoring
top                        # Interactive process viewer
htop                       # Enhanced version (may need to install)
```

---

## 12. Best Practices

### Comment Your Code

```bash
#!/bin/bash
#
# backup.sh - Automated backup script
# 
# Author: Your Name
# Date: December 2024
# Version: 1.0
#
# Description:
#   This script creates compressed backups of the specified directory
#   and maintains a rolling archive of the last 7 days.
#
# Usage:
#   ./backup.sh [source_directory] [backup_directory]
#

# Source directory to backup
SOURCE_DIR="${1:-/home/user/documents}"

# Destination for backups
BACKUP_DIR="${2:-/backup}"

# Function: Create the backup
# Arguments: None
# Returns: 0 on success, 1 on failure
create_backup() {
    # Implementation here
    ...
}
```

### Use Meaningful Variable Names

```bash
# Bad
x=5
y="hello"
z=$(date)

# Good
max_retries=5
greeting_message="hello"
current_date=$(date)
backup_directory="/backup"
log_file="/var/log/myscript.log"
```

### Handle Errors

Always check for errors and handle them gracefully:

```bash
#!/bin/bash
set -euo pipefail

# Check prerequisites
command -v tar >/dev/null 2>&1 || { echo "Error: tar not found"; exit 1; }

# Validate arguments
if [ $# -lt 1 ]; then
    echo "Usage: $0 <directory>"
    exit 1
fi

# Check if directory exists
if [ ! -d "$1" ]; then
    echo "Error: Directory not found: $1"
    exit 1
fi
```

### Test Scripts Safely

1. **Test in a safe environment first** (VM, test directory)
2. **Use echo for dangerous commands initially:**
```bash
# Instead of
rm -rf /var/log/*

# Use
echo "Would delete: rm -rf /var/log/*"
```

3. **Create backups before modifying:**
```bash
cp important.conf important.conf.bak
```

4. **Use set -n for syntax checking:**
```bash
bash -n myscript.sh    # Check syntax without executing
```

### Version Control with Git (Brief Introduction)

Track changes to your scripts with Git:

```bash
# Initialize a repository
cd ~/scripts
git init

# Add files
git add backup.sh

# Commit changes
git commit -m "Add backup script"

# View history
git log

# See changes
git diff

# Undo changes (before commit)
git checkout -- filename
```

💡 **Tip**: Even for small scripts, version control helps you track changes and revert mistakes.

### Script Security Considerations

1. **Don't hardcode passwords:**
```bash
# Bad
PASSWORD="secret123"

# Good - use environment variables or read from secure file
PASSWORD="${DB_PASSWORD:-}"
# or
read -sp "Enter password: " PASSWORD
```

2. **Validate user input:**
```bash
# Sanitize input that might be used in commands
user_input="$1"
# Check for dangerous characters
if [[ "$user_input" =~ [^a-zA-Z0-9_-] ]]; then
    echo "Invalid input"
    exit 1
fi
```

3. **Use full paths for commands in cron jobs:**
```bash
# In cron, use full paths
/usr/bin/tar -czf /backup/archive.tar.gz /home/user
```

4. **Set appropriate permissions:**
```bash
# Scripts with sensitive data
chmod 700 sensitive_script.sh   # Only owner can read/write/execute
```

5. **Be careful with eval:**
```bash
# Avoid eval with user input - it's dangerous!
# eval "$user_input"    # NEVER do this
```

---

## Summary

### ✅ What You Learned

In this capstone module, you learned:

1. **Shell Scripting Fundamentals**
   - Creating and running scripts
   - The shebang line and execute permissions
   - Variables and quoting

2. **Input Handling**
   - Reading user input with `read`
   - Processing command-line arguments
   - Understanding exit status

3. **Flow Control**
   - Conditional statements (if, case)
   - Loops (for, while, until)
   - Loop control (break, continue)

4. **Functions**
   - Defining and calling functions
   - Passing arguments and returning values
   - Local variables

5. **Error Handling**
   - Exit codes and set options
   - Trapping signals for cleanup
   - Defensive programming

6. **System Administration**
   - System information gathering
   - Disk and memory monitoring
   - Log analysis
   - Backup automation
   - Service monitoring

7. **Best Practices**
   - Code documentation
   - Error handling
   - Security considerations
   - Version control basics

---

## Course Completion

🎉 **Congratulations!** You have completed all 10 modules of the Linux 101 course!

You now have a solid foundation in Linux that includes:
- Understanding Linux and its philosophy
- Navigating the filesystem
- Working with files and text
- Managing permissions and ownership
- User and group administration
- Package management
- Process and service control
- Networking basics
- Shell scripting and automation

You are now equipped to:
- Administer Linux systems
- Automate routine tasks
- Troubleshoot common issues
- Continue learning advanced topics

**What's Next?**
- Practice by writing more scripts
- Explore advanced bash features
- Learn about cron jobs for scheduling
- Study system security in depth
- Consider certifications (Linux+, RHCSA)
- Contribute to open-source projects

---

## Quick Reference

### Script Template

```bash
#!/bin/bash
#
# script_name.sh - Brief description
# Author: Your Name
# Date: $(date +%Y-%m-%d)
#

set -euo pipefail

# Variables
SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"
LOG_FILE="${SCRIPT_DIR}/script.log"

# Functions
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

usage() {
    echo "Usage: $0 [options] <arguments>"
    echo "  -h  Show this help"
    exit 1
}

cleanup() {
    log "Cleaning up..."
}

# Trap for cleanup
trap cleanup EXIT

# Parse arguments
while getopts "h" opt; do
    case $opt in
        h) usage ;;
        *) usage ;;
    esac
done
shift $((OPTIND-1))

# Main script
log "Script started"
# Your code here
log "Script completed"
```

### Common Test Conditions

| Test | Description |
|------|-------------|
| `[ -e file ]` | File exists |
| `[ -f file ]` | Is a regular file |
| `[ -d file ]` | Is a directory |
| `[ -r file ]` | Is readable |
| `[ -w file ]` | Is writable |
| `[ -x file ]` | Is executable |
| `[ -z "$var" ]` | String is empty |
| `[ -n "$var" ]` | String is not empty |
| `[ "$a" = "$b" ]` | Strings are equal |
| `[ "$a" != "$b" ]` | Strings are not equal |
| `[ $a -eq $b ]` | Numbers are equal |
| `[ $a -ne $b ]` | Numbers are not equal |
| `[ $a -lt $b ]` | a less than b |
| `[ $a -le $b ]` | a less than or equal to b |
| `[ $a -gt $b ]` | a greater than b |
| `[ $a -ge $b ]` | a greater than or equal to b |

---

Next: Continue practicing with the exercises to solidify your shell scripting skills!