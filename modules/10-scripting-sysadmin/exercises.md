# Module 10: Exercises - Shell Scripting and System Administration

## 🎯 Learning Objectives

These exercises will help you:
- Create and run shell scripts
- Work with variables and user input
- Use conditionals and loops
- Build practical system administration tools
- Apply all concepts learned throughout the course

---

## Setup: Create Your Scripts Directory

Before starting the exercises, create a directory for your scripts:

```bash
mkdir -p ~/scripts
cd ~/scripts
```

💡 **Tip**: Keep all your practice scripts organized in this directory.

---

## Exercise 1: Your First Script

### Goal
Create your very first shell script that prints a greeting message.

### Tasks

1. **Create a file named `hello.sh`:**
   ```bash
   nano hello.sh
   ```

2. **Add the following content:**
   - A shebang line (`#!/bin/bash`)
   - A comment with your name and date
   - Echo "Hello, World!"
   - Echo "Welcome to shell scripting!"
   - Create a variable called `my_name` with your name
   - Echo a greeting using that variable

3. **Make the script executable:**
   ```bash
   chmod +x hello.sh
   ```

4. **Run the script three different ways:**
   - Using `./hello.sh`
   - Using `bash hello.sh`
   - Using `source hello.sh`

### Expected Output
```
Hello, World!
Welcome to shell scripting!
My name is Alice
I am learning bash scripting today!
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- Remember: no spaces around the `=` when assigning variables
- Use `$variable_name` to access the variable's value
- Add comments with `#` to document your code

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

```bash
#!/bin/bash
#
# hello.sh - My first shell script
# Author: Alice
# Date: December 2024
#

# Simple greeting
echo "Hello, World!"
echo "Welcome to shell scripting!"

# Using a variable
my_name="Alice"
echo "My name is $my_name"

# Using command substitution
today=$(date +%A)
echo "I am learning bash scripting on $today!"
```

**Running the script:**
```bash
# Method 1: Direct execution (requires execute permission)
./hello.sh

# Method 2: Using bash interpreter
bash hello.sh

# Method 3: Using source (runs in current shell)
source hello.sh
```

</details>

---

## Exercise 2: User Input Script

### Goal
Create a script that interacts with the user by asking questions and displaying personalized responses.

### Tasks

1. **Create `greeter.sh` that:**
   - Asks for the user's name
   - Asks for their favorite color
   - Asks for their age
   - Displays a personalized summary

2. **Add input validation:**
   - Use a prompt on the same line as input
   - Handle empty input with default values

### Expected Output
```
=== Personal Greeter ===

Enter your name: Alice
Enter your favorite color: blue
Enter your age: 25

=== Summary ===
Hello, Alice!
Your favorite color is blue.
You are 25 years old.
In 10 years, you will be 35 years old.
Thank you for using the Personal Greeter!
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- Use `read -p "Prompt: " variable` for inline prompts
- Use `${variable:-default}` to set default values
- For math, use `$((expression))` or `expr`

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

```bash
#!/bin/bash
#
# greeter.sh - Interactive greeting script
#

echo "=== Personal Greeter ==="
echo ""

# Get user input
read -p "Enter your name: " name
read -p "Enter your favorite color: " color
read -p "Enter your age: " age

# Set default values if empty
name=${name:-Anonymous}
color=${color:-unknown}
age=${age:-0}

echo ""
echo "=== Summary ==="
echo "Hello, $name!"
echo "Your favorite color is $color."
echo "You are $age years old."

# Calculate future age
future_age=$((age + 10))
echo "In 10 years, you will be $future_age years old."

echo "Thank you for using the Personal Greeter!"
```

**Enhanced version with password:**
```bash
#!/bin/bash
#
# greeter_enhanced.sh - With password demonstration
#

echo "=== Secure Greeter ==="

read -p "Username: " username
read -sp "Password: " password
echo ""  # New line after hidden input

echo ""
echo "Hello, $username!"
echo "Your password is ${#password} characters long."
echo "(We never show passwords in scripts!)"
```

</details>

---

## Exercise 3: Command-Line Arguments

### Goal
Create a script that processes command-line arguments and handles different scenarios.

### Tasks

1. **Create `args_demo.sh` that:**
   - Displays the script name
   - Shows the number of arguments passed
   - Lists all arguments
   - Processes each argument in a loop

2. **Add error handling:**
   - Check if at least one argument was provided
   - Display usage information if no arguments given

3. **Test with various arguments:**
   ```bash
   ./args_demo.sh
   ./args_demo.sh hello
   ./args_demo.sh one two three four five
   ```

### Expected Output
```
$ ./args_demo.sh apple banana cherry

=== Arguments Demo ===
Script name: ./args_demo.sh
Number of arguments: 3
All arguments: apple banana cherry

Processing each argument:
  Argument 1: apple
  Argument 2: banana
  Argument 3: cherry

Done!
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- `$0` = script name
- `$#` = number of arguments
- `$@` = all arguments
- `$1`, `$2`, etc. = individual arguments
- Use `for arg in "$@"` to loop through all arguments

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

```bash
#!/bin/bash
#
# args_demo.sh - Demonstrate command-line arguments
#

# Check for arguments
if [ $# -eq 0 ]; then
    echo "Usage: $0 <argument1> [argument2] [argument3] ..."
    echo "Example: $0 apple banana cherry"
    exit 1
fi

echo ""
echo "=== Arguments Demo ==="
echo "Script name: $0"
echo "Number of arguments: $#"
echo "All arguments: $@"
echo ""

echo "Processing each argument:"
count=1
for arg in "$@"; do
    echo "  Argument $count: $arg"
    ((count++))
done

echo ""
echo "Done!"
```

