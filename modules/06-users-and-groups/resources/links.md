# Module 6 Resources: User and Group Management

## 📚 Official Documentation

### GNU/Linux Documentation
- **GNU Coreutils - Users**: https://www.gnu.org/software/coreutils/manual/html_node/User-information.html
  - Official documentation for user information commands

- **Linux man pages - useradd**: https://man7.org/linux/man-pages/man8/useradd.8.html
  - Detailed man page for useradd with all options

- **Linux man pages - usermod**: https://man7.org/linux/man-pages/man8/usermod.8.html
  - Detailed man page for modifying user accounts

- **Linux man pages - passwd**: https://man7.org/linux/man-pages/man1/passwd.1.html
  - Password management command documentation

- **Linux man pages - groupadd**: https://man7.org/linux/man-pages/man8/groupadd.8.html
  - Group creation command documentation

- **Shadow Password Suite**: https://man7.org/linux/man-pages/man5/shadow.5.html
  - Understanding the /etc/shadow file format

---

## 🔧 User Administration Guides

### Beginner-Friendly Tutorials
- **Linux User Management** (Linuxize): https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/
  - Comprehensive guide to creating users

- **Linux Group Management** (Linuxize): https://linuxize.com/post/how-to-create-groups-in-linux/
  - Clear guide to group management

- **Managing Users and Groups** (Red Hat): https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/managing-users-and-groups_configuring-basic-system-settings
  - Enterprise-focused guide from Red Hat

- **User and Group Management** (Ubuntu): https://ubuntu.com/server/docs/security-users
  - Ubuntu server documentation on user security

- **How to Add and Delete Users** (DigitalOcean): https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-ubuntu-20-04
  - Step-by-step tutorial with clear explanations

### Understanding /etc/passwd and /etc/shadow
- **Understanding /etc/passwd**: https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/
  - Deep dive into the passwd file format

- **Understanding /etc/shadow**: https://www.cyberciti.biz/faq/understanding-etcshadow-file/
  - Explanation of shadow file and password hashes

- **Understanding /etc/group**: https://www.cyberciti.biz/faq/understanding-etcgroup-file/
  - Group file format explained

---

## 🔒 Security Best Practices

### User Security Guidelines
- **Linux Security Best Practices** (CIS Benchmarks): https://www.cisecurity.org/benchmark/distribution_independent_linux
  - Industry-standard security benchmarks for Linux

- **User Account Security** (SANS): https://www.sans.org/reading-room/whitepapers/linux/
  - Security whitepapers including user account hardening

- **Secure User Management** (Linux Handbook): https://linuxhandbook.com/linux-user-management/
  - Security-focused user management guide

- **Password Security Best Practices**: https://www.redhat.com/sysadmin/password-security-linux
  - Red Hat guide to password security

### Sudo Security
- **Sudo Security Policy** (sudo.ws): https://www.sudo.ws/docs/man/sudoers.man/
  - Official sudoers manual

- **Sudo Best Practices** (DigitalOcean): https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file
  - Guide to safely editing sudoers

- **Understanding Sudo**: https://www.redhat.com/sysadmin/sudo-access-in-linux
  - Red Hat guide to sudo configuration

### Principle of Least Privilege
- **Least Privilege Principle**: https://csrc.nist.gov/glossary/term/least_privilege
  - NIST definition and explanation

- **Implementing Least Privilege**: https://www.beyondtrust.com/resources/glossary/least-privilege
  - Practical guide to implementing least privilege

---

## 🔐 PAM (Pluggable Authentication Modules)

### Introduction to PAM
- **Linux PAM Guide**: https://www.redhat.com/sysadmin/pluggable-authentication-modules-pam
  - Red Hat introduction to PAM

- **PAM Tutorial** (Debian): https://wiki.debian.org/PAM
  - Debian Wiki PAM documentation

- **Understanding PAM**: https://www.linux.com/training-tutorials/understanding-pam/
  - Linux.com PAM tutorial

### PAM Configuration
- **PAM Configuration Files**: https://man7.org/linux/man-pages/man5/pam.conf.5.html
  - Man page for PAM configuration

- **pam.d Directory Explained**: https://www.thegeekdiary.com/understanding-pam-pluggable-authentication-modules-in-linux/
  - Guide to the /etc/pam.d directory

### Password Policies with PAM
- **Password Complexity with PAM**: https://www.server-world.info/en/note?os=Ubuntu_22.04&p=pam
  - Setting up password policies

- **pam_pwquality**: https://linux.die.net/man/8/pam_pwquality
  - Password quality module documentation

---

## 🌐 LDAP and Centralized User Management (Introduction)

### What is LDAP?
- **LDAP Introduction** (DigitalOcean): https://www.digitalocean.com/community/tutorials/understanding-the-ldap-protocol-data-hierarchy-and-entry-components
  - Beginner-friendly LDAP introduction

- **OpenLDAP Quick Start**: https://www.openldap.org/doc/admin24/quickstart.html
  - Official OpenLDAP getting started guide

### Centralized Authentication
- **FreeIPA Introduction**: https://www.freeipa.org/page/About
  - Enterprise identity management solution

- **SSSD (System Security Services Daemon)**: https://sssd.io/
  - Client-side service for centralized authentication

- **Active Directory Integration**: https://ubuntu.com/server/docs/service-sssd
  - Connecting Linux to Active Directory

### When to Use Centralized Management
- Use LDAP/AD when you have:
  - More than 10-20 users to manage
  - Multiple servers requiring the same user accounts
  - Need for centralized password policies
  - Compliance requirements for user auditing

