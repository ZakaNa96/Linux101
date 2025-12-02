# Module 3: Exercises - Working with Files and Directories

## 🎯 Exercise Overview

These exercises will help you practice creating, copying, moving, and deleting files and directories. You'll also master wildcards and file searching.

**Before You Begin:**
- Open your terminal in Kali Linux
- Navigate to your home directory: `cd ~`
- Create a practice directory: `mkdir -p ~/linux-practice/module3`
- Enter the practice directory: `cd ~/linux-practice/module3`

---

## Exercise 1: Create a Project Structure

### Objective
Learn to create directories and files to build a complete project structure.

### Tasks

1. Create a directory called `my-project`
2. Inside `my-project`, create these subdirectories:
   - `src` (for source code)
   - `docs` (for documentation)
   - `tests` (for test files)
3. Create these empty files:
   - `README.md` in the project root
   - `main.sh` inside `src`
   - `notes.txt` inside `docs`
4. Verify your structure with `ls -R`

### Step-by-Step Solution

<details>
<summary>💡 Hint 1: Creating directories</summary>

You can create multiple directories at once with:
```bash
mkdir dir1 dir2 dir3
```

Or create nested structures with:
```bash
mkdir -p parent/child/grandchild
```

</details>

<details>
<summary>💡 Hint 2: Creating files in subdirectories</summary>

You can create files in subdirectories directly:
```bash
touch directory/filename.txt
```

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Step 1: Create the project directory
mkdir my-project

# Step 2: Create subdirectories (two methods)
# Method A: Create one at a time
cd my-project
mkdir src
mkdir docs
mkdir tests

# Method B: Create all at once (from parent directory)
# mkdir my-project/src my-project/docs my-project/tests

# Step 3: Create the files
touch README.md
touch src/main.sh
touch docs/notes.txt

# Step 4: Verify the structure
ls -R
```

</details>

### Expected Output

```
my-project:
README.md  docs  src  tests

my-project/docs:
notes.txt

my-project/src:
main.sh

my-project/tests:
```

### ✅ Checklist
- [ ] `my-project` directory exists
- [ ] Three subdirectories created: `src`, `docs`, `tests`
- [ ] `README.md` in project root
- [ ] `main.sh` in `src` directory
- [ ] `notes.txt` in `docs` directory

---

## Exercise 2: Copy and Backup

### Objective
Practice copying files and directories for backup purposes.

### Prerequisites
Make sure you completed Exercise 1 and are in the `my-project` directory.

### Tasks

1. Copy `README.md` to `README.backup`
2. Copy the entire `docs` folder to `docs-backup`
3. Copy `main.sh` to the `tests` directory
4. Verify all copies exist

### Step-by-Step Solution

<details>
<summary>💡 Hint 1: Copying files</summary>

Basic file copy:
```bash
cp original.txt copy.txt
```

</details>

<details>
<summary>💡 Hint 2: Copying directories</summary>

Directories require the `-r` (recursive) flag:
```bash
cp -r source-dir destination-dir
```

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Make sure you're in my-project directory
cd ~/linux-practice/module3/my-project

# Step 1: Copy README.md
cp README.md README.backup

# Step 2: Copy docs directory
cp -r docs docs-backup

# Step 3: Copy main.sh to tests
cp src/main.sh tests/

# Step 4: Verify
ls -la
ls -la docs-backup
ls -la tests
```

</details>

### Expected Output

```bash
# After ls -la in my-project:
README.backup
README.md
docs
docs-backup
src
tests

# After ls -la docs-backup:
notes.txt

# After ls -la tests:
main.sh
```

### ✅ Checklist
- [ ] `README.backup` exists alongside `README.md`
- [ ] `docs-backup` directory exists with `notes.txt` inside
- [ ] `main.sh` exists in `tests` directory
- [ ] Original files still exist (copy, not move!)

---

## Exercise 3: Move and Rename

### Objective
Practice moving and renaming files and directories.

### Prerequisites
Complete Exercises 1 and 2.

### Tasks

1. Rename `docs/notes.txt` to `docs/documentation.txt`
2. Move `src/main.sh` to the project root directory
3. Rename the entire `my-project` directory to `linux-project`
4. Verify all changes

### Step-by-Step Solution

<details>
<summary>💡 Hint 1: Renaming files</summary>

Renaming is just moving to a new name:
```bash
mv oldname.txt newname.txt
```

</details>

<details>
<summary>💡 Hint 2: Moving to parent directory</summary>

Use `..` to reference the parent directory:
```bash
mv file.txt ../
```

