# Module 4: Viewing and Editing Text

## 🎯 Learning Goal

By the end of this module, you will be able to view file contents, navigate through large files, search for text patterns, use pipes and redirection, and edit files using nano and vim editors.

---

## Introduction

Text files are at the heart of Linux. Configuration files, logs, scripts, and data are all stored as text. Being able to view, search, and edit text efficiently is one of the most important skills for any Linux user. In this module, you'll learn various tools for working with text, from simple viewing to powerful searching and editing.

---

## 1. Viewing File Contents

### The `cat` Command

The `cat` (concatenate) command is the simplest way to display file contents:

```bash
# Display entire file
cat filename

# Display with line numbers
cat -n filename

# Display multiple files (concatenate them)
cat file1 file2

# Display multiple files with line numbers
cat -n file1 file2
```

**Example:**
```bash
cat /etc/hostname
```

### The `tac` Command

`tac` is `cat` spelled backwards - it displays files in reverse order (last line first):

```bash
tac filename
```

This is useful when you want to see the most recent entries first (like in log files).

💡 **Tip:** `cat` is perfect for small files. For larger files, use `less` or `more` instead to avoid flooding your terminal.

---

## 2. Viewing Large Files

When files are too large to fit on one screen, you need paginated viewing.

### The `less` Command (Recommended)

`less` is the most powerful pager - it lets you scroll forward and backward:

```bash
less filename
```

**Navigation in `less`:**

| Key | Action |
|-----|--------|
| `Space` or `f` | Move forward one page |
| `b` | Move backward one page |
| `g` | Go to the beginning of file |
| `G` | Go to the end of file |
| `/pattern` | Search forward for "pattern" |
| `?pattern` | Search backward for "pattern" |
| `n` | Go to next search match |
| `N` | Go to previous search match |
| `q` | Quit less |
| `h` | Display help |

**Example:**
```bash
less /var/log/syslog
```

💡 **Tip:** Remember "less is more" - `less` has more features than `more` and is the preferred pager.

### The `more` Command

`more` is an older, simpler pager (can only scroll forward):

```bash
more filename
```

Press `Space` to advance, `q` to quit.

### The `head` Command

`head` displays the first lines of a file (default: 10 lines):

```bash
# Show first 10 lines
head filename

# Show first 20 lines
head -n 20 filename

# Short form
head -20 filename
```

**Example:**
```bash
head /etc/passwd
head -n 5 /etc/passwd
```

### The `tail` Command

`tail` displays the last lines of a file (default: 10 lines):

```bash
# Show last 10 lines
tail filename

# Show last 20 lines
tail -n 20 filename

# Short form
tail -20 filename
```

### Following Live Updates with `tail -f`

One of the most useful features - watch a file as it grows in real-time:

```bash
tail -f /var/log/syslog
```

This is invaluable for monitoring log files! Press `Ctrl+C` to stop watching.

💡 **Tip:** Use `tail -f` when troubleshooting services - you can watch logs update as things happen.

---

## 3. Text Processing Commands

### The `grep` Command

`grep` searches for patterns in files - one of the most powerful Linux tools:

```bash
# Basic search
grep pattern filename

# Case-insensitive search
grep -i pattern filename

# Show line numbers
grep -n pattern filename

# Recursive search in directories
grep -r pattern directory

# Invert match (show lines NOT containing pattern)
grep -v pattern filename

# Count matching lines
grep -c pattern filename

# Show only filenames with matches
grep -l pattern *.txt
```

**Examples:**
```bash
# Find "root" in passwd file
grep root /etc/passwd

# Find "error" (case-insensitive) in logs
grep -i error /var/log/syslog

# Find all occurrences in a directory
grep -r "password" /etc/

# Find lines WITHOUT "comment"
grep -v "#" /etc/fstab
```

💡 **Tip:** `grep` supports regular expressions for advanced pattern matching. Start simple, then learn regex as needed.

### The `wc` Command (Word Count)

`wc` counts lines, words, and characters:

```bash
# Show lines, words, and characters
wc filename

# Count lines only
wc -l filename

# Count words only
wc -w filename

# Count characters only
wc -c filename
```

**Example:**
```bash
# How many users in the system?
wc -l /etc/passwd
```

### The `sort` Command

`sort` arranges lines in order:

```bash
# Sort alphabetically
sort filename

# Sort in reverse order
sort -r filename

# Sort numerically
sort -n filename

# Sort by specific field (column)
sort -k 2 filename
```

