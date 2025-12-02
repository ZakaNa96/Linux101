# Module 2 Resources: Navigating the File System

A curated list of resources to deepen your understanding of Linux file system navigation.

---

## 📚 Official Documentation

### Filesystem Hierarchy Standard (FHS)
- **[FHS Specification](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)** - The official Linux Foundation document describing the directory structure and contents
- **[Wikipedia: Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)** - A good overview with explanations

### Man Pages (Built into Linux)
```bash
# Read the manual for any command:
man ls
man cd
man pwd
man hier    # Describes the filesystem hierarchy
```

---

## 🎮 Interactive Learning

### Online Linux Terminals
Practice without installing anything:

- **[JSLinux](https://bellard.org/jslinux/)** - Linux running in your browser
- **[Copy.sh Virtual Machine](https://copy.sh/v86/)** - Browser-based x86 virtual machine
- **[Webminal](https://www.webminal.org/)** - Free online Linux terminal for practice

### Interactive Tutorials
- **[Linux Journey - Command Line](https://linuxjourney.com/lesson/the-shell)** - Free interactive lessons
- **[OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)** - Learn Linux through a fun security game (levels 0-5 cover navigation)
- **[Terminus (MIT)](https://web.mit.edu/mprat/Public/web/Terminus/Web/main.html)** - A text adventure game that teaches command line basics

---

## 📖 Tutorials and Guides

### File System Structure
- **[Linux File System Hierarchy Explained](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)** - Linux Foundation blog post
- **[Understanding Linux File Hierarchy Structure](https://www.tecmint.com/linux-directory-structure-and-important-files-paths-explained/)** - TecMint detailed guide
- **[The Linux Directory Structure Explained](https://www.howtogeek.com/117435/htg-explains-the-linux-directory-structure-explained/)** - How-To Geek beginner guide

### Navigation Commands
- **[GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/coreutils.html)** - Official documentation for `ls`, `pwd`, `cd`, etc.
- **[SS64 Linux Commands](https://ss64.com/bash/)** - Quick reference for all bash commands
- **[TLDR Pages](https://tldr.sh/)** - Simplified, practical examples for commands

---

## 📹 Video Resources

### YouTube Channels
- **[NetworkChuck](https://www.youtube.com/c/NetworkChuck)** - Engaging Linux tutorials
- **[LearnLinuxTV](https://www.youtube.com/c/LearnLinuxtv)** - Comprehensive Linux learning
- **[tutoriaLinux](https://www.youtube.com/c/tutoriaLinux)** - System administration focused

### Specific Videos
- **Linux File System Explained** - Search for "Linux filesystem hierarchy explained" on YouTube
- **Terminal Navigation Basics** - Search for "Linux terminal navigation for beginners"

---

## 🛠️ Tools and Utilities

### Visual File System Tools
- **[ncdu](https://dev.yorhel.nl/ncdu)** - NCurses Disk Usage - visual directory analyzer
  ```bash
  sudo apt install ncdu
  ncdu /
  ```
- **[tree](https://linux.die.net/man/1/tree)** - Display directories as trees
  ```bash
  sudo apt install tree
  tree -L 2 /
  ```
- **[ranger](https://ranger.github.io/)** - Console file manager with vim-like bindings
  ```bash
  sudo apt install ranger
  ranger
  ```

### Enhanced ls Alternatives
- **[exa](https://the.exa.website/)** - Modern replacement for ls with colors and Git support
  ```bash
  sudo apt install exa
  exa -la
  ```
- **[lsd](https://github.com/Peltoche/lsd)** - LSDeluxe - ls with icons and colors
- **[bat](https://github.com/sharkdp/bat)** - A cat clone with syntax highlighting

---

## 📝 Cheat Sheets

### Printable References
- **[Linux Commands Cheat Sheet (PDF)](https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/)** - Comprehensive command reference
- **[Bash Cheat Sheet](https://devhints.io/bash)** - Quick bash reference
- **[UNIX/Linux Command Reference (FOSSwire)](https://files.fosswire.com/2007/08/fwunixref.pdf)** - Classic one-page reference

### Online References
- **[ExplainShell](https://explainshell.com/)** - Paste a command to see what each part does
- **[ShellCheck](https://www.shellcheck.net/)** - Check your shell scripts for errors
- **[Cheat.sh](https://cheat.sh/)** - Quick command examples in terminal
  ```bash
  curl cheat.sh/ls
  curl cheat.sh/cd
  ```

---

## 📚 Books (Free and Paid)

### Free Online Books
- **[The Linux Command Line](https://linuxcommand.org/tlcl.php)** by William Shotts - Complete free book (PDF)
- **[Linux Fundamentals](https://linux-training.be/linuxfun.pdf)** by Paul Cobbaut - Free PDF textbook
- **[Introduction to Linux](https://tldp.org/LDP/intro-linux/intro-linux.pdf)** - The Linux Documentation Project

### Recommended Books (Paid)
- **"How Linux Works" by Brian Ward** - Deep understanding of Linux systems
- **"The Linux Command Line" by William Shotts** - Physical book version
- **"UNIX and Linux System Administration Handbook"** - Industry standard reference

---

## 🎯 Practice Environments

### Virtual Machines
- **[Kali Linux](https://www.kali.org/get-kali/)** - Your current learning environment
- **[Ubuntu](https://ubuntu.com/download)** - Popular beginner-friendly distribution
- **[Linux Mint](https://linuxmint.com/)** - User-friendly Linux distribution

### Online Sandboxes
- **[Katacoda](https://www.katacoda.com/)** - Interactive Linux scenarios (now part of O'Reilly)
- **[Play with Docker](https://labs.play-with-docker.com/)** - Linux containers in browser

---

## 🔧 Kali Linux Specific

### Kali Documentation
- **[Kali Linux Documentation](https://www.kali.org/docs/)** - Official documentation
- **[Kali Linux Tools](https://www.kali.org/tools/)** - List of all included tools
- **[Kali Training](https://www.kali.org/training/)** - Official Kali training resources

---

## 💡 Tips for Using These Resources

1. **Start with the basics**: Linux Journey and TLDR pages are great for beginners
2. **Practice in your VM**: Try every command you learn in your Kali Linux VM
3. **Use man pages**: Get comfortable with `man command` for built-in help
4. **Bookmark ExplainShell**: It's invaluable for understanding complex commands
5. **Join communities**: r/linux4noobs on Reddit, Linux forums, and Discord servers

---

## 🇩🇪 German Language Resources

- **[SelfLinux](https://www.selflinux.org/)** - German Linux documentation
- **[Ubuntu Users (German)](https://wiki.ubuntuusers.de/)** - Comprehensive German Linux wiki
- **[Pro-Linux](https://www.pro-linux.de/)** - German Linux news and tutorials
- **[LinuxCommunity](https://www.linux-community.de/)** - German Linux magazine and community

---

## 🔗 Quick Reference Links

| Topic | Best Resource |
|-------|---------------|
| Learn file hierarchy | [Linux Journey](https://linuxjourney.com/) |
| Practice navigation | [OverTheWire Bandit](https://overthewire.org/wargames/bandit/) |
| Command help | [TLDR Pages](https://tldr.sh/) |
| Understand commands | [ExplainShell](https://explainshell.com/) |
| Visual learning | YouTube - NetworkChuck |
| Deep reading | [The Linux Command Line Book](https://linuxcommand.org/tlcl.php) |

---

*Last updated: December 2024*