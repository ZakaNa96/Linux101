# 📋 Capstone Project Requirements

## Technical Requirements

This document outlines everything you need to complete the capstone project successfully.

---

## 🖥️ System Requirements

### Virtual Machine Setup

| Requirement | Specification |
|-------------|---------------|
| **Operating System** | Kali Linux (latest version) |
| **RAM** | Minimum 2 GB (4 GB recommended) |
| **Disk Space** | Minimum 20 GB available |
| **Network** | NAT or Bridged adapter configured |
| **Virtualization** | VirtualBox, VMware, or similar |

### VM Configuration Checklist

Before starting, ensure your Kali VM has:

- [ ] Fresh installation or clean snapshot
- [ ] Network connectivity (can ping google.com)
- [ ] Root access or sudo privileges
- [ ] Terminal access working
- [ ] Sufficient disk space for project files

---

## 📚 Required Knowledge

You should be comfortable with the following concepts from each module:

### Module 1: Introduction to Linux
- Understanding of Linux distributions
- Basic terminal usage
- Understanding of the root user

### Module 2: Navigating the Filesystem
- Commands: `pwd`, `cd`, `ls`
- Understanding absolute vs. relative paths
- Familiarity with key directories (`/home`, `/var`, `/etc`)

### Module 3: Working with Files and Directories
- Commands: `mkdir`, `touch`, `cp`, `mv`, `rm`
- Creating directory structures
- Working with hidden files

### Module 4: Viewing and Editing Text Files
- Commands: `cat`, `less`, `head`, `tail`
- Using `nano` or `vim` for editing
- Creating configuration files

### Module 5: Permissions and Ownership
- Understanding `rwx` permissions
- Commands: `chmod`, `chown`
- Understanding user/group/other permissions
- Special permissions awareness

### Module 6: Users and Groups
- Commands: `useradd`, `usermod`, `groupadd`
- Understanding `/etc/passwd` and `/etc/group`
- Managing user home directories

### Module 7: Package Management
- Commands: `apt update`, `apt install`
- Searching for packages
- Listing installed packages

### Module 8: Processes and Services
- Commands: `systemctl`, `ps`, `top`
- Understanding services and daemons
- Cron job basics

### Module 9: Networking Basics
- Commands: `ip`, `ping`, `ss`
- Understanding IP addresses and ports
- Basic firewall concepts with `ufw`

### Module 10: Scripting and Sysadmin Tasks
- Bash scripting basics
- Variables and conditions
- Reading system information programmatically

---

## 🔧 Tools You'll Need

### Pre-installed on Kali Linux
These tools should already be available:

| Tool | Purpose | Verify Command |
|------|---------|----------------|
| `bash` | Shell scripting | `bash --version` |
| `nano` | Text editing | `nano --version` |
| `apt` | Package management | `apt --version` |
| `systemctl` | Service management | `systemctl --version` |
| `ip` | Network configuration | `ip --version` |
| `cron` | Task scheduling | `crontab -l` |

### Tools to Install During Project
You'll install these as part of the project:

| Tool | Purpose |
|------|---------|
| `git` | Version control |
| `vim` | Advanced text editor |
| `curl` | Data transfer |
| `wget` | File downloading |
| `htop` | Process viewer |
| `tree` | Directory visualization |
| `ufw` | Firewall management |

---

## 📦 Expected Deliverables

By the end of this project, you should have created:

### Files and Directories

```
/home/projects/
├── README.md
├── webapp/
│   └── README.md
└── scripts/
    ├── README.md
    └── health-check.sh

/var/log/company/
└── README.md

/home/developer1/
/home/developer2/
/home/teamlead/

/etc/motd (modified)
```

### Users and Groups

| User | Groups | Home Directory |
|------|--------|----------------|
| developer1 | developers | /home/developer1 |
| developer2 | developers | /home/developer2 |
| teamlead | developers, leads | /home/teamlead |

### Scripts

1. **System Health Check Script** (`health-check.sh`)
   - Checks disk usage
   - Checks memory usage
   - Lists running services
   - Tests network connectivity
   - Generates report file

2. **Bonus Scripts** (optional)
   - User onboarding script
   - Backup script

### Documentation Files

| File | Contents |
|------|----------|
| System Documentation | Hardware info, OS version, installed packages |
| Network Documentation | IP addresses, network config, firewall rules |
| Directory READMEs | Purpose of each created directory |

### Configuration Changes

- [ ] MOTD (Message of the Day) customized
- [ ] UFW firewall configured
- [ ] Cron jobs scheduled
- [ ] SSH service verified

---

## ✅ Success Criteria

Your project will be evaluated on:

### Functionality (40%)
- All required users and groups exist
- Permissions work as specified
- Scripts execute without errors
- Cron jobs are scheduled correctly
- Firewall rules are applied

### Security (20%)
- No overly permissive permissions (avoid 777)
- Proper group-based access control
- Firewall configured appropriately
- Secure defaults maintained

### Documentation (20%)
- Clear README files
- Commands documented
- Configuration explained
- Network topology documented

### Code Quality (10%)
- Scripts are readable
- Proper comments
- Error handling in scripts
- Consistent naming conventions

### Completeness (10%)
- All phases attempted
- Submission checklist completed
- No missing components

---

## ⚠️ Important Notes

### Working as Root

Many tasks require root privileges. You can either:

1. **Login as root** (Kali default)
2. **Use sudo** for individual commands
3. **Use sudo -i** for a root shell

> ⚡ **Best Practice:** Use `sudo` for individual commands rather than staying logged in as root. This creates an audit trail and prevents accidental damage.

### Making Mistakes

Don't panic if something goes wrong:

1. **Read error messages** - they usually tell you what's wrong
2. **Check your syntax** - typos are the most common issue
3. **Verify paths** - make sure directories exist
4. **Check permissions** - you might need sudo

### Taking Snapshots

Before starting, take a VM snapshot:

```bash
# In VirtualBox: Machine → Take Snapshot
# In VMware: VM → Snapshot → Take Snapshot
```

This allows you to restore if something goes seriously wrong.

### Time Management

| If you're stuck for... | Then... |
|------------------------|---------|
| 5-10 minutes | Re-read the instructions |
| 10-20 minutes | Check the module materials |
| 20-30 minutes | Consult the solution guide |
| Longer | Take a break and come back |

---

## 🚦 Ready to Start?

Complete this pre-flight checklist:

- [ ] VM is running with network access
- [ ] You can open a terminal
- [ ] You have root/sudo access
- [ ] You've taken a snapshot
- [ ] You have the course materials accessible
- [ ] You've read the tasks.md file

**Once all boxes are checked, proceed to `tasks.md` to begin your project!**

---

*Remember: This project simulates real work. Take it seriously, but don't stress - learning from mistakes is part of the process!*