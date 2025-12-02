# Module 6: User and Group Management

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Understand the Linux user system and user types
- Read and interpret user configuration files (`/etc/passwd`, `/etc/shadow`, `/etc/group`)
- Use sudo and understand the root user
- Create, modify, and delete users
- Create and manage groups
- Manage passwords and account security
- Switch between users effectively
- Apply user management to real-world scenarios

---

## Prerequisites

Before starting this module, you should be comfortable with:
- ✅ Module 1: Basic terminal navigation
- ✅ Module 2: Navigating the filesystem
- ✅ Module 3: Working with files and directories
- ✅ Module 4: Viewing and editing text files
- ✅ Module 5: File permissions and ownership

---

## 1. Understanding Users in Linux

### What is a User?

In Linux, a **user** is an entity that can access the system and its resources. Every process runs as a user, and every file is owned by a user. Users provide:

```
┌─────────────────────────────────────────────────────────────────┐
│                    WHY USERS MATTER                              │
├─────────────────────────────────────────────────────────────────┤
│  🔐 Security      - Control who can access the system            │
│  📁 Ownership     - Files belong to specific users               │
│  📊 Accounting    - Track who does what                          │
│  🚧 Isolation     - Separate user environments                   │
│  🛡️  Privilege    - Limit what actions users can perform         │
└─────────────────────────────────────────────────────────────────┘
```

### User IDs (UID)

Every user has a unique numeric identifier called a **UID** (User ID):

```
┌─────────────────────────────────────────────────────────────────┐
│                    USER ID RANGES                                │
├─────────────────────────────────────────────────────────────────┤
│  UID 0          │  root (superuser)                              │
│  UID 1-999      │  System users (services, daemons)              │
│  UID 1000+      │  Regular users (human users)                   │
└─────────────────────────────────────────────────────────────────┘
```

- **UID 0**: The root user - has unlimited access to the system
- **System users (1-999)**: Used by services like web servers, databases
- **Regular users (1000+)**: Human users who log in and work

💡 **Tip**: The first regular user created during installation typically gets UID 1000.

### The `/etc/passwd` File

The `/etc/passwd` file contains information about all users on the system. Despite its name, it no longer stores passwords (those are in `/etc/shadow`).

```bash
# View the passwd file
cat /etc/passwd

# View your own entry
grep $USER /etc/passwd
```

**Format of each line:**
```
username:x:UID:GID:comment:home_directory:shell
```

**Example breakdown:**
```
kali:x:1000:1000:Kali User,,,:/home/kali:/bin/bash
│    │ │    │    │          │           │
│    │ │    │    │          │           └── Login shell
│    │ │    │    │          └── Home directory
│    │ │    │    └── Comment/Full name (GECOS field)
│    │ │    └── Primary Group ID (GID)
│    │ └── User ID (UID)
│    └── Password placeholder (x = password in /etc/shadow)
└── Username
```

**Common entries you'll see:**
```
root:x:0:0:root:/root:/bin/bash              # The superuser
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin  # System daemon
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin  # Web server
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
kali:x:1000:1000:,,,:/home/kali:/bin/zsh     # Regular user
```

### The `/etc/shadow` File

The `/etc/shadow` file stores encrypted password information. It's only readable by root for security reasons.

```bash
# Try to read it (will fail as regular user)
cat /etc/shadow

# Read with sudo
sudo cat /etc/shadow
```

**Format:**
```
username:encrypted_password:last_change:min:max:warn:inactive:expire:reserved
```

**Example:**
```
kali:$y$j9T$...:19691:0:99999:7:::
│    │           │     │ │     │
│    │           │     │ │     └── Password expiry warning days
│    │           │     │ └── Maximum password age
│    │           │     └── Minimum password age
│    │           └── Last password change (days since Jan 1, 1970)
│    └── Encrypted password hash
└── Username
```

**Password field special values:**
- `*` or `!` = Account is locked
- `!!` = Password has never been set
- Empty = No password (⚠️ dangerous!)

