# Module 3: Resources - Working with Files and Directories

## 📚 Official Documentation

### GNU Coreutils Manual
The official documentation for core Linux file utilities (`cp`, `mv`, `rm`, `mkdir`, `touch`, etc.)

- **GNU Coreutils Manual** - https://www.gnu.org/software/coreutils/manual/coreutils.html
- **cp (copy)** - https://www.gnu.org/software/coreutils/manual/html_node/cp-invocation.html
- **mv (move)** - https://www.gnu.org/software/coreutils/manual/html_node/mv-invocation.html
- **rm (remove)** - https://www.gnu.org/software/coreutils/manual/html_node/rm-invocation.html
- **mkdir** - https://www.gnu.org/software/coreutils/manual/html_node/mkdir-invocation.html
- **touch** - https://www.gnu.org/software/coreutils/manual/html_node/touch-invocation.html
- **rmdir** - https://www.gnu.org/software/coreutils/manual/html_node/rmdir-invocation.html

### Find Command Documentation
- **GNU Find Manual** - https://www.gnu.org/software/findutils/manual/html_mono/find.html
- **Find Man Page** - https://man7.org/linux/man-pages/man1/find.1.html

---

## 🎯 Tutorials and Guides

### File Operations
- **Linux File Management** (LinuxCommand.org) - https://linuxcommand.org/lc3_lts0050.php
- **Manipulating Files** (LinuxCommand.org) - https://linuxcommand.org/lc3_lts0060.php
- **Linux cp Command** (Linuxize) - https://linuxize.com/post/cp-command-in-linux/
- **Linux mv Command** (Linuxize) - https://linuxize.com/post/how-to-move-files-in-linux-with-mv-command/
- **Linux rm Command** (Linuxize) - https://linuxize.com/post/rm-command-in-linux/
- **Linux mkdir Command** (Linuxize) - https://linuxize.com/post/how-to-create-directories-in-linux-with-the-mkdir-command/

### Wildcards and Globbing
- **Wildcards** (Linux Documentation Project) - https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm
- **Bash Wildcards** (LinuxCommand.org) - https://linuxcommand.org/lc3_lts0080.php
- **Glob Patterns** (Bash Manual) - https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html
- **Wildcard Tutorial** (Ryan's Tutorials) - https://ryanstutorials.net/linuxtutorial/wildcards.php

### Find Command Tutorials
- **35 Practical Examples of Find** (TecMint) - https://www.tecmint.com/35-practical-examples-of-linux-find-command/
- **How to Use Find Command** (Linuxize) - https://linuxize.com/post/how-to-find-files-in-linux-using-the-command-line/
- **Find Command Tutorial** (DigitalOcean) - https://www.digitalocean.com/community/tutorials/how-to-use-find-and-locate-to-search-for-files-on-linux
- **Locate Command** (Linuxize) - https://linuxize.com/post/locate-command-in-linux/

---

## ⚠️ Safety Resources

### Safe File Deletion
- **Safe rm Practices** - https://www.tecmint.com/recover-deleted-files-in-linux/
- **Trash-CLI** (Safer alternative to rm) - https://github.com/andreafrancia/trash-cli
- **Why rm -rf is Dangerous** - https://www.howtogeek.com/449860/how-to-use-the-rm-command-on-linux/

### Backup Best Practices
- **Linux Backup Strategies** - https://linuxize.com/post/how-to-use-rsync-for-local-and-remote-data-transfer-and-synchronization/
- **Rsync Tutorial** - https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories

---

## 📖 Reference Materials

### Cheat Sheets
- **Linux Commands Cheat Sheet** (Red Hat) - https://developers.redhat.com/cheat-sheets/linux-commands-cheat-sheet
- **File Management Cheat Sheet** (CheatSheet.dennyzhang.com) - https://cheatsheet.dennyzhang.com/cheatsheet-file
- **Find Command Cheat Sheet** - https://www.linuxtrainingacademy.com/linux-find-command-cheat-sheet/

### Man Pages Online
- **Linux Man Pages** - https://man7.org/linux/man-pages/
- **Explainshell** (Interactive command explanation) - https://explainshell.com/
- **TLDR Pages** (Simplified man pages) - https://tldr.sh/

---

## 🎥 Video Tutorials

### YouTube Resources
- **Linux File System Basics** (NetworkChuck) - Search: "NetworkChuck Linux file system"
- **Linux Command Line for Beginners** (freeCodeCamp) - Search: "freeCodeCamp Linux command line"
- **The Linux File System** (LearnLinuxTV) - Search: "LearnLinuxTV file management"

---

## 🔧 Interactive Learning

### Practice Platforms
- **OverTheWire Bandit** - https://overthewire.org/wargames/bandit/
  - Levels 0-5 cover basic file operations
- **Linux Journey** - https://linuxjourney.com/
  - "Command Line" and "Text-Fu" sections
- **Terminus Game** - https://web.mit.edu/mprat/Public/web/Terminus/Web/main.html
  - Interactive command-line adventure

### Online Terminals
- **JSLinux** - https://bellard.org/jslinux/
- **Copy.sh Virtual Machine** - https://copy.sh/v86/
- **Webminal** - https://www.webminal.org/

---

## 📚 Books and Extended Reading

### Free Online Books
- **The Linux Command Line** by William Shotts - https://linuxcommand.org/tlcl.php
  - Chapter 4: Manipulating Files and Directories
  - Chapter 17: Searching for Files
- **Linux Fundamentals** by Paul Cobbaut - https://linux-training.be/linuxfun.pdf

### Recommended Chapters
- TLCL Chapter 4: "Manipulating Files and Directories" - Covers cp, mv, mkdir, rm
- TLCL Chapter 17: "Searching for Files" - Covers find, locate, whereis

---

## 🛡️ Kali Linux Specific

### Kali Documentation
- **Kali Linux Documentation** - https://www.kali.org/docs/
- **Kali Tools** - https://www.kali.org/tools/

### File Operations in Security Context
- **File Analysis Tools** - https://www.kali.org/tools/#forensics
- **Secure File Deletion** - https://www.kali.org/tools/secure-delete/

---

## 💡 Quick Command Reference

### Creating
```bash
touch file.txt          # Create empty file
mkdir dirname           # Create directory
mkdir -p path/to/dir    # Create nested directories
```

### Copying
```bash
cp src dest            # Copy file
cp -r src dest         # Copy directory
cp -i src dest         # Interactive (confirm overwrite)
```

### Moving/Renaming
```bash
mv src dest            # Move or rename
mv -i src dest         # Interactive
```

### Deleting
```bash
rm file               # Delete file
rm -r dir             # Delete directory
rm -i file            # Interactive
rmdir emptydir        # Delete empty directory only
```

### Finding
```bash
find /path -name "pattern"    # Find by name
find /path -type f            # Find files
find /path -type d            # Find directories
locate filename               # Fast database search
which command                 # Find command location
```

### Wildcards
```bash
*       # Any characters
?       # Single character
[abc]   # Any character in set
[a-z]   # Character range
```

---

## 🔗 Community and Support

### Forums and Q&A
- **Unix & Linux Stack Exchange** - https://unix.stackexchange.com/
- **Ask Ubuntu** - https://askubuntu.com/
- **Reddit r/linuxquestions** - https://www.reddit.com/r/linuxquestions/
- **Reddit r/linux4noobs** - https://www.reddit.com/r/linux4noobs/

---

*Last updated: Module 3 - Working with Files and Directories*