### The `uniq` Command

`uniq` removes duplicate consecutive lines:

```bash
# Remove duplicates (file must be sorted first!)
sort filename | uniq

# Count occurrences
sort filename | uniq -c

# Show only duplicates
sort filename | uniq -d
```

⚠️ **Warning:** `uniq` only removes *consecutive* duplicates. Always `sort` first!

---

## 4. Output Redirection and Pipes

### Output Redirection

Redirect command output to files:

```bash
# Redirect output to file (overwrites!)
command > filename

# Append output to file
command >> filename

# Redirect errors to file
command 2> errorfile

# Redirect both output and errors
command &> alloutput

# Redirect output and errors separately
command > output.txt 2> errors.txt
```

**Examples:**
```bash
# Save file listing
ls -la > filelist.txt

# Append current date to a log
date >> mylog.txt

# Save only errors
grep something /nonexistent 2> errors.txt
```

⚠️ **Warning:** Using `>` overwrites the file completely! Use `>>` to append instead.

### Pipes (`|`)

Pipes send output from one command as input to another:

```bash
command1 | command2 | command3
```

**Examples:**
```bash
# Search within command output
cat /etc/passwd | grep bash

# Count users with bash shell
grep bash /etc/passwd | wc -l

# Sort and remove duplicates
cat names.txt | sort | uniq

# View long output page by page
ls -la /etc | less

# Find largest files
ls -la | sort -k 5 -n | tail -10
```

### Common Pipe Combinations

```bash
# Find and count specific entries
grep "pattern" file | wc -l

# Search, sort, and display
cat file | grep "something" | sort | less

# Process and save
command | grep "filter" | sort > result.txt
```

💡 **Tip:** Think of pipes as an assembly line - each command processes data and passes it to the next.

---

## 5. The nano Text Editor

`nano` is a simple, beginner-friendly text editor that runs in the terminal.

### Opening Files

```bash
# Open existing file
nano filename

# Open file at specific line number
nano +10 filename

# Open new file
nano newfile.txt
```

### Basic Navigation

| Key | Action |
|-----|--------|
| Arrow keys | Move cursor |
| `Page Up` | Scroll up one page |
| `Page Down` | Scroll down one page |
| `Ctrl+A` | Go to beginning of line |
| `Ctrl+E` | Go to end of line |
| `Ctrl+Y` | Scroll up |
| `Ctrl+V` | Scroll down |

### Essential Keyboard Shortcuts

The `^` symbol means `Ctrl`. For example, `^O` means `Ctrl+O`.

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Save file (Write Out) |
| `Ctrl+X` | Exit nano |
| `Ctrl+K` | Cut current line |
| `Ctrl+U` | Paste (Uncut) |
| `Ctrl+W` | Search for text |
| `Ctrl+\` | Search and replace |
| `Ctrl+G` | Display help |
| `Ctrl+C` | Show cursor position |

### The Bottom Help Bar

nano displays available commands at the bottom of the screen:

```
^G Get Help  ^O Write Out ^W Where Is  ^K Cut Text  ^J Justify
^X Exit      ^R Read File ^\ Replace   ^U Uncut Text^T To Spell
```

This is your cheat sheet! The shortcuts shown change based on context.

### Typical nano Workflow

1. Open file: `nano myfile.txt`
2. Make your edits using arrow keys and typing
3. Save: `Ctrl+O`, then press `Enter` to confirm filename
4. Exit: `Ctrl+X`

💡 **Tip:** If you try to exit without saving, nano will ask if you want to save changes. Press `Y` for yes, `N` for no.

### Creating New Files

Simply open nano with a new filename:

```bash
nano mynotes.txt
```

Type your content, then `Ctrl+O` to save and `Ctrl+X` to exit.

---

## 6. Introduction to vim (Basics Only)

### Why Learn vim?

`vim` is a powerful text editor found on virtually every Linux/Unix system. While it has a steep learning curve, knowing basic vim commands is essential because:

- It's the default editor on many systems
- Some commands open vim automatically
- It's incredibly powerful once mastered
- You might find yourself stuck in vim with no way out!

### Opening vim

```bash
# Open vim with a file
vim filename