Or use full path:
```bash
mv src/file.txt ./
```

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Make sure you're in the project directory
cd ~/linux-practice/module3/my-project

# Step 1: Rename notes.txt to documentation.txt
mv docs/notes.txt docs/documentation.txt

# Step 2: Move main.sh to project root
mv src/main.sh ./
# Or: mv src/main.sh .

# Step 3: Go up one level to rename the project
cd ..
mv my-project linux-project

# Step 4: Verify changes
ls -la linux-project
ls linux-project/docs
ls linux-project/src
```

</details>

### Expected Output

```bash
# After ls -la linux-project:
README.backup
README.md
docs
docs-backup
main.sh        # Now in root!
src
tests

# After ls linux-project/docs:
documentation.txt   # Renamed from notes.txt

# After ls linux-project/src:
# (empty - main.sh was moved out)
```

### ✅ Checklist
- [ ] `documentation.txt` exists in `docs` (renamed from `notes.txt`)
- [ ] `main.sh` is now in the project root (moved from `src`)
- [ ] `src` directory is now empty
- [ ] Project folder is now called `linux-project`

---

## Exercise 4: Safe Deletion Practice

### Objective
Learn to delete files and directories safely using confirmation prompts.

### ⚠️ Warning
Pay close attention in this exercise! Deleted files cannot be recovered.

### Prerequisites
Complete Exercises 1-3.

### Tasks

1. Delete `README.backup` with confirmation (`-i` flag)
2. Delete the empty `src` directory using `rmdir`
3. Delete the `docs-backup` directory with all contents using `rm -r`
4. Try deleting `tests` with `rmdir` and observe the error
5. Delete `tests` correctly

### Step-by-Step Solution

<details>
<summary>💡 Hint 1: Safe deletion</summary>

Always use `-i` to be asked for confirmation:
```bash
rm -i filename
```

Type `y` for yes, `n` for no.

</details>

<details>
<summary>💡 Hint 2: Empty vs non-empty directories</summary>

- `rmdir` only works on empty directories
- `rm -r` removes directories with content

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Navigate to the project
cd ~/linux-practice/module3/linux-project

# Step 1: Delete README.backup with confirmation
rm -i README.backup
# Answer: y (yes)

# Step 2: Delete empty src directory
rmdir src

# Step 3: Delete docs-backup with contents
rm -r docs-backup
# Or with confirmation: rm -ri docs-backup

# Step 4: Try to delete tests with rmdir
rmdir tests
# Error: rmdir: failed to remove 'tests': Directory not empty

# Step 5: Delete tests correctly
rm -r tests

# Verify
ls -la
```

</details>

### Expected Output

```bash
# After rm -i README.backup:
rm: remove regular empty file 'README.backup'? y

# After rmdir src:
# (no output = success)

# After rmdir tests (the error):
rmdir: failed to remove 'tests': Directory not empty

# Final ls -la:
README.md
docs
main.sh
```

### ✅ Checklist
- [ ] `README.backup` deleted with confirmation
- [ ] `src` directory removed (was empty)
- [ ] `docs-backup` directory and contents removed
- [ ] Observed the `rmdir` error for non-empty directory
- [ ] `tests` directory removed properly
- [ ] Only `README.md`, `docs`, and `main.sh` remain

---

## Exercise 5: Wildcard Mastery

### Objective
Master wildcards for efficient batch file operations.

### Tasks

1. Create test files:
   - `test1.txt`, `test2.txt`, `test3.txt`
   - `test1.log`, `test2.log`, `test3.log`
   - `data.txt`, `report.txt`
   - `backup.tar`, `archive.zip`

2. List all `.txt` files using wildcards
3. List all files starting with `test`
4. List files matching `test?.txt` pattern
5. Create a `txt-backup` folder
6. Copy all `.txt` files to `txt-backup`
7. Delete all `.log` files

### Step-by-Step Solution

<details>
<summary>💡 Hint 1: Creating multiple files</summary>

You can create multiple files at once:
```bash
touch file1.txt file2.txt file3.txt
```

Or use brace expansion:
```bash
touch test{1,2,3}.txt
```

</details>

<details>
<summary>💡 Hint 2: Wildcard patterns</summary>

- `*.txt` - all files ending in .txt
- `test*` - all files starting with "test"
- `test?.txt` - test + single character + .txt

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Navigate to project
cd ~/linux-practice/module3/linux-project

# Step 1: Create test files
touch test1.txt test2.txt test3.txt
touch test1.log test2.log test3.log
touch data.txt report.txt
touch backup.tar archive.zip

# Verify creation
ls -la

# Step 2: List all .txt files
ls *.txt