⚠️ **Warning**: Never edit `/etc/shadow` directly. Use password management commands instead.

### Home Directories

Each user typically has a home directory where their personal files are stored:

```
/home/
├── alice/          # Alice's home directory
├── bob/            # Bob's home directory
└── kali/           # Kali user's home directory

/root/              # Root user's home directory (separate location)
```

```bash
# Print your home directory
echo $HOME

# Go to your home directory
cd ~
# or
cd $HOME
# or just
cd

# View home directory permissions
ls -la /home/
```

💡 **Tip**: The `~` (tilde) is a shortcut for the current user's home directory. `~username` refers to another user's home directory.

---

## 2. Understanding Groups

### What is a Group?

A **group** is a collection of users. Groups simplify permission management by allowing you to grant access to multiple users at once.

```
┌─────────────────────────────────────────────────────────────────┐
│                    WHY GROUPS MATTER                             │
├─────────────────────────────────────────────────────────────────┤
│  👥 Collaboration  - Multiple users share file access            │
│  🔐 Security       - Control access at group level               │
│  📊 Organization   - Logical grouping of users                   │
│  ⚡ Efficiency     - Manage permissions for many users at once   │
└─────────────────────────────────────────────────────────────────┘
```

### Group IDs (GID)

Like users, groups have unique numeric identifiers called **GID** (Group ID):

- **GID 0**: The root group
- **GID 1-999**: System groups
- **GID 1000+**: User groups

### Primary vs Secondary Groups

Every user has:
- **One primary group** (specified in `/etc/passwd`)
- **Zero or more secondary groups** (specified in `/etc/group`)

```
┌─────────────────────────────────────────────────────────────────┐
│                    PRIMARY vs SECONDARY GROUPS                   │
├─────────────────────────────────────────────────────────────────┤
│  PRIMARY GROUP                                                   │
│  • New files are owned by this group by default                  │
│  • Listed in /etc/passwd (4th field)                             │
│  • Usually same name as username                                 │
│                                                                  │
│  SECONDARY GROUPS                                                │
│  • Additional group memberships                                  │
│  • Listed in /etc/group                                          │
│  • User gets access to files owned by these groups              │
└─────────────────────────────────────────────────────────────────┘
```

### The `/etc/group` File

The `/etc/group` file contains information about all groups:

```bash
# View the group file
cat /etc/group

# View groups containing a specific user
grep $USER /etc/group
```

**Format:**
```
groupname:x:GID:member_list
```

**Example breakdown:**
```
sudo:x:27:kali
│    │ │  │
│    │ │  └── Members (comma-separated)
│    │ └── Group ID (GID)
│    └── Password placeholder (rarely used)
└── Group name
```

**Common groups:**
```
root:x:0:                    # Root group
sudo:x:27:kali               # Users who can use sudo
www-data:x:33:               # Web server group
users:x:100:                 # General users group
kali:x:1000:                 # Kali's primary group
```

### Why Groups Matter for Shared Access

Consider a development team scenario:

```
Without Groups:
Alice needs access → Set permissions for Alice
Bob needs access → Set permissions for Bob
Carol needs access → Set permissions for Carol
(Repeat for every file and every user!)

With Groups:
Create 'developers' group
Add Alice, Bob, Carol to group
Set permissions once for 'developers' group
✓ Everyone has access
✓ Easy to add/remove members
```

---

## 3. Viewing User Information

### `whoami` - Current User

```bash
# Display your username
whoami
```

Output:
```
kali
```

### `id` - User and Group IDs

```bash
# Show your user and group information
id

# Show information for a specific user
id root
id www-data
```

Output:
```
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo),100(users)
│              │               │
│              │               └── All groups (primary and secondary)
│              └── Primary group
└── User ID and name
```

### `groups` - Show Group Memberships

```bash
# Show your groups
groups

# Show groups for a specific user
groups root
groups www-data
```