# Open vim without a file
vim
```

### Understanding vim Modes

vim has different modes - this is what confuses beginners most:

| Mode | Purpose | How to Enter |
|------|---------|--------------|
| **Normal** | Navigate and execute commands | Press `Esc` |
| **Insert** | Type and edit text | Press `i` |
| **Command** | Execute commands like save/quit | Press `:` (from Normal mode) |

⚠️ **Warning:** When you open vim, you're in Normal mode. You CAN'T type text until you enter Insert mode!

### Essential Survival Commands

**Entering Insert Mode (to type text):**
- `i` - Insert before cursor
- `a` - Insert after cursor
- `o` - Open new line below

**Returning to Normal Mode:**
- `Esc` - Always returns you to Normal mode

**Saving and Quitting (from Normal mode, type these commands):**

| Command | Action |
|---------|--------|
| `:w` | Save (write) the file |
| `:q` | Quit (only works if saved) |
| `:wq` | Save and quit |
| `:q!` | Quit WITHOUT saving (force quit) |
| `:x` | Save and quit (same as `:wq`) |

### Basic vim Workflow

1. Open file: `vim filename`
2. You're in Normal mode - press `i` to enter Insert mode
3. Type your text
4. Press `Esc` to return to Normal mode
5. Type `:wq` and press `Enter` to save and quit

### If You Get Stuck!

If vim behaves strangely, press `Esc` several times, then type `:q!` and press `Enter` to force quit without saving.

💡 **Tip:** Run `vimtutor` in your terminal for an interactive vim tutorial. It takes about 30 minutes and teaches you the basics hands-on.

---

## 7. Practical Use Cases

### Viewing Configuration Files

Configuration files live in `/etc`:

```bash
# View network configuration
cat /etc/hostname
cat /etc/hosts

# View user information
less /etc/passwd

# View group information
less /etc/group
```

### Editing Configuration Files

⚠️ **Warning:** Always back up configuration files before editing!

```bash
# Backup first
sudo cp /etc/hosts /etc/hosts.backup

# Edit with nano
sudo nano /etc/hosts
```

### Searching Through Logs

Log files are in `/var/log`:

```bash
# View recent system logs
tail -50 /var/log/syslog

# Watch logs in real-time
tail -f /var/log/syslog

# Search for errors
grep -i error /var/log/syslog

# Find authentication issues
grep -i "authentication" /var/log/auth.log
```

### Creating Simple Text Files

```bash
# Quick file creation with echo
echo "Hello World" > hello.txt

# Multi-line file with cat
cat > notes.txt << EOF
Line 1
Line 2
Line 3
EOF

# Create and edit with nano
nano todo.txt
```

---

## Command Reference Table

| Command | Description |
|---------|-------------|
| `cat file` | Display file contents |
| `cat -n file` | Display with line numbers |
| `tac file` | Display in reverse |
| `less file` | Paginated viewing |
| `more file` | Simple paginated viewing |
| `head file` | First 10 lines |
| `head -n X file` | First X lines |
| `tail file` | Last 10 lines |
| `tail -n X file` | Last X lines |
| `tail -f file` | Follow file updates |
| `grep pattern file` | Search for pattern |
| `grep -i pattern file` | Case-insensitive search |
| `grep -r pattern dir` | Recursive search |
| `grep -v pattern file` | Invert match |
| `wc file` | Count lines/words/chars |
| `wc -l file` | Count lines only |
| `sort file` | Sort lines |
| `uniq` | Remove duplicates |
| `>` | Redirect (overwrite) |
| `>>` | Redirect (append) |
| `\|` | Pipe to next command |
| `nano file` | Edit with nano |
| `vim file` | Edit with vim |

---

## ✅ What You Learned

In this module, you learned how to:

- ✅ View file contents with `cat`, `less`, `more`, `head`, and `tail`
- ✅ Follow live log updates with `tail -f`
- ✅ Search for text patterns with `grep`
- ✅ Count lines, words, and characters with `wc`
- ✅ Sort and remove duplicates with `sort` and `uniq`
- ✅ Redirect output to files with `>` and `>>`
- ✅ Chain commands together with pipes (`|`)
- ✅ Edit files with the nano text editor
- ✅ Survive and exit vim
- ✅ View and edit configuration files
- ✅ Search through system logs

---

## Next Steps

In Module 5, you'll learn about file permissions and ownership - understanding who can read, write, and execute files on a Linux system.

---

💡 **Pro Tip:** Practice these commands daily! Try viewing different files in `/etc` and `/var/log`. The more you use these tools, the more natural they become.