**Advanced version - file processor:**
```bash
#!/bin/bash
#
# file_processor.sh - Process files from arguments
#

if [ $# -eq 0 ]; then
    echo "Usage: $0 <file1> [file2] ..."
    exit 1
fi

echo "=== File Processor ==="
echo ""

for file in "$@"; do
    if [ -f "$file" ]; then
        lines=$(wc -l < "$file")
        size=$(du -h "$file" | cut -f1)
        echo "✓ $file: $lines lines, $size"
    elif [ -d "$file" ]; then
        echo "⊡ $file: is a directory"
    else
        echo "✗ $file: not found"
    fi
done
```

</details>

---

## Exercise 4: Conditional Script

### Goal
Create scripts that use conditional statements to make decisions.

### Tasks

#### Part A: File Checker
Create `file_checker.sh` that:
- Takes a filename as an argument
- Checks if the file exists
- If it exists, checks if it's readable, writable, and executable
- Reports the file type (regular file or directory)

#### Part B: Number Analyzer
Create `number_analyzer.sh` that:
- Asks the user for a number
- Determines if it's positive, negative, or zero
- Determines if it's even or odd
- Checks if it's greater than 100

#### Part C: Simple Menu
Create `menu.sh` that:
- Displays a menu with options
- Processes user selection
- Uses a case statement

### Expected Output (Part A)
```
$ ./file_checker.sh /etc/passwd

=== File Checker ===
Checking: /etc/passwd

✓ File exists
✓ It is a regular file
✓ Readable: Yes
✓ Writable: No
✓ Executable: No
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- File tests: `-e` (exists), `-f` (file), `-d` (directory), `-r` (readable), `-w` (writable), `-x` (executable)
- Use `[[ ]]` for more robust conditions
- Case syntax: `case $var in pattern) commands ;; esac`
- For even/odd: use modulo `$((num % 2))`

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

**Part A: file_checker.sh**
```bash
#!/bin/bash
#
# file_checker.sh - Check file properties
#

if [ $# -eq 0 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

filename="$1"

echo ""
echo "=== File Checker ==="
echo "Checking: $filename"
echo ""

# Check if file exists
if [ -e "$filename" ]; then
    echo "✓ File exists"
    
    # Check file type
    if [ -f "$filename" ]; then
        echo "✓ It is a regular file"
    elif [ -d "$filename" ]; then
        echo "✓ It is a directory"
    elif [ -L "$filename" ]; then
        echo "✓ It is a symbolic link"
    else
        echo "✓ It is a special file"
    fi
    
    # Check permissions
    if [ -r "$filename" ]; then
        echo "✓ Readable: Yes"
    else
        echo "✗ Readable: No"
    fi
    
    if [ -w "$filename" ]; then
        echo "✓ Writable: Yes"
    else
        echo "✗ Writable: No"
    fi
    
    if [ -x "$filename" ]; then
        echo "✓ Executable: Yes"
    else
        echo "✗ Executable: No"
    fi
    
    # Show size if it's a file
    if [ -f "$filename" ]; then
        size=$(du -h "$filename" | cut -f1)
        echo "✓ Size: $size"
    fi
    
else
    echo "✗ File does not exist!"
    exit 1
fi
```

**Part B: number_analyzer.sh**
```bash
#!/bin/bash
#
# number_analyzer.sh - Analyze a number
#

read -p "Enter a number: " num

# Validate input is a number
if ! [[ "$num" =~ ^-?[0-9]+$ ]]; then
    echo "Error: Please enter a valid integer"
    exit 1
fi

echo ""
echo "=== Number Analysis ==="
echo "Number: $num"
echo ""

# Positive, negative, or zero
if [ $num -gt 0 ]; then
    echo "✓ The number is positive"
elif [ $num -lt 0 ]; then
    echo "✓ The number is negative"
else
    echo "✓ The number is zero"
fi

# Even or odd (skip for zero)
if [ $num -ne 0 ]; then
    if [ $((num % 2)) -eq 0 ]; then
        echo "✓ The number is even"
    else
        echo "✓ The number is odd"
    fi
fi

# Greater than 100
if [ $num -gt 100 ]; then
    echo "✓ The number is greater than 100"
elif [ $num -lt -100 ]; then
    echo "✓ The number is less than -100"
else
    echo "✓ The number is between -100 and 100"
fi
```

**Part C: menu.sh**
```bash
#!/bin/bash
#
# menu.sh - Simple menu system
#

show_menu() {
    echo ""
    echo "=== System Menu ==="
    echo "1) Show current date and time"
    echo "2) Show current directory"
    echo "3) Show disk usage"
    echo "4) Show who is logged in"
    echo "5) Show system uptime"
    echo "6) Exit"
    echo ""
}

while true; do
    show_menu
    read -p "Enter your choice [1-6]: " choice
    
    case $choice in
        1)
            echo ""
            echo "Current date and time:"
            date
            ;;
        2)
            echo ""
            echo "Current directory:"
            pwd
            ;;
        3)
            echo ""
            echo "Disk usage:"
            df -h | head -5
            ;;
        4)
            echo ""
            echo "Logged in users:"
            who
            ;;
        5)
            echo ""
            echo "System uptime:"
            uptime
            ;;
        6)
            echo "Goodbye!"
            exit 0
            ;;
        *)
            echo "Invalid option. Please try again."
            ;;
    esac
    
    echo ""
    read -p "Press Enter to continue..."