# Step 3: List all files starting with "test"
ls test*

# Step 4: List files matching test?.txt
ls test?.txt

# Step 5: Create backup folder
mkdir txt-backup

# Step 6: Copy all .txt files to backup
cp *.txt txt-backup/

# Verify copy
ls txt-backup/

# Step 7: Delete all .log files (safely)
# First, check what will be deleted
ls *.log
# Then delete
rm *.log

# Final verification
ls -la
```

</details>

### Expected Output

```bash
# ls *.txt:
data.txt  report.txt  test1.txt  test2.txt  test3.txt

# ls test*:
test1.log  test1.txt  test2.log  test2.txt  test3.log  test3.txt

# ls test?.txt:
test1.txt  test2.txt  test3.txt

# ls txt-backup/:
data.txt  report.txt  test1.txt  test2.txt  test3.txt

# Final ls (no .log files):
README.md  archive.zip  backup.tar  data.txt  docs  main.sh
report.txt  test1.txt  test2.txt  test3.txt  txt-backup
```

### ✅ Checklist
- [ ] All test files created
- [ ] `*.txt` shows only text files
- [ ] `test*` shows all test files
- [ ] `test?.txt` shows test1-3.txt (not .log files)
- [ ] `txt-backup` contains all .txt files
- [ ] All .log files deleted
- [ ] Original .txt files still exist

---

## Exercise 6: Find and Locate

### Objective
Learn to search for files using various methods.

### Tasks

1. Find all `.conf` files in `/etc` (may need sudo)
2. Find all directories named `log` in `/var`
3. Find where the `bash` command is located
4. Find all `.txt` files in your home directory
5. Find files larger than 1MB in `/var/log`
6. Find files modified in the last day in your practice directory

### Step-by-Step Solution

<details>
<summary>💡 Hint 1: find syntax</summary>

```bash
find [where] [criteria] [options]
```

- `-name "pattern"` - search by name
- `-type f` - files only
- `-type d` - directories only

</details>

<details>
<summary>💡 Hint 2: Using which and whereis</summary>

```bash
which command    # Find command path
whereis command  # Find binary, source, manual
```

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Step 1: Find .conf files in /etc
find /etc -name "*.conf" 2>/dev/null | head -20
# Note: 2>/dev/null hides permission errors
# head -20 limits output to first 20 results

# Step 2: Find directories named "log" in /var
sudo find /var -type d -name "log"

# Step 3: Find bash command location
which bash
whereis bash

# Step 4: Find .txt files in home directory
find ~ -name "*.txt" -type f 2>/dev/null

# Step 5: Find files larger than 1MB in /var/log
sudo find /var/log -type f -size +1M

# Step 6: Find files modified in last day
find ~/linux-practice/module3 -mtime -1
# Or for last hour:
find ~/linux-practice/module3 -mmin -60
```

</details>

### Expected Output (examples)

```bash
# find /etc -name "*.conf":
/etc/adduser.conf
/etc/ca-certificates.conf
/etc/debconf.conf
# ... more files

# find /var -type d -name "log":
/var/log

# which bash:
/usr/bin/bash

# whereis bash:
bash: /usr/bin/bash /etc/bash.bashrc /usr/share/man/man1/bash.1.gz

# find ~ -name "*.txt":
/home/yourusername/linux-practice/module3/linux-project/test1.txt
# ... more files
```

### ✅ Checklist
- [ ] Found .conf files in /etc
- [ ] Found log directories in /var
- [ ] Found bash location with `which`
- [ ] Got extended info with `whereis`
- [ ] Found .txt files in home directory
- [ ] Found large files in /var/log
- [ ] Found recently modified files

---

## Challenge Exercise: Organize Downloads

### Objective
Simulate organizing a messy downloads folder using everything you've learned.

### Scenario
You have a messy downloads folder with various file types. Your task is to organize them into categorized subfolders.

### Setup

First, create the mock downloads structure:

```bash
# Create and enter downloads folder
mkdir -p ~/linux-practice/module3/downloads
cd ~/linux-practice/module3/downloads

# Create mock downloaded files
touch photo1.jpg photo2.jpg photo3.png screenshot.png
touch document1.pdf document2.pdf report.docx notes.txt
touch song1.mp3 song2.mp3 video.mp4 movie.mkv
touch setup.exe installer.deb package.tar.gz archive.zip
touch random.tmp cache.tmp data.bak
```

### Tasks

1. Create organized folders:
   - `images/` for .jpg, .png files
   - `documents/` for .pdf, .docx, .txt files
   - `media/` for .mp3, .mp4, .mkv files
   - `archives/` for .tar.gz, .zip, .deb, .exe files

