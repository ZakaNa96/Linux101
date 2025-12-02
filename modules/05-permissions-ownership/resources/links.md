# Module 5 Resources: File Permissions and Ownership

## 📚 Official Documentation

### GNU/Linux Documentation
- **GNU Coreutils - chmod**: https://www.gnu.org/software/coreutils/manual/html_node/chmod-invocation.html
  - Official documentation for the chmod command
  
- **GNU Coreutils - chown**: https://www.gnu.org/software/coreutils/manual/html_node/chown-invocation.html
  - Official documentation for the chown command

- **Linux man pages - chmod**: https://man7.org/linux/man-pages/man1/chmod.1.html
  - Detailed man page for chmod with all options

- **Linux man pages - chown**: https://man7.org/linux/man-pages/man1/chown.1.html
  - Detailed man page for chown with all options

### File Permissions Deep Dive
- **Linux Filesystem Hierarchy**: https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/
  - Understanding where files live and their typical permissions

- **Understanding Linux File Permissions**: https://linuxhandbook.com/linux-file-permissions/
  - Comprehensive guide to Linux permissions

---

## 🔧 Interactive Tools

### chmod Calculators
- **Chmod Calculator**: https://chmod-calculator.com/
  - Interactive tool to calculate and convert permission values
  - Visual representation of symbolic and numeric modes

- **Chmod Command Calculator**: https://chmodcommand.com/
  - Another helpful chmod calculator with examples

- **SS64 chmod Reference**: https://ss64.com/bash/chmod.html
  - Quick reference with common examples

### Permission Visualization
- **Permission Visualizer**: https://mason.gmu.edu/~montMDi/info/permissions_visual.html
  - Visual explanation of Linux permissions

---

## 📖 Tutorials and Guides

### Beginner-Friendly Tutorials
- **Linux File Permissions Explained** (Linuxize): https://linuxize.com/post/understanding-linux-file-permissions/
  - Clear explanation with practical examples

- **Linux Permissions 101** (Red Hat): https://www.redhat.com/sysadmin/linux-file-permissions-explained
  - Enterprise-focused explanation from Red Hat

- **File Permissions in Linux** (DigitalOcean): https://www.digitalocean.com/community/tutorials/linux-permissions-basics-and-how-to-use-umask-on-a-vps
  - Covers basics and umask in detail

### Advanced Topics
- **Special Permissions Explained** (LinuxConfig): https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits
  - SUID, SGID, and Sticky bit in depth

- **Understanding umask**: https://www.cyberciti.biz/tips/understanding-linux-unix-umask-value-usage.html
  - Complete guide to umask

- **ACLs (Access Control Lists)**: https://www.redhat.com/sysadmin/linux-access-control-lists
  - Advanced permissions beyond traditional Unix permissions

---

## 🎥 Video Tutorials

### YouTube Resources
- **Linux File Permissions in 5 Minutes**: https://www.youtube.com/results?search_query=linux+file+permissions+explained
  - Search for quick visual explanations

- **chmod and chown Tutorial**: https://www.youtube.com/results?search_query=chmod+chown+linux+tutorial
  - Video walkthroughs of permission commands

---

## 🔒 Security Best Practices

### Security Guidelines
- **CIS Benchmarks**: https://www.cisecurity.org/cis-benchmarks/
  - Industry-standard security benchmarks including file permissions

- **Linux Security Best Practices** (SANS): https://www.sans.org/reading-room/whitepapers/linux/
  - Security whitepapers including permission hardening

- **OWASP File Permissions**: https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission
  - Web application file permission testing

### Common Permission Mistakes
- **Security Misconfigurations**: https://blog.securelayer7.net/linux-file-permission-security-issues/
  - Common permission errors and how to avoid them

---

## 📋 Quick Reference Cards

### Cheat Sheets
- **Linux Permissions Cheat Sheet** (DevHints): https://devhints.io/chmod
  - Quick reference for chmod commands

- **Linux Command Cheat Sheet**: https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/
  - Includes permission-related commands

### Permission Tables

#### Numeric Permission Reference
| Number | Permission | Symbolic |
|--------|------------|----------|
| 0 | No permission | --- |
| 1 | Execute | --x |
| 2 | Write | -w- |
| 3 | Write + Execute | -wx |
| 4 | Read | r-- |
| 5 | Read + Execute | r-x |
| 6 | Read + Write | rw- |
| 7 | Read + Write + Execute | rwx |

#### Common Permission Patterns
| Numeric | Symbolic | Use Case |
|---------|----------|----------|
| 755 | rwxr-xr-x | Executable files, directories |
| 644 | rw-r--r-- | Regular files |
| 700 | rwx------ | Private directories |
| 600 | rw------- | Private files (SSH keys, configs) |
| 750 | rwxr-x--- | Group-shared directories |
| 640 | rw-r----- | Group-shared files |
| 777 | rwxrwxrwx | ⚠️ AVOID - Security risk |

---

## 🛠️ Related Commands Reference

### Permission Commands
| Command | Description |
|---------|-------------|
| `chmod` | Change file permissions |
| `chown` | Change file owner and group |
| `chgrp` | Change file group |
| `umask` | Set default permissions |
| `ls -l` | View permissions |
| `stat` | Detailed file information |
| `getfacl` | Get file ACLs |
| `setfacl` | Set file ACLs |

### User/Group Commands
| Command | Description |
|---------|-------------|
| `whoami` | Current username |
| `groups` | Groups you belong to |
| `id` | User and group IDs |
| `users` | Currently logged in users |

---

## 💡 Tips for Learning

### Practice Environments
- **Kali Linux VM**: Your current setup - safe for experimentation
- **Docker containers**: Create disposable environments for testing
- **WSL (Windows Subsystem for Linux)**: Practice on Windows

### Recommended Practice
1. Always use a test directory for experiments
2. Never change permissions on system files without understanding
3. Avoid using `chmod 777` - it's almost never necessary
4. Document permission changes in production environments
5. Use `ls -la` frequently to verify changes

---

## 🔗 Additional Resources

### Forums and Communities
- **Unix & Linux Stack Exchange**: https://unix.stackexchange.com/questions/tagged/permissions
  - Q&A for permission-related questions

- **Linux Questions Forum**: https://www.linuxquestions.org/
  - Active community for Linux help

### Books
- **The Linux Command Line** by William Shotts (Free PDF): https://linuxcommand.org/tlcl.php
  - Chapter on permissions is excellent

- **How Linux Works** by Brian Ward
  - Deep dive into Linux internals including permissions

---

## 📌 Bookmark These

Essential links to bookmark for quick access:

1. 🧮 **Chmod Calculator**: https://chmod-calculator.com/
2. 📖 **Linux man pages**: https://man7.org/linux/man-pages/
3. 🔒 **Red Hat Permissions Guide**: https://www.redhat.com/sysadmin/linux-file-permissions-explained
4. 📋 **Chmod Cheat Sheet**: https://devhints.io/chmod

---

*Last updated: Module 5 - File Permissions and Ownership*