# Text Editing & Processing Cheatsheet

> Quick reference for text editors, patterns, and text processing tools.  
> *For detailed explanations, see Module 4 of this course.*

---

## nano Editor

### Basic Commands

| Shortcut | Action |
|----------|--------|
| `nano file.txt` | Open/create file |
| `Ctrl+O` | Save (Write Out) |
| `Ctrl+X` | Exit |
| `Ctrl+G` | Help |

### Navigation

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Go to beginning of line |
| `Ctrl+E` | Go to end of line |
| `Ctrl+Y` | Page up |
| `Ctrl+V` | Page down |
| `Ctrl+_` | Go to line number |
| `Ctrl+C` | Show current position |

### Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+K` | Cut current line |
| `Ctrl+U` | Paste (uncut) |
| `Ctrl+J` | Justify paragraph |
| `Ctrl+\` | Search and replace |
| `Alt+U` | Undo |
| `Alt+E` | Redo |

### Search

| Shortcut | Action |
|----------|--------|
| `Ctrl+W` | Search forward |
| `Alt+W` | Repeat last search |
| `Ctrl+\` | Search and replace |

### Useful Options

```bash
nano -l file.txt      # Show line numbers
nano -i file.txt      # Auto-indent
nano -m file.txt      # Enable mouse
nano -B file.txt      # Backup before editing
nano +10 file.txt     # Open at line 10
```

---

## vim Editor (Survival Guide)

### Modes

| Mode | Purpose | Enter with |
|------|---------|------------|
| Normal | Navigate, commands | `Esc` |
| Insert | Type text | `i`, `a`, `o` |
| Visual | Select text | `v`, `V` |
| Command | Execute commands | `:` |

### Essential Commands

#### Starting vim
```bash
vim file.txt          # Open file
vim +10 file.txt      # Open at line 10
vim +/pattern file    # Open at pattern
```

#### Exiting (The Most Important!)
| Command | Action |
|---------|--------|
| `:q` | Quit (if no changes) |
| `:q!` | Quit without saving |
| `:w` | Save |
| `:wq` or `:x` | Save and quit |
| `ZZ` | Save and quit (Normal mode) |
| `ZQ` | Quit without saving (Normal mode) |

#### Entering Insert Mode
| Key | Action |
|-----|--------|
| `i` | Insert before cursor |
| `a` | Insert after cursor |
| `I` | Insert at line beginning |
| `A` | Insert at line end |
| `o` | New line below |
| `O` | New line above |

#### Navigation (Normal Mode)
| Key | Action |
|-----|--------|
| `h j k l` | Left, down, up, right |
| `w` | Next word |
| `b` | Previous word |
| `0` | Start of line |
| `$` | End of line |
| `gg` | First line |
| `G` | Last line |
| `10G` | Go to line 10 |
| `Ctrl+f` | Page down |
| `Ctrl+b` | Page up |

#### Editing (Normal Mode)
| Command | Action |
|---------|--------|
| `x` | Delete character |
| `dd` | Delete line |
| `dw` | Delete word |
| `D` | Delete to end of line |
| `yy` | Copy (yank) line |
| `yw` | Copy word |
| `p` | Paste after cursor |
| `P` | Paste before cursor |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `.` | Repeat last command |

#### Search
| Command | Action |
|---------|--------|
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Next match |
| `N` | Previous match |
| `*` | Search word under cursor |

#### Search and Replace
```vim
:s/old/new/          " Replace first on line
:s/old/new/g         " Replace all on line
:%s/old/new/g        " Replace all in file
:%s/old/new/gc       " Replace all with confirmation
```

---

## grep Patterns

### Basic Usage

| Command | Description |
|---------|-------------|
| `grep "pattern" file` | Search for pattern |
| `grep -i "pattern" file` | Case insensitive |
| `grep -v "pattern" file` | Invert match |
| `grep -n "pattern" file` | Show line numbers |
| `grep -c "pattern" file` | Count matches |
| `grep -l "pattern" files` | List matching files |
| `grep -r "pattern" dir/` | Recursive search |
| `grep -w "word" file` | Match whole word |

### Regular Expression Patterns

| Pattern | Matches |
|---------|---------|
| `.` | Any single character |
| `*` | Zero or more of previous |
| `+` | One or more of previous (use -E) |
| `?` | Zero or one of previous (use -E) |
| `^` | Start of line |
| `$` | End of line |
| `[abc]` | Any character in brackets |
| `[^abc]` | Any character NOT in brackets |
| `[a-z]` | Range of characters |
| `\d` | Digit (use -P for Perl regex) |
| `\w` | Word character |
| `\s` | Whitespace |
| `\b` | Word boundary |

### Extended Regex (-E or egrep)

```bash
# Using extended regex
grep -E "pattern1|pattern2" file    # OR
grep -E "colou?r" file              # Optional character
grep -E "go+d" file                 # One or more
grep -E "[0-9]{3}" file             # Exactly 3 digits
```

### Common Examples

```bash
# Find lines starting with #
grep "^#" config.conf

# Find empty lines
grep "^$" file.txt

# Find lines ending with period
grep "\.$" file.txt

# Find email addresses (basic)
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file

# Find IP addresses (basic)
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" file

# Find words containing "error" or "Error" or "ERROR"
grep -i "error" logfile

# Count lines containing "TODO"
grep -c "TODO" *.py