2. Move files to appropriate folders using wildcards

3. Delete all temporary files (.tmp, .bak)

4. Verify the final structure

5. **Document your commands** in a file called `organization-log.txt`

### Step-by-Step Solution

<details>
<summary>💡 Hints</summary>

- Use `mkdir` to create all folders at once
- Use wildcards with `mv` to move multiple files
- Remember: you can use multiple patterns like `*.jpg *.png`
- Use `echo "text" >> file` to document commands

</details>

<details>
<summary>✅ Full Solution</summary>

```bash
# Navigate to downloads folder
cd ~/linux-practice/module3/downloads

# Step 1: Create organized folders
mkdir images documents media archives

# Step 2: Move files to appropriate folders

# Move images
mv *.jpg *.png images/

# Move documents
mv *.pdf *.docx *.txt documents/

# Move media files
mv *.mp3 *.mp4 *.mkv media/

# Move archives and installers
mv *.tar.gz *.zip *.deb *.exe archives/

# Step 3: Delete temporary files
rm *.tmp *.bak

# Step 4: Verify structure
echo "=== Downloads Organization Result ==="
ls -la
echo ""
echo "=== Images ===" && ls images/
echo ""
echo "=== Documents ===" && ls documents/
echo ""
echo "=== Media ===" && ls media/
echo ""
echo "=== Archives ===" && ls archives/

# Step 5: Document commands
cat > organization-log.txt << 'EOF'
# Downloads Organization Log
# Date: $(date)

## Commands Used:

# Created folders:
mkdir images documents media archives

# Moved images:
mv *.jpg *.png images/

# Moved documents:
mv *.pdf *.docx *.txt documents/

# Moved media:
mv *.mp3 *.mp4 *.mkv media/

# Moved archives:
mv *.tar.gz *.zip *.deb *.exe archives/

# Deleted temp files:
rm *.tmp *.bak
EOF

echo ""
echo "=== Organization Log ==="
cat organization-log.txt
```

</details>

### Expected Final Structure

```
downloads/
├── archives/
│   ├── archive.zip
│   ├── installer.deb
│   ├── package.tar.gz
│   └── setup.exe
├── documents/
│   ├── document1.pdf
│   ├── document2.pdf
│   ├── notes.txt
│   └── report.docx
├── images/
│   ├── photo1.jpg
│   ├── photo2.jpg
│   ├── photo3.png
│   └── screenshot.png
├── media/
│   ├── movie.mkv
│   ├── song1.mp3
│   ├── song2.mp3
│   └── video.mp4
└── organization-log.txt
```

### ✅ Checklist
- [ ] Four category folders created
- [ ] All images in `images/`
- [ ] All documents in `documents/`
- [ ] All media in `media/`
- [ ] All archives in `archives/`
- [ ] Temporary files deleted
- [ ] Commands documented in `organization-log.txt`

---

## 🏆 Bonus Challenges

### Challenge A: Backup Script Simulation

Create a backup of your entire `linux-project` with today's date:

```bash
# Create backup with date
cp -r linux-project linux-project-backup-$(date +%Y%m%d)
```

### Challenge B: Find and Count

Count how many `.txt` files are in your practice directory:

```bash
find ~/linux-practice -name "*.txt" | wc -l
```

### Challenge C: Selective Copy

Copy only files larger than 0 bytes to a new folder:

```bash
mkdir non-empty-files
find . -type f -size +0 -exec cp {} non-empty-files/ \;
```

---

## 📝 Exercise Summary

After completing these exercises, you should be comfortable with:

| Skill | Commands Practiced |
|-------|-------------------|
| Creating files | `touch`, `>`, `>>` |
| Creating directories | `mkdir`, `mkdir -p` |
| Copying | `cp`, `cp -r`, `cp -i` |
| Moving/Renaming | `mv` |
| Deleting | `rm`, `rm -i`, `rm -r`, `rmdir` |
| Wildcards | `*`, `?`, `[...]` |
| Finding files | `find`, `which`, `whereis` |

---

## 🧹 Cleanup

When you're done practicing, you can clean up:

```bash
# Remove all practice files
cd ~
rm -ri linux-practice/module3

# Or keep them for future reference!
```

💡 **Tip:** Consider keeping your practice directories as reference material. You can always clean up later!

---

## Next Module Preview

In **Module 4: Viewing and Editing File Contents**, you'll learn to:
- View file contents with `cat`, `less`, `head`, `tail`
- Edit files with `nano` (beginner-friendly editor)
- Search within files using `grep`
- Understand file permissions