done
```

</details>

---

## Exercise 5: Loop Practice

### Goal
Create scripts that use different types of loops effectively.

### Tasks

#### Part A: Countdown
Create `countdown.sh` that:
- Takes a number as an argument
- Counts down from that number to 1
- Prints "Blastoff!" at the end
- Adds a 1-second delay between numbers

#### Part B: File Lister
Create `file_lister.sh` that:
- Lists all files in the current directory
- Shows file type (file/directory/link)
- Shows file size
- Counts total files and directories

#### Part C: Multiplication Table
Create `multiplication.sh` that:
- Takes a number as an argument
- Prints the multiplication table for that number (1-10)

#### Part D: File Reader
Create `line_reader.sh` that:
- Takes a filename as an argument
- Reads and displays the file line by line
- Numbers each line
- Skips empty lines

### Expected Output (Part A)
```
$ ./countdown.sh 5
5...
4...
3...
2...
1...
Blastoff! 🚀
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- For countdown: `for i in $(seq $num -1 1)` or `for ((i=num; i>=1; i--))`
- For delay: `sleep 1`
- For file loops: `for file in *; do ... done`
- For reading files: `while IFS= read -r line; do ... done < file`

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

**Part A: countdown.sh**
```bash
#!/bin/bash
#
# countdown.sh - Countdown timer
#

if [ $# -eq 0 ]; then
    echo "Usage: $0 <number>"
    exit 1
fi

num=$1

# Validate input
if ! [[ "$num" =~ ^[0-9]+$ ]] || [ "$num" -eq 0 ]; then
    echo "Error: Please provide a positive integer"
    exit 1
fi

echo "Starting countdown from $num..."
echo ""

for ((i=num; i>=1; i--)); do
    echo "$i..."
    sleep 1
done

echo "Blastoff! 🚀"
```

**Part B: file_lister.sh**
```bash
#!/bin/bash
#
# file_lister.sh - List files with details
#

dir="${1:-.}"  # Use current directory if none specified

echo "=== File Listing: $dir ==="
echo ""

file_count=0
dir_count=0
link_count=0

for item in "$dir"/*; do
    # Skip if no matches
    [ -e "$item" ] || continue
    
    name=$(basename "$item")
    
    if [ -L "$item" ]; then
        target=$(readlink "$item")
        echo "[LINK] $name -> $target"
        ((link_count++))
    elif [ -d "$item" ]; then
        echo "[DIR]  $name/"
        ((dir_count++))
    elif [ -f "$item" ]; then
        size=$(du -h "$item" | cut -f1)
        echo "[FILE] $name ($size)"
        ((file_count++))
    fi
done

echo ""
echo "=== Summary ==="
echo "Files: $file_count"
echo "Directories: $dir_count"
echo "Symbolic links: $link_count"
echo "Total: $((file_count + dir_count + link_count))"
```

**Part C: multiplication.sh**
```bash
#!/bin/bash
#
# multiplication.sh - Multiplication table generator
#

if [ $# -eq 0 ]; then
    echo "Usage: $0 <number>"
    exit 1
fi

num=$1

echo ""
echo "=== Multiplication Table for $num ==="
echo ""

for i in {1..10}; do
    result=$((num * i))
    printf "%d x %2d = %3d\n" "$num" "$i" "$result"
done

echo ""
```

**Part D: line_reader.sh**
```bash
#!/bin/bash
#
# line_reader.sh - Read file line by line
#

if [ $# -eq 0 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

filename="$1"

if [ ! -f "$filename" ]; then
    echo "Error: File not found: $filename"
    exit 1
fi

echo "=== Reading: $filename ==="
echo ""

line_num=0
non_empty=0

while IFS= read -r line || [ -n "$line" ]; do
    ((line_num++))
    
    # Skip empty lines
    if [ -z "$line" ]; then
        continue
    fi
    
    ((non_empty++))
    printf "%4d | %s\n" "$line_num" "$line"
    
done < "$filename"

echo ""
echo "=== Statistics ==="
echo "Total lines: $line_num"
echo "Non-empty lines: $non_empty"
```

</details>

---

## Exercise 6: System Info Script

### Goal
Create a comprehensive system information script that gathers and displays important system details.

### Tasks

Create `sysinfo.sh` that displays:
- Hostname and operating system
- Current user
- Current date and time
- System uptime
- CPU information
- Memory usage
- Disk usage
- Network IP addresses
- Logged in users
- Top 5 processes by CPU usage

### Requirements
- Use functions to organize the code
- Format the output nicely
- Add section headers
- Handle errors gracefully

### Expected Output
```
======================================================
            SYSTEM INFORMATION REPORT
======================================================

>>> SYSTEM DETAILS
Hostname:     kali
OS:           Kali GNU/Linux Rolling
Kernel:       5.10.0-kali9-amd64
Current User: alice

>>> DATE & UPTIME
Date:         Mon Dec  2 10:30:00 UTC 2024
Uptime:       up 2 days, 5 hours, 30 minutes

>>> CPU INFORMATION
Model:        Intel(R) Core(TM) i7-9700 CPU @ 3.00GHz
Cores:        8

>>> MEMORY USAGE
              total        used        free
Mem:          16Gi        4.2Gi       10Gi
Swap:         2.0Gi       0.0Ki       2.0Gi

>>> DISK USAGE
Filesystem    Size  Used Avail Use% Mounted on
/dev/sda1     100G   25G   70G  27% /

>>> NETWORK
IP Address:   192.168.1.100

>>> LOGGED IN USERS
alice    pts/0        2024-12-02 08:00

>>> TOP 5 PROCESSES (by CPU)
USER       PID %CPU COMMAND
alice     1234  5.2 firefox
alice     5678  2.1 code
root       123  0.5 Xorg

======================================================
              END OF REPORT
======================================================
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- Use `uname`, `hostname`, `/etc/os-release` for system info
- Use `lscpu` for CPU info, `nproc` for core count
- Use `free -h` for memory
- Use `df -h` for disk
- Use `ip addr` or `hostname -I` for IP
- Use `who` for logged in users
- Use `ps aux --sort=-%cpu | head` for top processes

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

```bash
#!/bin/bash
#
# sysinfo.sh - System Information Report
# Author: Student
# Date: December 2024
#

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
NC='\033[0m'