Output:
```
kali sudo users
```

### `who` - Who is Logged In

```bash
# Show logged-in users
who
```

Output:
```
kali     tty7         2024-12-01 10:30 (:0)
kali     pts/0        2024-12-01 10:35 (:0)
```

### `w` - Who is Logged In and What They're Doing

```bash
# Show logged-in users with activity
w
```

Output:
```
 10:45:32 up 2 days,  3:22,  2 users,  load average: 0.15, 0.10, 0.09
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
kali     tty7     :0               Mon10    2days  1:23   0.05s /usr/bin/startplasma
kali     pts/0    :0               10:35    0.00s  0.05s  0.00s w
```

### `last` - Login History

```bash
# Show recent logins
last

# Show last 5 logins
last -5

# Show logins for a specific user
last kali

# Show last system reboots
last reboot
```

Output:
```
kali     pts/0        :0               Mon Dec  1 10:35   still logged in
kali     tty7         :0               Mon Dec  1 10:30   still logged in
reboot   system boot  5.15.0-kali3-amd Mon Dec  1 10:30   still running
kali     pts/0        :0               Sun Nov 30 09:15 - 18:30  (09:15)
```

💡 **Tip**: Use `lastlog` to see the last login time for all users.

---

## 4. The root User and sudo

### What is root?

The **root** user (UID 0) is the superuser with unlimited access to the system:

```
┌─────────────────────────────────────────────────────────────────┐
│                    ROOT USER POWERS                              │
├─────────────────────────────────────────────────────────────────┤
│  ✓ Read, write, execute any file                                 │
│  ✓ Change any file permissions or ownership                      │
│  ✓ Install and remove software                                   │
│  ✓ Create and delete any user                                    │
│  ✓ Start and stop any service                                    │
│  ✓ Access all hardware devices                                   │
│  ✓ Override any security restriction                             │
└─────────────────────────────────────────────────────────────────┘
```

### Why You Shouldn't Always Be Root

⚠️ **Warning**: Running as root is dangerous!

```
┌─────────────────────────────────────────────────────────────────┐
│               DANGERS OF RUNNING AS ROOT                         │
├─────────────────────────────────────────────────────────────────┤
│  💥 Mistakes are permanent - `rm -rf /` destroys everything      │
│  🦠 Malware gets full access - one download ruins the system     │
│  🔓 No protection - no confirmation for dangerous actions        │
│  📝 No audit trail - harder to track what changed                │
│  🎯 Larger attack surface - compromises affect everything        │
└─────────────────────────────────────────────────────────────────┘
```

**The Principle of Least Privilege:**
> Users should have only the minimum access needed to perform their tasks.

⚠️ **Kali Linux Note**: Kali Linux historically ran as root by default for penetration testing. Modern versions create a regular user, but many tools still require root. Even on Kali, avoid running as root when not necessary.

### Using `sudo` - Run as Superuser

The `sudo` command lets you run a single command as root:

```bash
# Run a command as root
sudo command_here

# Examples
sudo apt update           # Update package lists
sudo cat /etc/shadow      # Read protected file
sudo useradd newuser      # Create a user
```