# Find lines NOT containing pattern
grep -v "DEBUG" logfile
```

---

## Redirection and Pipes

### Output Redirection

| Operator | Description | Example |
|----------|-------------|---------|
| `>` | Redirect stdout (overwrite) | `ls > files.txt` |
| `>>` | Redirect stdout (append) | `echo "text" >> file.txt` |
| `2>` | Redirect stderr | `cmd 2> errors.txt` |
| `2>>` | Redirect stderr (append) | `cmd 2>> errors.txt` |
| `&>` | Redirect both stdout and stderr | `cmd &> all.txt` |
| `2>&1` | Redirect stderr to stdout | `cmd > out.txt 2>&1` |

### Input Redirection

| Operator | Description | Example |
|----------|-------------|---------|
| `<` | Redirect stdin | `sort < unsorted.txt` |
| `<<` | Here document | `cat << EOF` |
| `<<<` | Here string | `grep "x" <<< "text"` |

### Pipes

```bash
# Basic pipe: send output of one command to another
command1 | command2 | command3

# Examples
ls -la | grep ".txt"                # Filter ls output
cat file.txt | sort | uniq          # Sort and remove duplicates
ps aux | grep nginx                  # Find nginx processes
history | tail -20                   # Last 20 commands
cat access.log | cut -d' ' -f1 | sort | uniq -c | sort -rn
```

### Special Redirections

```bash
# Discard output
command > /dev/null

# Discard all output
command &> /dev/null

# Discard errors only
command 2> /dev/null

# Tee: write to file AND stdout
command | tee output.txt
command | tee -a output.txt    # Append mode
```

---

## Text Processing Commands

### sort

| Option | Description | Example |
|--------|-------------|---------|
| `sort` | Sort alphabetically | `sort names.txt` |
| `sort -n` | Sort numerically | `sort -n numbers.txt` |
| `sort -r` | Reverse sort | `sort -r file.txt` |
| `sort -k2` | Sort by 2nd field | `sort -k2 data.txt` |
| `sort -t:` | Set delimiter | `sort -t: -k3 -n /etc/passwd` |
| `sort -u` | Unique values only | `sort -u file.txt` |

### uniq

| Option | Description | Example |
|--------|-------------|---------|
| `uniq` | Remove adjacent duplicates | `sort file \| uniq` |
| `uniq -c` | Count occurrences | `sort file \| uniq -c` |
| `uniq -d` | Show only duplicates | `sort file \| uniq -d` |
| `uniq -u` | Show only unique lines | `sort file \| uniq -u` |

### wc (Word Count)

| Option | Description | Example |
|--------|-------------|---------|
| `wc file` | Lines, words, bytes | `wc file.txt` |
| `wc -l` | Count lines | `wc -l file.txt` |
| `wc -w` | Count words | `wc -w file.txt` |
| `wc -c` | Count bytes | `wc -c file.txt` |
| `wc -m` | Count characters | `wc -m file.txt` |

### cut

| Option | Description | Example |
|--------|-------------|---------|
| `cut -c1-5` | Characters 1-5 | `cut -c1-5 file.txt` |
| `cut -d: -f1` | Field 1, delimiter : | `cut -d: -f1 /etc/passwd` |
| `cut -d, -f1,3` | Fields 1 and 3 | `cut -d, -f1,3 data.csv` |
| `cut -d' ' -f2-` | Field 2 onwards | `cut -d' ' -f2- file.txt` |

### tr (Translate)

| Usage | Description | Example |
|-------|-------------|---------|
| `tr 'a-z' 'A-Z'` | Lowercase to uppercase | `echo "hello" \| tr 'a-z' 'A-Z'` |
| `tr -d 'chars'` | Delete characters | `echo "a1b2c3" \| tr -d '0-9'` |
| `tr -s ' '` | Squeeze repeats | `echo "a  b  c" \| tr -s ' '` |
| `tr '\n' ' '` | Replace newlines | `cat file \| tr '\n' ' '` |

### sed (Stream Editor)

| Command | Description | Example |
|---------|-------------|---------|
| `sed 's/old/new/'` | Replace first occurrence | `sed 's/foo/bar/' file` |
| `sed 's/old/new/g'` | Replace all | `sed 's/foo/bar/g' file` |
| `sed -i` | Edit in place | `sed -i 's/foo/bar/g' file` |
| `sed '5d'` | Delete line 5 | `sed '5d' file` |
| `sed '/pattern/d'` | Delete matching lines | `sed '/^#/d' file` |
| `sed -n '5,10p'` | Print lines 5-10 | `sed -n '5,10p' file` |

### awk (Pattern Processing)

| Command | Description | Example |
|---------|-------------|---------|
| `awk '{print $1}'` | Print 1st field | `awk '{print $1}' file` |
| `awk '{print $NF}'` | Print last field | `awk '{print $NF}' file` |
| `awk -F:` | Set field separator | `awk -F: '{print $1}' /etc/passwd` |
| `awk '/pattern/'` | Print matching lines | `awk '/error/' log` |
| `awk '{sum+=$1} END {print sum}'` | Sum values | Calculate total |

---

## Common Pipelines

```bash
# Count unique IPs in access log
cut -d' ' -f1 access.log | sort | uniq -c | sort -rn

# Find largest files
du -ah /home | sort -rh | head -10

# List users sorted by UID
cut -d: -f1,3 /etc/passwd | sort -t: -k2 -n

# Count file types in directory
find . -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn

# Extract and sort error messages
grep "ERROR" logfile | cut -d: -f4 | sort | uniq -c | sort -rn

# Replace text in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Word frequency count
cat file | tr -s ' ' '\n' | sort | uniq -c | sort -rn | head -10
```

---

*Reference: Module 4 - Viewing and Editing Text*