# Print header
print_header() {
    echo ""
    echo "======================================================"
    echo "            SYSTEM INFORMATION REPORT"
    echo "======================================================"
    echo ""
}

# Print section
print_section() {
    echo -e "${BLUE}>>> $1${NC}"
}

# System details
show_system() {
    print_section "SYSTEM DETAILS"
    echo "Hostname:     $(hostname)"
    
    if [ -f /etc/os-release ]; then
        echo "OS:           $(grep PRETTY_NAME /etc/os-release | cut -d'"' -f2)"
    fi
    
    echo "Kernel:       $(uname -r)"
    echo "Current User: $(whoami)"
    echo ""
}

# Date and uptime
show_uptime() {
    print_section "DATE & UPTIME"
    echo "Date:         $(date)"
    echo "Uptime:       $(uptime -p)"
    echo ""
}

# CPU info
show_cpu() {
    print_section "CPU INFORMATION"
    
    if command -v lscpu &> /dev/null; then
        model=$(lscpu | grep "Model name" | cut -d':' -f2 | xargs)
        echo "Model:        $model"
    fi
    
    echo "Cores:        $(nproc)"
    echo ""
}

# Memory info
show_memory() {
    print_section "MEMORY USAGE"
    free -h | grep -E "Mem|Swap|total"
    echo ""
}

# Disk info
show_disk() {
    print_section "DISK USAGE"
    df -h | grep -E "^/dev|Filesystem" | head -5
    echo ""
}

# Network info
show_network() {
    print_section "NETWORK"
    
    # Try different methods to get IP
    if command -v ip &> /dev/null; then
        ip=$(ip -4 addr show | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | grep -v '127.0.0.1' | head -1)
    elif command -v hostname &> /dev/null; then
        ip=$(hostname -I | awk '{print $1}')
    else
        ip="Unknown"
    fi
    
    echo "IP Address:   ${ip:-Not available}"
    echo ""
}

# Logged in users
show_users() {
    print_section "LOGGED IN USERS"
    who
    echo ""
}

# Top processes
show_processes() {
    print_section "TOP 5 PROCESSES (by CPU)"
    ps aux --sort=-%cpu | head -6 | awk '{printf "%-10s %6s %5s %s\n", $1, $2, $3, $11}'
    echo ""
}

# Print footer
print_footer() {
    echo "======================================================"
    echo "              END OF REPORT"
    echo "======================================================"
    echo ""
}

# Main
main() {
    print_header
    show_system
    show_uptime
    show_cpu
    show_memory
    show_disk
    show_network
    show_users
    show_processes
    print_footer
}

# Run main function
main
```

</details>

---

## Exercise 7: Backup Script

### Goal
Create a practical backup script that can be used for regular system backups.

### Tasks

Create `backup.sh` that:
1. Takes a source directory as argument (defaults to ~/Documents)
2. Creates a backup directory if it doesn't exist
3. Creates a compressed tar archive with timestamp
4. Logs all operations
5. Keeps only the last 5 backups (deletes older ones)
6. Verifies the backup was created successfully

### Requirements
- Use meaningful variable names
- Include error handling
- Add logging functionality
- Make it safe to run multiple times

### Usage Example
```bash
./backup.sh                          # Backup ~/Documents
./backup.sh /home/user/projects      # Backup specific directory
./backup.sh /etc /root/backups       # Backup /etc to custom location
```

### Expected Output
```
=== Backup Script ===
Date: 2024-12-02 10:30:00

Source:      /home/alice/Documents
Destination: /home/alice/backups
Backup name: backup_Documents_20241202_103000.tar.gz

[INFO] Creating backup...
[INFO] Backup created successfully!
[INFO] Size: 15M
[INFO] Location: /home/alice/backups/backup_Documents_20241202_103000.tar.gz
[INFO] Cleaning old backups (keeping last 5)...
[INFO] Removed: backup_Documents_20241125_090000.tar.gz
[INFO] Backup complete!
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- Use `tar -czf` to create compressed archives
- Use `date +%Y%m%d_%H%M%S` for timestamp
- Use `basename` to get directory name
- Use `ls -t | tail -n +6` to list files to delete
- Always quote variable expansions

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