💡 **Note**: LDAP and centralized authentication are advanced topics. Focus on local user management first, then explore these when managing multiple systems.

---

## 🎥 Video Tutorials

### YouTube Resources
- **Linux User Management**: https://www.youtube.com/results?search_query=linux+user+management+tutorial
  - Search for comprehensive video tutorials

- **useradd vs adduser**: https://www.youtube.com/results?search_query=useradd+vs+adduser+linux
  - Understanding the difference between these commands

- **Linux sudo Explained**: https://www.youtube.com/results?search_query=linux+sudo+explained
  - Video explanations of sudo

---

## 📋 Quick Reference Cards

### Cheat Sheets
- **Linux User Commands Cheat Sheet**: https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/
  - Includes user management commands

- **Red Hat User Management**: https://access.redhat.com/articles/1189123
  - Red Hat quick reference

### Command Summary Tables

#### User Management Commands
| Command | Description |
|---------|-------------|
| `whoami` | Show current username |
| `id` | Show user and group IDs |
| `id username` | Show info for specific user |
| `groups` | Show group memberships |
| `who` | Show logged-in users |
| `w` | Show users and their activity |
| `last` | Show login history |
| `useradd -m user` | Create user with home |
| `userdel -r user` | Delete user and home |
| `passwd user` | Set/change password |

#### Group Management Commands
| Command | Description |
|---------|-------------|
| `groupadd group` | Create a group |
| `groupdel group` | Delete a group |
| `usermod -aG group user` | Add user to group |
| `gpasswd -a user group` | Add user to group |
| `gpasswd -d user group` | Remove user from group |
| `groups user` | Show user's groups |

#### User Modification Commands
| Command | Description |
|---------|-------------|
| `usermod -aG group user` | Add to group (APPEND!) |
| `usermod -l new old` | Rename user |
| `usermod -d /path user` | Change home directory |
| `usermod -s /shell user` | Change shell |
| `usermod -L user` | Lock account |
| `usermod -U user` | Unlock account |

#### Important Files
| File | Purpose |
|------|---------|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted passwords |
| `/etc/group` | Group information |
| `/etc/sudoers` | Sudo configuration |
| `/etc/skel/` | New user home template |
| `/etc/login.defs` | Login defaults |

---

## 🔗 Additional Resources

### Forums and Communities
- **Unix & Linux Stack Exchange**: https://unix.stackexchange.com/questions/tagged/users
  - Q&A for user management questions

- **Linux Questions Forum**: https://www.linuxquestions.org/questions/linux-security-4/
  - Active community for Linux security and user topics

- **Reddit r/linuxadmin**: https://www.reddit.com/r/linuxadmin/
  - Discussions on Linux system administration

### Books
- **The Linux Command Line** by William Shotts (Free PDF): https://linuxcommand.org/tlcl.php
  - Excellent chapter on users and permissions

- **How Linux Works** by Brian Ward
  - Deep dive into Linux internals including user management

- **UNIX and Linux System Administration Handbook**
  - Comprehensive guide to system administration

---

## 🔍 Troubleshooting Common Issues

### User Creation Problems
- **"Cannot lock /etc/passwd"**: Another process is modifying user files. Wait or check for stuck processes.
- **"User already exists"**: Choose a different username or delete the existing user first.
- **Home directory not created**: Use `-m` flag with useradd.

### Permission Issues
- **"Permission denied" with sudo**: User not in sudo group. Add with `usermod -aG sudo username`.
- **"User not in sudoers file"**: Edit `/etc/sudoers` with `visudo`.
- **Group membership not working**: Log out and back in, or use `newgrp groupname`.

### Password Problems
- **"Authentication failure"**: Wrong password or account locked. Check with `passwd -S username`.
- **Cannot change password**: May need sudo, or password policies may prevent weak passwords.

---

## 💡 Tips for Learning

### Practice Environments
- **Kali Linux VM**: Your current setup - safe for experimentation
- **Docker containers**: Create disposable environments for testing
- **VirtualBox snapshots**: Take snapshots before experimenting

### Recommended Practice Order
1. Learn to view user information (`whoami`, `id`, `groups`)
2. Understand the configuration files (`/etc/passwd`, `/etc/group`)
3. Practice with sudo safely
4. Create and delete test users
5. Set up groups and shared directories
6. Implement a complete team scenario

### Common Mistakes to Avoid
- ⚠️ Using `-G` instead of `-aG` (removes existing group memberships!)
- ⚠️ Forgetting to set passwords for new users
- ⚠️ Running as root when not necessary
- ⚠️ Deleting users without checking for their files first
- ⚠️ Editing `/etc/sudoers` directly instead of using `visudo`

---

## 📌 Bookmark These

Essential links to bookmark for quick access:

1. 📖 **Linux man pages**: https://man7.org/linux/man-pages/
2. 🔐 **Sudo Manual**: https://www.sudo.ws/docs/man/
3. 👥 **User Management Guide**: https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/
4. 🔒 **CIS Benchmarks**: https://www.cisecurity.org/cis-benchmarks/

---

## 🎯 Next Steps After This Module

Once you're comfortable with user and group management, consider learning:

1. **Process Management** - Managing running processes and services
2. **System Monitoring** - Monitoring system resources and user activity
3. **Log Management** - Understanding and analyzing system logs
4. **Backup and Recovery** - Protecting user data
5. **Advanced Authentication** - PAM, LDAP, SSO

---

*Last updated: Module 6 - User and Group Management*