**How sudo works:**
1. You type `sudo command`
2. System checks if you're in the sudoers file
3. System asks for YOUR password (not root's)
4. Command runs with root privileges
5. Sudo access is cached briefly (usually 15 minutes)

```bash
# See what sudo commands you can run
sudo -l
```

### Becoming root: `sudo -i` and `sudo su`

Sometimes you need an interactive root shell:

```bash
# Open a root shell (recommended way)
sudo -i

# Alternative: switch user to root
sudo su

# Alternative: switch to root with environment
sudo su -
```

**Differences:**
- `sudo -i`: Opens a login shell as root (loads root's environment)
- `sudo su`: Opens a shell as root (keeps some of your environment)
- `sudo su -`: Opens a login shell as root (similar to `sudo -i`)

```bash
# Exit the root shell when done
exit
```

💡 **Tip**: Your prompt usually changes to `#` when you're root:
- Regular user: `kali@kali:~$`
- Root user: `root@kali:~#`

### The `/etc/sudoers` File

The `/etc/sudoers` file controls who can use sudo and what they can do:

```bash
# View sudoers (never edit directly!)
sudo cat /etc/sudoers
```

⚠️ **Warning**: Always edit sudoers with `visudo`, which validates syntax:
```bash
sudo visudo
```

**Common sudoers entries:**
```
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# Allow kali to run all commands without password
kali    ALL=(ALL) NOPASSWD: ALL
```

### The sudo Group / wheel Group

Users in the `sudo` group (or `wheel` on some systems) can use sudo:

```bash
# Check if you're in the sudo group
groups | grep sudo

# Add a user to the sudo group (makes them an admin)
sudo usermod -aG sudo username
```

---

## 5. Creating Users

### `useradd` - Basic User Creation

The `useradd` command creates new users:

```bash
# Basic user creation (minimal - no home directory)
sudo useradd username

# Create user with home directory (-m)
sudo useradd -m username

# Create user with home directory and bash shell
sudo useradd -m -s /bin/bash username

# Create user with specific shell and groups
sudo useradd -m -s /bin/bash -G sudo,users username

# Create user with comment (full name)
sudo useradd -m -s /bin/bash -c "John Doe" johnd
```

**Common useradd options:**

| Option | Description |
|--------|-------------|
| `-m` | Create home directory |
| `-s /bin/bash` | Set login shell |
| `-G group1,group2` | Add to secondary groups |
| `-c "comment"` | Set comment/full name |
| `-d /path` | Specify home directory location |
| `-u UID` | Specify user ID |
| `-g group` | Specify primary group |
| `-e YYYY-MM-DD` | Account expiration date |

**Example: Complete user creation:**
```bash
# Create a developer user
sudo useradd -m -s /bin/bash -G sudo,developers -c "Jane Smith" janes

# Verify
grep janes /etc/passwd
id janes
```

### `adduser` - Interactive User Creation (Debian/Ubuntu/Kali)

On Debian-based systems, `adduser` provides a friendlier interface:

```bash
# Interactive user creation
sudo adduser newuser
```

Output:
```
Adding user `newuser' ...
Adding new group `newuser' (1001) ...
Adding new user `newuser' (1001) with group `newuser' ...
Creating home directory `/home/newuser' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for newuser
Enter the new value, or press ENTER for the default
        Full Name []: New User
        Room Number []: 
        Work Phone []: 
        Home Phone []: 
        Other []: 
Is the information correct? [Y/n] y
```

💡 **Tip**: `adduser` automatically:
- Creates a home directory
- Copies skeleton files from `/etc/skel`
- Prompts for a password
- Asks for user information

### Setting Passwords with `passwd`

After creating a user with `useradd`, set their password:

```bash
# Set password for a user (as root)
sudo passwd username
```

Output:
```
New password: 
Retype new password: 
passwd: password updated successfully
```

---

## 6. Modifying Users

### `usermod` - Modify User Accounts

The `usermod` command modifies existing user accounts:

### Add User to Groups

```bash
# Add user to a group (IMPORTANT: use -a to append!)
sudo usermod -aG groupname username

# Add user to multiple groups
sudo usermod -aG group1,group2 username

# Example: Give user sudo access
sudo usermod -aG sudo janes
```

⚠️ **Warning**: Always use `-aG` (append to groups), not just `-G`. Using `-G` alone removes the user from all other secondary groups!

```bash
# WRONG - removes from all other groups:
sudo usermod -G newgroup username

# CORRECT - adds to group while keeping existing:
sudo usermod -aG newgroup username
```

### Change Username

```bash
# Rename a user (must not be logged in)
sudo usermod -l newname oldname
```

### Change Home Directory

```bash
# Change home directory (doesn't move files)
sudo usermod -d /new/home/path username

# Change home directory and move files
sudo usermod -d /new/home/path -m username
```

### Change Shell

```bash
# Change user's login shell
sudo usermod -s /bin/zsh username

# View available shells
cat /etc/shells
```

### Lock and Unlock Accounts

```bash
# Lock a user account (disable login)
sudo usermod -L username

# Unlock a user account
sudo usermod -U username

# Check if account is locked (look for ! in password field)
sudo grep username /etc/shadow
```

### `usermod` Options Summary

| Option | Description |
|--------|-------------|
| `-aG group` | Append to secondary groups |
| `-l newname` | Change login name |
| `-d /path` | Change home directory |
| `-m` | Move home directory (use with -d) |
| `-s /shell` | Change login shell |
| `-L` | Lock account |
| `-U` | Unlock account |
| `-e YYYY-MM-DD` | Set account expiration |
| `-c "comment"` | Change comment/full name |

---

## 7. Deleting Users

### `userdel` - Delete User Accounts

```bash
# Delete user (keeps home directory)
sudo userdel username

# Delete user AND their home directory
sudo userdel -r username
```

### What Happens to User's Files

When you delete a user:

**Without `-r`:**
- User is removed from `/etc/passwd` and `/etc/shadow`
- Home directory and files remain (owned by the old UID)
- Files elsewhere remain (now owned by a numeric UID)

**With `-r`:**
- User is removed from system files
- Home directory and mail spool are deleted
- Files elsewhere still remain!

```bash
# Find files owned by a deleted user (by UID)
sudo find / -uid 1001 2>/dev/null

# Find files with no valid owner
sudo find / -nouser 2>/dev/null
```

💡 **Tip**: Before deleting a user, consider:
1. Backing up their important files
2. Transferring file ownership
3. Checking for running processes

```bash
# Check for processes owned by user
ps -u username

# Kill all processes for a user (careful!)
sudo pkill -u username
```

---

## 8. Managing Groups

### Create Groups

```bash
# Create a new group
sudo groupadd groupname

# Create group with specific GID
sudo groupadd -g 2000 groupname
```

**Example: Create a developers group:**
```bash
sudo groupadd developers
```

### Rename Groups

```bash
# Rename a group
sudo groupmod -n newname oldname
```

### Delete Groups

```bash
# Delete a group
sudo groupdel groupname
```

⚠️ **Warning**: You can't delete a group if it's any user's primary group.

### Add/Remove Users from Groups

**Using `gpasswd`:**
```bash
# Add user to group
sudo gpasswd -a username groupname

# Remove user from group
sudo gpasswd -d username groupname
```

**Using `usermod`:**
```bash
# Add user to group (remember -a for append!)
sudo usermod -aG groupname username
```

### View Group Information

```bash
# Show group details
getent group groupname

# List all groups
cat /etc/group

# Show groups a user belongs to
groups username
id username
```

---

## 9. Password Management

### Change Your Own Password

```bash
# Change your password
passwd
```

Output:
```
Changing password for kali.
Current password: 
New password: 
Retype new password: 
passwd: password updated successfully
```

### Change Another User's Password (as root)

```bash
# Set password for another user
sudo passwd username
```

### Lock and Unlock Passwords

```bash
# Lock password (prevents login)
sudo passwd -l username

# Unlock password
sudo passwd -u username
```

### Force Password Change

```bash
# Force user to change password on next login
sudo passwd -e username
```

### View Password Status

```bash
# Check password status
sudo passwd -S username
```

Output:
```
username P 12/01/2024 0 99999 7 -1
│        │ │          │ │     │ │
│        │ │          │ │     │ └── Inactive period (-1 = never)
│        │ │          │ │     └── Warning days before expiry
│        │ │          │ └── Maximum password age
│        │ │          └── Minimum password age
│        │ └── Last password change date
│        └── Status (P=usable password, L=locked, NP=no password)
└── Username
```

### Password Policies (Brief Introduction)

Password policies control:
- Minimum/maximum password age
- Password complexity requirements
- Account lockout after failed attempts

```bash
# View password aging info
sudo chage -l username

# Set password to expire in 90 days
sudo chage -M 90 username

# Set minimum days between password changes
sudo chage -m 7 username
```

💡 **Tip**: Enterprise systems often use PAM (Pluggable Authentication Modules) for more sophisticated password policies.

---

## 10. Switching Users

### `su` - Switch User

The `su` command lets you switch to another user:

```bash
# Switch to another user (asks for THEIR password)
su username

# Switch to another user with their environment
su - username

# Switch to root (asks for ROOT password)
su

# Switch to root with root's environment
su -
```

**Difference between `su` and `su -`:**

| Command | Environment | Working Directory |
|---------|-------------|-------------------|
| `su username` | Keeps your environment | Stays in current directory |
| `su - username` | Loads user's environment | Changes to their home |

```bash
# Return to your original user
exit
```

### `su` vs `sudo`

```
┌─────────────────────────────────────────────────────────────────┐
│                    su vs sudo COMPARISON                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  su (Switch User)                                                │
│  ────────────────                                                │
│  • Switches to another user completely                           │
│  • Requires TARGET user's password                               │
│  • Opens a new shell as that user                                │
│  • Use: su - username                                            │
│                                                                  │
│  sudo (Superuser Do)                                             │
│  ────────────────────                                            │
│  • Runs ONE command as another user (default: root)              │
│  • Requires YOUR password                                        │
│  • Returns to your shell after command                           │
│  • Use: sudo command                                             │
│                                                                  │
│  sudo -i / sudo su                                               │
│  ─────────────────                                               │
│  • Opens interactive root shell                                  │
│  • Requires YOUR password                                        │
│  • Combines benefits of both                                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**When to use each:**

| Scenario | Use |
|----------|-----|
| Run one command as root | `sudo command` |
| Need extended root access | `sudo -i` |
| Test as another user | `su - username` |
| Root password is known | `su -` |
| Root password is unknown | `sudo -i` |

💡 **Tip**: Most modern systems prefer `sudo` because:
- It uses your own password (no shared root password)
- It logs all commands run
- It can be configured with fine-grained permissions

---

## 11. Practical Scenarios

### Scenario 1: Creating a New Employee Account

A new developer, John Smith, is joining the team:

```bash
# Create the user with home directory and bash shell
sudo useradd -m -s /bin/bash -c "John Smith" jsmith

# Set a temporary password
sudo passwd jsmith

# Force password change on first login
sudo passwd -e jsmith

# Add to developers group
sudo usermod -aG developers jsmith

# Add sudo access if needed
sudo usermod -aG sudo jsmith

# Verify the setup
id jsmith
ls -la /home/jsmith
```

### Scenario 2: Setting Up Team Access with Groups

Create a shared project directory for developers:

```bash
# Create the developers group (if it doesn't exist)
sudo groupadd developers

# Add team members to the group
sudo usermod -aG developers alice
sudo usermod -aG developers bob
sudo usermod -aG developers carol

# Create shared directory
sudo mkdir -p /projects/webapp

# Set group ownership
sudo chown :developers /projects/webapp

# Set permissions: group can read/write/execute
sudo chmod 2770 /projects/webapp

# The 2 sets SGID - new files inherit the group

# Verify
ls -ld /projects/webapp
# drwxrws--- 2 root developers 4096 Dec  1 10:30 /projects/webapp
```

### Scenario 3: Disabling a Departing Employee

When an employee leaves:

```bash
# 1. Lock the account immediately
sudo usermod -L jsmith

# 2. Kill any running processes
sudo pkill -u jsmith

# 3. Backup their home directory
sudo tar -czvf /backup/jsmith-home-$(date +%Y%m%d).tar.gz /home/jsmith

# 4. Optionally transfer file ownership
sudo find /projects -user jsmith -exec chown newowner:developers {} \;

# 5. When ready, delete the account
sudo userdel jsmith

# Or delete with home directory
sudo userdel -r jsmith
```

### Scenario 4: Security Best Practices

```bash
# Check for users with no password
sudo awk -F: '($2 == "") {print $1}' /etc/shadow

# Check for UID 0 users (should only be root)
awk -F: '($3 == 0) {print $1}' /etc/passwd

# Find users with login shells
grep -v '/nologin\|/false' /etc/passwd

# Check for users in sudo group
getent group sudo

# Check recent successful logins
last -10

# Check failed login attempts
sudo lastb -10
```

---

## Quick Reference

### User Commands Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│                USER MANAGEMENT COMMANDS                       │
├──────────────────────────────────────────────────────────────┤
│  VIEWING INFORMATION                                          │
│  ──────────────────                                           │
│  whoami              Show current username                    │
│  id                  Show user/group IDs                      │
│  id username         Show IDs for specific user               │
│  groups              Show group memberships                   │
│  who                 Show logged-in users                     │
│  w                   Show users and activity                  │
│  last                Show login history                       │
├──────────────────────────────────────────────────────────────┤
│  CREATING USERS                                               │
│  ──────────────                                               │
│  useradd -m user     Create user with home directory          │
│  useradd -m -s /bin/bash user    With bash shell              │
│  useradd -m -G grp user    With secondary group               │
│  adduser user        Interactive creation (Debian)            │
│  passwd user         Set/change password                      │
├──────────────────────────────────────────────────────────────┤
│  MODIFYING USERS                                              │
│  ──────────────                                               │
│  usermod -aG grp user    Add to group (APPEND!)               │
│  usermod -L user         Lock account                         │
│  usermod -U user         Unlock account                       │
│  usermod -s /shell user  Change shell                         │
│  usermod -l new old      Rename user                          │
├──────────────────────────────────────────────────────────────┤
│  DELETING USERS                                               │
│  ─────────────                                                │
│  userdel user        Delete user (keep home)                  │
│  userdel -r user     Delete user and home directory           │
├──────────────────────────────────────────────────────────────┤
│  GROUP MANAGEMENT                                             │
│  ────────────────                                             │
│  groupadd grp        Create group                             │
│  groupdel grp        Delete group                             │
│  gpasswd -a user grp Add user to group                        │
│  gpasswd -d user grp Remove user from group                   │
├──────────────────────────────────────────────────────────────┤
│  SUDO AND SWITCHING                                           │
│  ──────────────────                                           │
│  sudo command        Run command as root                      │
│  sudo -i             Open root shell                          │
│  su - user           Switch to user                           │
│  exit                Return to previous user                  │
└──────────────────────────────────────────────────────────────┘
```

### Important Files

| File | Purpose |
|------|---------|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted passwords |
| `/etc/group` | Group information |
| `/etc/sudoers` | Sudo configuration |
| `/etc/skel/` | Template for new home directories |
| `/etc/login.defs` | Default user/password settings |

---

## ✅ What You Learned

In this module, you learned:

- [x] What users are and how they're identified (UID)
- [x] The structure of `/etc/passwd`, `/etc/shadow`, and `/etc/group`
- [x] How to view user information with `whoami`, `id`, `groups`, `who`, `w`, `last`
- [x] What root is and why you shouldn't always use it
- [x] How to use `sudo` to run commands as root
- [x] How to create users with `useradd` and `adduser`
- [x] How to modify users with `usermod`
- [x] How to delete users with `userdel`
- [x] How to manage groups with `groupadd`, `groupdel`, `gpasswd`
- [x] How to manage passwords with `passwd`
- [x] The difference between `su` and `sudo`
- [x] Real-world user management scenarios

---

## Next Steps

In the next module, we'll explore **process management** - how to view running processes, manage background jobs, and control system services.

Practice the exercises to reinforce your understanding of user and group management!