```bash
#!/bin/bash
#
# backup.sh - Directory backup script with rotation
# Author: Student
# Date: December 2024
#

set -euo pipefail

# Configuration
DEFAULT_SOURCE="$HOME/Documents"
DEFAULT_BACKUP_DIR="$HOME/backups"
MAX_BACKUPS=5
LOG_FILE=""

# Parse arguments
SOURCE_DIR="${1:-$DEFAULT_SOURCE}"
BACKUP_DIR="${2:-$DEFAULT_BACKUP_DIR}"

# Generate names
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
DIR_NAME=$(basename "$SOURCE_DIR")
BACKUP_NAME="backup_${DIR_NAME}_${TIMESTAMP}.tar.gz"
LOG_FILE="$BACKUP_DIR/backup.log"

# Logging function
log() {
    local level="$1"
    local message="$2"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$level] $message"
    if [ -n "$LOG_FILE" ] && [ -d "$(dirname "$LOG_FILE")" ]; then
        echo "[$timestamp] [$level] $message" >> "$LOG_FILE"
    fi
}

# Error handler
error_exit() {
    log "ERROR" "$1"
    exit 1
}

# Print header
print_header() {
    echo ""
    echo "=== Backup Script ==="
    echo "Date: $(date '+%Y-%m-%d %H:%M:%S')"
    echo ""
    echo "Source:      $SOURCE_DIR"
    echo "Destination: $BACKUP_DIR"
    echo "Backup name: $BACKUP_NAME"
    echo ""
}

# Validate source
validate_source() {
    if [ ! -d "$SOURCE_DIR" ]; then
        error_exit "Source directory does not exist: $SOURCE_DIR"
    fi
    
    if [ ! -r "$SOURCE_DIR" ]; then
        error_exit "Source directory is not readable: $SOURCE_DIR"
    fi
}

# Create backup directory
create_backup_dir() {
    if [ ! -d "$BACKUP_DIR" ]; then
        log "INFO" "Creating backup directory: $BACKUP_DIR"
        mkdir -p "$BACKUP_DIR" || error_exit "Failed to create backup directory"
    fi
}

# Create backup
create_backup() {
    log "INFO" "Creating backup..."
    
    # Create the archive
    tar -czf "$BACKUP_DIR/$BACKUP_NAME" \
        -C "$(dirname "$SOURCE_DIR")" \
        "$(basename "$SOURCE_DIR")" 2>/dev/null
    
    if [ $? -eq 0 ] && [ -f "$BACKUP_DIR/$BACKUP_NAME" ]; then
        local size=$(du -h "$BACKUP_DIR/$BACKUP_NAME" | cut -f1)
        log "INFO" "Backup created successfully!"
        log "INFO" "Size: $size"
        log "INFO" "Location: $BACKUP_DIR/$BACKUP_NAME"
    else
        error_exit "Failed to create backup"
    fi
}

# Rotate old backups
rotate_backups() {
    log "INFO" "Cleaning old backups (keeping last $MAX_BACKUPS)..."
    
    cd "$BACKUP_DIR"
    
    # Find and remove old backups
    local pattern="backup_${DIR_NAME}_*.tar.gz"
    local count=$(ls -1 $pattern 2>/dev/null | wc -l)
    
    if [ "$count" -gt "$MAX_BACKUPS" ]; then
        ls -t $pattern | tail -n +$((MAX_BACKUPS + 1)) | while read -r old_backup; do
            log "INFO" "Removed: $old_backup"
            rm -f "$old_backup"
        done
    fi
    
    cd - > /dev/null
}

# Verify backup
verify_backup() {
    log "INFO" "Verifying backup integrity..."
    
    if tar -tzf "$BACKUP_DIR/$BACKUP_NAME" > /dev/null 2>&1; then
        log "INFO" "Backup verified successfully!"
    else
        error_exit "Backup verification failed!"
    fi
}

# List existing backups
list_backups() {
    echo ""
    echo "=== Existing Backups ==="
    ls -lh "$BACKUP_DIR"/backup_${DIR_NAME}_*.tar.gz 2>/dev/null || echo "No previous backups found"
}

# Main function
main() {
    print_header
    validate_source
    create_backup_dir
    create_backup
    verify_backup
    rotate_backups
    list_backups
    
    echo ""
    log "INFO" "Backup complete!"
}

# Run main
main
```

</details>

---

## Challenge Exercise: Complete Admin Toolkit

### Goal
Create a comprehensive menu-driven system administration toolkit that combines everything you've learned throughout this course.

### Tasks

Create `admin_toolkit.sh` that provides:

1. **System Information Menu**
   - Show system overview
   - Show CPU details
   - Show memory usage
   - Show disk usage

2. **User Management Menu**
   - List all users
   - Show user details
   - Check if user exists
   - Show current logged in users

3. **Network Menu**
   - Show IP configuration
   - Test connectivity (ping)
   - Show open ports
   - Show active connections

4. **File Operations Menu**
   - Search for files
   - Find large files
   - Check disk usage by directory
   - Find recently modified files

5. **Service Menu** (may require sudo)
   - List running services
   - Check service status
   - View recent system logs

6. **Additional Features**
   - Logging to file
   - Help option
   - Clean exit with confirmation

### Requirements
- Modular design with functions
- Error handling
- Input validation
- Professional formatting
- Comments throughout

### Expected Interface
```
╔════════════════════════════════════════════════════════╗
║            LINUX ADMIN TOOLKIT v1.0                   ║
╠════════════════════════════════════════════════════════╣
║  1) System Information                                 ║
║  2) User Management                                    ║
║  3) Network Utilities                                  ║
║  4) File Operations                                    ║
║  5) Service Management                                 ║
║  6) View Logs                                          ║
║  7) Help                                               ║
║  8) Exit                                               ║
╚════════════════════════════════════════════════════════╝

Enter your choice [1-8]:
```

### Hints
<details>
<summary>Click to reveal hints</summary>

- Use nested case statements or call sub-menus
- Create reusable functions for common tasks
- Use `printf` for formatted output
- Consider using arrays for menu options
- Test each function independently before integration

</details>

### Solution
<details>
<summary>Click to reveal solution</summary>

```bash
#!/bin/bash
#
# admin_toolkit.sh - Comprehensive System Administration Toolkit
# Author: Student
# Date: December 2024
# Version: 1.0
#

# ====================
# Configuration
# ====================

VERSION="1.0"
LOG_FILE="$HOME/admin_toolkit.log"

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
CYAN='\033[0;36m'
NC='\033[0m'

# ====================
# Utility Functions
# ====================

log() {
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$timestamp] $1" >> "$LOG_FILE"
}

print_banner() {
    clear
    echo -e "${CYAN}"
    echo "╔════════════════════════════════════════════════════════╗"
    echo "║            LINUX ADMIN TOOLKIT v$VERSION                    ║"
    echo "╚════════════════════════════════════════════════════════╝"
    echo -e "${NC}"
}

print_menu() {
    echo -e "${BLUE}╔════════════════════════════════════════════════════════╗${NC}"
    echo -e "${BLUE}║${NC}  1) System Information                                 ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  2) User Management                                    ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  3) Network Utilities                                  ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  4) File Operations                                    ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  5) Service Management                                 ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  6) View Toolkit Log                                   ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  7) Help                                               ${BLUE}║${NC}"
    echo -e "${BLUE}║${NC}  8) Exit                                               ${BLUE}║${NC}"
    echo -e "${BLUE}╚════════════════════════════════════════════════════════╝${NC}"
    echo ""
}

press_enter() {
    echo ""
    read -p "Press Enter to continue..."
}

print_submenu_header() {
    echo ""
    echo -e "${GREEN}=== $1 ===${NC}"
    echo ""
}

# ====================
# System Information
# ====================

system_menu() {
    while true; do
        print_banner
        print_submenu_header "System Information"
        echo "1) System Overview"
        echo "2) CPU Information"
        echo "3) Memory Usage"
        echo "4) Disk Usage"
        echo "5) Back to Main Menu"
        echo ""
        read -p "Enter choice [1-5]: " choice
        
        case $choice in
            1) system_overview ;;
            2) cpu_info ;;
            3) memory_info ;;
            4) disk_info ;;
            5) return ;;
            *) echo -e "${RED}Invalid option${NC}" ;;
        esac
    done
}

system_overview() {
    print_submenu_header "System Overview"
    echo "Hostname:     $(hostname)"
    echo "OS:           $(cat /etc/os-release 2>/dev/null | grep PRETTY_NAME | cut -d'"' -f2)"
    echo "Kernel:       $(uname -r)"
    echo "Architecture: $(uname -m)"
    echo "Uptime:       $(uptime -p)"
    echo "Date:         $(date)"
    log "Viewed system overview"
    press_enter
}

cpu_info() {
    print_submenu_header "CPU Information"
    if command -v lscpu &>/dev/null; then
        lscpu | grep -E "Model name|CPU\(s\)|Thread|Core|Socket|MHz"
    else
        echo "CPU info not available"
    fi
    log "Viewed CPU info"
    press_enter
}

memory_info() {
    print_submenu_header "Memory Usage"
    free -h
    echo ""
    echo "Top 5 memory-consuming processes:"
    ps aux --sort=-%mem | head -6 | awk '{printf "%-10s %6s %5s%% %s\n", $1, $2, $4, $11}'
    log "Viewed memory info"
    press_enter
}

disk_info() {
    print_submenu_header "Disk Usage"
    df -h | grep -E "^/dev|Filesystem"
    log "Viewed disk info"
    press_enter
}

# ====================
# User Management
# ====================

user_menu() {
    while true; do
        print_banner
        print_submenu_header "User Management"
        echo "1) List All Users"
        echo "2) Show User Details"
        echo "3) Check If User Exists"
        echo "4) Show Logged In Users"
        echo "5) Show User Groups"
        echo "6) Back to Main Menu"
        echo ""
        read -p "Enter choice [1-6]: " choice
        
        case $choice in
            1) list_users ;;
            2) user_details ;;
            3) check_user ;;
            4) logged_users ;;
            5) user_groups ;;
            6) return ;;
            *) echo -e "${RED}Invalid option${NC}" ;;
        esac
    done
}

list_users() {
    print_submenu_header "All System Users"
    echo "Username       UID    Home Directory"
    echo "--------       ---    --------------"
    awk -F: '{printf "%-14s %-6s %s\n", $1, $3, $6}' /etc/passwd | head -20
    echo ""
    echo "Total users: $(wc -l < /etc/passwd)"
    log "Listed users"
    press_enter
}

user_details() {
    print_submenu_header "User Details"
    read -p "Enter username: " username
    if id "$username" &>/dev/null; then
        echo ""
        echo "User ID info:"
        id "$username"
        echo ""
        echo "Home directory: $(grep "^$username:" /etc/passwd | cut -d: -f6)"
        echo "Shell: $(grep "^$username:" /etc/passwd | cut -d: -f7)"
        log "Viewed details for user: $username"
    else
        echo -e "${RED}User '$username' not found${NC}"
    fi
    press_enter
}

check_user() {
    print_submenu_header "Check User Existence"
    read -p "Enter username to check: " username
    if id "$username" &>/dev/null; then
        echo -e "${GREEN}✓ User '$username' exists${NC}"
    else
        echo -e "${RED}✗ User '$username' does not exist${NC}"
    fi
    log "Checked user existence: $username"
    press_enter
}

logged_users() {
    print_submenu_header "Currently Logged In Users"
    who
    echo ""
    echo "Total logged in: $(who | wc -l)"
    log "Viewed logged in users"
    press_enter
}

user_groups() {
    print_submenu_header "User Groups"
    read -p "Enter username: " username
    if id "$username" &>/dev/null; then
        echo "Groups for $username:"
        groups "$username"
    else
        echo -e "${RED}User '$username' not found${NC}"
    fi
    log "Viewed groups for: $username"
    press_enter
}

# ====================
# Network Utilities
# ====================

network_menu() {
    while true; do
        print_banner
        print_submenu_header "Network Utilities"
        echo "1) Show IP Configuration"
        echo "2) Test Connectivity (Ping)"
        echo "3) Show Listening Ports"
        echo "4) Show Active Connections"
        echo "5) Show Routing Table"
        echo "6) Back to Main Menu"
        echo ""
        read -p "Enter choice [1-6]: " choice
        
        case $choice in
            1) ip_config ;;
            2) ping_test ;;
            3) listening_ports ;;
            4) active_connections ;;
            5) routing_table ;;
            6) return ;;
            *) echo -e "${RED}Invalid option${NC}" ;;
        esac
    done
}

ip_config() {
    print_submenu_header "IP Configuration"
    if command -v ip &>/dev/null; then
        ip addr show | grep -E "^[0-9]|inet "
    else
        ifconfig 2>/dev/null || echo "Network tools not available"
    fi
    log "Viewed IP configuration"
    press_enter
}

ping_test() {
    print_submenu_header "Connectivity Test"
    read -p "Enter host to ping (default: google.com): " host
    host=${host:-google.com}
    echo "Pinging $host..."
    ping -c 4 "$host" 2>&1 || echo -e "${RED}Ping failed${NC}"
    log "Ping test to: $host"
    press_enter
}

listening_ports() {
    print_submenu_header "Listening Ports"
    if command -v ss &>/dev/null; then
        ss -tlnp 2>/dev/null || ss -tln
    elif command -v netstat &>/dev/null; then
        netstat -tlnp 2>/dev/null || netstat -tln
    else
        echo "Network tools not available"
    fi
    log "Viewed listening ports"
    press_enter
}

active_connections() {
    print_submenu_header "Active Connections"
    if command -v ss &>/dev/null; then
        ss -tn state established
    else
        netstat -tn 2>/dev/null | grep ESTABLISHED
    fi
    log "Viewed active connections"
    press_enter
}

routing_table() {
    print_submenu_header "Routing Table"
    if command -v ip &>/dev/null; then
        ip route
    else
        route -n 2>/dev/null || netstat -rn
    fi
    log "Viewed routing table"
    press_enter
}

# ====================
# File Operations
# ====================

file_menu() {
    while true; do
        print_banner
        print_submenu_header "File Operations"
        echo "1) Search for Files"
        echo "2) Find Large Files"
        echo "3) Directory Size"
        echo "4) Recently Modified Files"
        echo "5) File Type Statistics"
        echo "6) Back to Main Menu"
        echo ""
        read -p "Enter choice [1-6]: " choice
        
        case $choice in
            1) search_files ;;
            2) large_files ;;
            3) dir_size ;;
            4) recent_files ;;
            5) file_stats ;;
            6) return ;;
            *) echo -e "${RED}Invalid option${NC}" ;;
        esac
    done
}

search_files() {
    print_submenu_header "Search for Files"
    read -p "Enter search pattern: " pattern
    read -p "Enter directory to search (default: /home): " search_dir
    search_dir=${search_dir:-/home}
    echo ""
    echo "Searching for '$pattern' in $search_dir..."
    find "$search_dir" -name "*$pattern*" 2>/dev/null | head -20
    log "Searched for: $pattern in $search_dir"
    press_enter
}

large_files() {
    print_submenu_header "Large Files (Top 10)"
    read -p "Enter directory (default: /home): " search_dir
    search_dir=${search_dir:-/home}
    read -p "Minimum size in MB (default: 100): " min_size
    min_size=${min_size:-100}
    echo ""
    echo "Finding files larger than ${min_size}MB in $search_dir..."
    find "$search_dir" -type f -size +${min_size}M -exec ls -lh {} \; 2>/dev/null | sort -k5 -h | tail -10
    log "Found large files in: $search_dir"
    press_enter
}

dir_size() {
    print_submenu_header "Directory Size"
    read -p "Enter directory (default: /home): " target_dir
    target_dir=${target_dir:-/home}
    echo ""
    echo "Calculating sizes..."
    du -h --max-depth=1 "$target_dir" 2>/dev/null | sort -h
    log "Checked directory size: $target_dir"
    press_enter
}

recent_files() {
    print_submenu_header "Recently Modified Files"
    read -p "Enter directory (default: /home): " search_dir
    search_dir=${search_dir:-/home}
    read -p "Modified within last N days (default: 1): " days
    days=${days:-1}
    echo ""
    echo "Files modified in last $days day(s):"
    find "$search_dir" -type f -mtime -$days 2>/dev/null | head -20
    log "Found recent files in: $search_dir"
    press_enter
}

file_stats() {
    print_submenu_header "File Type Statistics"
    read -p "Enter directory (default: .): " target_dir
    target_dir=${target_dir:-.}
    echo ""
    echo "File types in $target_dir:"
    find "$target_dir" -type f 2>/dev/null | sed 's/.*\.//' | sort | uniq -c | sort -rn | head -10
    log "File stats for: $target_dir"
    press_enter
}

# ====================
# Service Management
# ====================

service_menu() {
    while true; do
        print_banner
        print_submenu_header "Service Management"
        echo "1) List Running Services"
        echo "2) Check Service Status"
        echo "3) View System Logs"
        echo "4) Show Boot Messages"
        echo "5) Back to Main Menu"
        echo ""
        read -p "Enter choice [1-5]: " choice
        
        case $choice in
            1) running_services ;;
            2) service_status ;;
            3) system_logs ;;
            4) boot_messages ;;
            5) return ;;
            *) echo -e "${RED}Invalid option${NC}" ;;
        esac
    done
}

running_services() {
    print_submenu_header "Running Services"
    if command -v systemctl &>/dev/null; then
        systemctl list-units --type=service --state=running | head -20
    else
        service --status-all 2>/dev/null | grep "+"
    fi
    log "Listed running services"
    press_enter
}

service_status() {
    print_submenu_header "Service Status"
    read -p "Enter service name: " service_name
    if command -v systemctl &>/dev/null; then
        systemctl status "$service_name" 2>&1 | head -15
    else
        service "$service_name" status 2>&1
    fi
    log "Checked status: $service_name"
    press_enter
}

system_logs() {
    print_submenu_header "System Logs (Last 20 lines)"
    if command -v journalctl &>/dev/null; then
        journalctl -n 20 --no-pager 2>/dev/null || echo "Permission denied. Try with sudo."
    else
        tail -20 /var/log/syslog 2>/dev/null || tail -20 /var/log/messages 2>/dev/null
    fi
    log "Viewed system logs"
    press_enter
}

boot_messages() {
    print_submenu_header "Boot Messages"
    dmesg 2>/dev/null | tail -20 || echo "Permission denied. Try with sudo."
    log "Viewed boot messages"
    press_enter
}

# ====================
# View Toolkit Log
# ====================

view_log() {
    print_submenu_header "Toolkit Log"
    if [ -f "$LOG_FILE" ]; then
        echo "Last 20 log entries:"
        echo ""
        tail -20 "$LOG_FILE"
    else
        echo "No log file found."
    fi
    press_enter
}

# ====================
# Help
# ====================

show_help() {
    print_banner
    print_submenu_header "Help"
    echo "Admin Toolkit v$VERSION"
    echo ""
    echo "This toolkit provides easy access to common system"
    echo "administration tasks through a menu-driven interface."
    echo ""
    echo "Features:"
    echo "  • System Information - View system, CPU, memory, disk info"
    echo "  • User Management - Manage and view user information"
    echo "  • Network Utilities - Network configuration and testing"
    echo "  • File Operations - Search and analyze files"
    echo "  • Service Management - View and check services"
    echo ""
    echo "Log file: $LOG_FILE"
    echo ""
    echo "Note: Some features may require sudo privileges."
    press_enter
}

# ====================
# Exit
# ====================

confirm_exit() {
    echo ""
    read -p "Are you sure you want to exit? [y/N]: " confirm
    if [[ "$confirm" =~ ^[Yy]$ ]]; then
        log "Toolkit closed"
        echo ""
        echo -e "${GREEN}Thank you for using Admin Toolkit!${NC}"
        echo ""
        exit 0
    fi
}

# ====================
# Main Loop
# ====================

main() {
    log "Toolkit started"
    
    while true; do
        print_banner
        print_menu
        read -p "Enter your choice [1-8]: " main_choice
        
        case $main_choice in
            1) system_menu ;;
            2) user_menu ;;
            3) network_menu ;;
            4) file_menu ;;
            5) service_menu ;;
            6) view_log ;;
            7) show_help ;;
            8) confirm_exit ;;
            *) echo -e "${RED}Invalid option. Please try again.${NC}"; sleep 1 ;;
        esac
    done
}

# Run main
main
```

</details>

---

## Bonus Exercises

### Bonus 1: Weather Script
Create a script that fetches weather information using `curl`:
```bash
curl "wttr.in/London?format=3"
```

### Bonus 2: Process Monitor
Create a script that monitors a specific process and alerts if it stops.

### Bonus 3: Auto-Cleanup Script
Create a script that:
- Removes files older than 30 days from /tmp
- Clears package cache
- Empties trash

### Bonus 4: SSH Connection Manager
Create a menu-driven script to manage SSH connections to different servers.

---

## Summary

### ✅ What You Practiced

In these exercises, you practiced:

1. **Basic Scripting**
   - Creating and running scripts
   - Using the shebang line
   - Adding execute permissions

2. **Variables and Input**
   - Working with variables
   - Reading user input
   - Processing command-line arguments

3. **Control Structures**
   - If/else conditions
   - Case statements
   - For and while loops

4. **Functions**
   - Creating modular code
   - Passing arguments to functions
   - Returning values

5. **System Administration**
   - Gathering system information
   - Creating backups
   - Managing users
   - Monitoring disk space
   - Working with services

6. **Best Practices**
   - Error handling
   - Logging
   - Code organization
   - User-friendly output

---

## Next Steps

Now that you've completed all exercises:

1. **Practice Regularly** - Create scripts for your daily tasks
2. **Explore Advanced Topics** - Learn about sed, awk, and regex
3. **Set Up Cron Jobs** - Schedule your scripts to run automatically
4. **Version Control** - Use Git to track your scripts
5. **Share Knowledge** - Help others learn what you've learned

🎉 **Congratulations on completing the Linux 101 course!**