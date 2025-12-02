# Module 6 Exercises: User and Group Management

## 🎯 Exercise Goals

These exercises will help you:
- View and interpret user and group information
- Understand the `/etc/passwd` and `/etc/group` files
- Use sudo effectively and safely
- Create, modify, and delete users
- Manage groups and group membership
- Apply user management to real-world scenarios

---

## Exercise 1: Explore User Information

### Objective
Learn to view information about users on the system.

### Tasks

**1.1 Find your username and UID**

Run the appropriate commands to answer:
- What is your username?
- What is your UID (User ID)?
- What is your primary GID (Group ID)?

```bash
# Your commands here
```

**1.2 List your group memberships**

Find out all the groups you belong to:

```bash
# Your command here
```

**1.3 View detailed identity information**

Use the `id` command to see your complete identity:

```bash
# Your command here
```

What groups are you a member of?

**1.4 Check who is currently logged in**

```bash
# Show who is logged in
# Your command here

# Show who is logged in and what they're doing
# Your command here
```

**1.5 View login history**

```bash
# Show recent logins
# Your command here

# Show last 5 logins only
# Your command here
```

<details>
<summary>💡 Hints</summary>

- `whoami` shows your username
- `id` shows UIDs, GIDs, and group memberships
- `groups` shows group names only
- `who` shows logged-in users
- `w` shows users with more detail
- `last` shows login history
- `last -n 5` or `last -5` limits output

</details>

<details>
<summary>✅ Solutions</summary>

**1.1 Username and UID:**
```bash
$ whoami
kali

$ id
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo),100(users)

# UID is 1000, GID is 1000
```

**1.2 Group memberships:**
```bash
$ groups
kali sudo users
```

**1.3 Detailed identity:**
```bash
$ id
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo),100(users)

# Primary group: kali (1000)
# Secondary groups: sudo (27), users (100)
```

**1.4 Who is logged in:**
```bash
$ who
kali     tty7         2024-12-01 10:30 (:0)
kali     pts/0        2024-12-01 10:35 (:0)

$ w
 10:45:32 up 2 days,  3:22,  2 users,  load average: 0.15, 0.10, 0.09
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
kali     tty7     :0               Mon10    2days  1:23   0.05s /usr/bin/startplasma
kali     pts/0    :0               10:35    0.00s  0.05s  0.00s w
```

**1.5 Login history:**
```bash
$ last
kali     pts/0        :0               Mon Dec  1 10:35   still logged in
kali     tty7         :0               Mon Dec  1 10:30   still logged in
...

$ last -5
# Shows only last 5 entries
```

</details>

---

## Exercise 2: Understanding /etc/passwd

### Objective
Learn to read and interpret the `/etc/passwd` file.

### Tasks

**2.1 View the passwd file**

```bash
# View the entire file
cat /etc/passwd

# Count the number of lines
wc -l /etc/passwd
```

How many user accounts exist on your system?

**2.2 Parse a line from /etc/passwd**

Find your own entry and break it down:

```bash
# Find your entry
grep $USER /etc/passwd
```

Fill in this table for your user:

| Field | Value | Meaning |
|-------|-------|---------|
| Username | | |
| Password field | | |
| UID | | |
| GID | | |
| Comment/GECOS | | |
| Home directory | | |
| Shell | | |

**2.3 Find the root user entry**

```bash
# Your command here
```

What is root's:
- UID?
- GID?
- Home directory?
- Shell?

**2.4 Identify system vs regular users**

```bash
# Find users with UID less than 1000 (system users)
awk -F: '$3 < 1000 {print $1, $3}' /etc/passwd

# Find users with UID 1000 or greater (regular users)
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd
```

How many system users are there? How many regular users?

**2.5 Find users with bash shell**

```bash
# Your command here
```

**2.6 Find users without a valid login shell**

```bash
# Find users with /usr/sbin/nologin or /bin/false
grep -E 'nologin|/bin/false' /etc/passwd
```

Why do these accounts have these shells?

<details>
<summary>💡 Hints</summary>

- `/etc/passwd` format: `user:x:UID:GID:comment:home:shell`
- Use `grep` to find specific entries
- Use `awk -F:` to parse the colon-separated fields
- System users typically have UID < 1000
- `/usr/sbin/nologin` prevents interactive login

</details>

<details>
<summary>✅ Solutions</summary>

**2.2 Your entry breakdown:**
```bash
$ grep $USER /etc/passwd
kali:x:1000:1000:,,,:/home/kali:/bin/zsh
```

| Field | Value | Meaning |
|-------|-------|---------|
| Username | kali | Login name |
| Password field | x | Password in /etc/shadow |
| UID | 1000 | User ID number |
| GID | 1000 | Primary group ID |
| Comment/GECOS | ,,, | Full name (empty) |
| Home directory | /home/kali | User's home |
| Shell | /bin/zsh | Login shell |

**2.3 Root user entry:**
```bash
$ grep ^root /etc/passwd
root:x:0:0:root:/root:/bin/bash
```

- UID: 0
- GID: 0
- Home directory: /root
- Shell: /bin/bash

**2.4 System vs regular users:**
```bash
$ awk -F: '$3 < 1000 {print $1, $3}' /etc/passwd | wc -l
# Number of system users (typically 20-40)

$ awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd
kali 1000
# Usually just your user(s)
```

**2.5 Users with bash shell:**
```bash
$ grep '/bin/bash' /etc/passwd
root:x:0:0:root:/root:/bin/bash
```

**2.6 Users without valid login shell:**
These are system accounts for services (www-data, nobody, daemon, etc.). They have `/usr/sbin/nologin` to prevent anyone from logging in as them interactively - they only run background services.

</details>

---

## Exercise 3: Using sudo

### Objective
Understand how to use sudo safely and effectively.

### Tasks

**3.1 Check your sudo privileges**

```bash
# See what you can run with sudo
sudo -l
```

What commands are you allowed to run?

**3.2 Run a command as root**

```bash
# Try to read the shadow file without sudo (should fail)
cat /etc/shadow

# Now use sudo
sudo cat /etc/shadow | head -5
```

What error did you get without sudo?

**3.3 Try commands that require sudo**

```bash
# Try these without sudo first, then with sudo:

# Create a user (without sudo - should fail)
useradd testfail

# Read shadow file (without sudo - should fail)
cat /etc/shadow

# Install a package (without sudo - should fail)
apt update
```

**3.4 Switch to root and back**

```bash
# Become root
sudo -i

# Verify you are root
whoami
id

# Check the prompt (should show # instead of $)

# Exit back to your normal user
exit

# Verify you're back
whoami
```

**3.5 Run a single command as root without staying root**

```bash
# This runs one command and returns
sudo whoami

# You're still yourself
whoami
```

**3.6 Understanding sudo vs su**

If you know the root password, try:
```bash
# Switch to root using su (requires root password)
su -

# Exit
exit
```

On Kali, if running as a regular user, try:
```bash
# This uses YOUR password
sudo -i

# This would need ROOT's password
su -
```

<details>
<summary>💡 Hints</summary>

- `sudo -l` shows your sudo capabilities
- `sudo command` runs one command as root
- `sudo -i` opens a root shell
- `su -` requires the root password
- On Kali, your user is typically in the sudo group
- The prompt changes from `$` to `#` when you're root

</details>

<details>
<summary>✅ Solutions</summary>

**3.1 Sudo privileges:**
```bash
$ sudo -l
Matching Defaults entries for kali on kali:
    env_reset, mail_badpass, ...

User kali may run the following commands on kali:
    (ALL : ALL) ALL
```
This means you can run ALL commands as ANY user.

**3.2 Reading shadow file:**
```bash
$ cat /etc/shadow
cat: /etc/shadow: Permission denied

$ sudo cat /etc/shadow | head -5
root:$y$j9T$...:19691:0:99999:7:::
daemon:*:19405:0:99999:7:::
bin:*:19405:0:99999:7:::
sys:*:19405:0:99999:7:::
sync:*:19405:0:99999:7:::
```

**3.3 Commands requiring sudo:**
```bash
$ useradd testfail
useradd: Permission denied.
useradd: cannot lock /etc/passwd; try again later.

$ cat /etc/shadow
cat: /etc/shadow: Permission denied

$ apt update
Reading package lists... Done
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
```

**3.4 Switching to root:**
```bash
$ sudo -i
root@kali:~# whoami
root
root@kali:~# id
uid=0(root) gid=0(root) groups=0(root)
root@kali:~# exit
logout
$ whoami
kali
```

Notice the prompt changed from `$` to `#` and back.

</details>

---

## Exercise 4: Create a New User

### Objective
Practice creating user accounts with various options.

### Tasks

**4.1 Create a basic user with home directory**

```bash
# Create user 'testuser' with home directory
sudo useradd -m testuser

# Verify the user was created
grep testuser /etc/passwd

# Check the home directory
ls -la /home/testuser
```

**4.2 Set a password for the user**

```bash
# Set password for testuser
sudo passwd testuser
# Enter a password when prompted (e.g., "test123")
```

**4.3 View the new user's information**

```bash
# Check user details
id testuser

# Check group membership
groups testuser

# Check password status
sudo passwd -S testuser
```

**4.4 Log in as the new user**

```bash
# Switch to the new user
su - testuser

# Verify
whoami
pwd

# Check the environment
echo $HOME
echo $SHELL

# Exit back to your user
exit
```

**4.5 Check the user in system files**

```bash
# View in passwd
grep testuser /etc/passwd

# View in shadow (password hash)
sudo grep testuser /etc/shadow

# View in group
grep testuser /etc/group
```

**4.6 Create a more complete user**

```bash
# Create a user with all options
sudo useradd -m -s /bin/bash -c "Test Developer" -G users testdev

# Set password
sudo passwd testdev

# Verify
grep testdev /etc/passwd
id testdev
```

<details>
<summary>💡 Hints</summary>

- `-m` creates the home directory
- `-s /bin/bash` sets the shell
- `-c "comment"` sets the full name
- `-G group` adds to secondary groups
- Use `passwd username` to set passwords
- Use `su - username` to test login

</details>

<details>
<summary>✅ Solutions</summary>

**4.1 Creating basic user:**
```bash
$ sudo useradd -m testuser
$ grep testuser /etc/passwd
testuser:x:1001:1001::/home/testuser:/bin/sh

$ ls -la /home/testuser
total 20
drwxr-xr-x 2 testuser testuser 4096 Dec  1 10:30 .
drwxr-xr-x 4 root     root     4096 Dec  1 10:30 ..
-rw-r--r-- 1 testuser testuser  220 Dec  1 10:30 .bash_logout
-rw-r--r-- 1 testuser testuser 3771 Dec  1 10:30 .bashrc
-rw-r--r-- 1 testuser testuser  807 Dec  1 10:30 .profile
```

**4.3 User information:**
```bash
$ id testuser
uid=1001(testuser) gid=1001(testuser) groups=1001(testuser)

$ groups testuser
testuser : testuser

$ sudo passwd -S testuser
testuser P 12/01/2024 0 99999 7 -1
```

**4.4 Logging in as new user:**
```bash
$ su - testuser
Password: 
$ whoami
testuser
$ pwd
/home/testuser
$ echo $HOME
/home/testuser
$ exit
```

**4.6 Complete user creation:**
```bash
$ grep testdev /etc/passwd
testdev:x:1002:1002:Test Developer:/home/testdev:/bin/bash

$ id testdev
uid=1002(testdev) gid=1002(testdev) groups=1002(testdev),100(users)
```

</details>

---

## Exercise 5: Manage Groups

### Objective
Learn to create groups and manage group membership.

### Tasks

**5.1 Create a new group called "developers"**

```bash
# Create the group
sudo groupadd developers

# Verify it was created
grep developers /etc/group

# Check the GID
getent group developers
```

**5.2 Add your test user to the group**

```bash
# Add testuser to developers group
sudo usermod -aG developers testuser

# Verify group membership
id testuser
groups testuser
```

⚠️ **Important**: Why did we use `-aG` and not just `-G`?

**5.3 Verify group membership**

```bash
# Check from the group side
grep developers /etc/group

# Check from the user side
id testuser

# Log in as testuser and check groups
su - testuser
groups
exit
```

**5.4 Create a shared directory for the group**

```bash
# Create the directory
sudo mkdir -p /projects/devwork

# Set group ownership
sudo chown :developers /projects/devwork

# Set permissions (group can read/write/execute)
sudo chmod 770 /projects/devwork

# Enable SGID for group inheritance
sudo chmod g+s /projects/devwork

# Verify
ls -ld /projects/devwork
```

**5.5 Test file access with group permissions**

```bash
# As your regular user (if in developers group) or testuser
su - testuser

# Try to access the directory
cd /projects/devwork

# Create a file
touch myfile.txt

# Check who owns it
ls -l myfile.txt

# Exit
exit
```

**5.6 Use gpasswd to manage group members**

```bash
# Add testdev to developers using gpasswd
sudo gpasswd -a testdev developers

# Verify
grep developers /etc/group

# Remove a user from group
sudo gpasswd -d testdev developers

# Verify removal
grep developers /etc/group
```

<details>
<summary>💡 Hints</summary>

- `groupadd` creates groups
- `usermod -aG` adds to group (keep `-a` for append!)
- `gpasswd -a` adds user to group
- `gpasswd -d` removes user from group
- Users must log out/in for group changes to take effect
- Use `newgrp groupname` to activate group membership without logout

</details>

<details>
<summary>✅ Solutions</summary>

**5.1 Creating group:**
```bash
$ sudo groupadd developers
$ grep developers /etc/group
developers:x:1003:
```

**5.2 Adding to group:**
```bash
$ sudo usermod -aG developers testuser
$ id testuser
uid=1001(testuser) gid=1001(testuser) groups=1001(testuser),1003(developers)
```

We use `-aG` because:
- `-a` = append (add to existing groups)
- `-G` = set secondary groups
- Without `-a`, the user would be REMOVED from all other secondary groups!

**5.4 Shared directory:**
```bash
$ ls -ld /projects/devwork
drwxrws--- 2 root developers 4096 Dec  1 10:30 /projects/devwork
```

The `s` in the group permissions indicates SGID is set.

**5.5 Testing access:**
```bash
$ su - testuser
$ cd /projects/devwork
$ touch myfile.txt
$ ls -l myfile.txt
-rw-r--r-- 1 testuser developers 0 Dec  1 10:30 myfile.txt
```

Notice the file belongs to the `developers` group (because of SGID).

</details>

---

## Exercise 6: Modify and Delete Users

### Objective
Practice modifying user accounts and safely deleting users.

### Tasks

**6.1 Change a user's shell**

```bash
# Check testuser's current shell
grep testuser /etc/passwd

# Change shell to bash
sudo usermod -s /bin/bash testuser

# Verify
grep testuser /etc/passwd
```

**6.2 Add a user to additional groups**

```bash
# Create another group
sudo groupadd testers

# Add testuser to testers (keeping existing groups)
sudo usermod -aG testers testuser

# Verify testuser is in both developers and testers
id testuser
```

**6.3 Lock and unlock an account**

```bash
# Lock the account
sudo usermod -L testdev

# Check the shadow file (notice the ! before the password)
sudo grep testdev /etc/shadow

# Try to login (should fail)
su - testdev

# Unlock the account
sudo usermod -U testdev

# Check shadow again (! should be gone)
sudo grep testdev /etc/shadow

# Try login again (should work)
su - testdev
exit
```

**6.4 Change user comment (full name)**

```bash
# Change the GECOS field
sudo usermod -c "Test User Account" testuser

# Verify
grep testuser /etc/passwd
```

**6.5 Delete a user (safely)**

```bash
# First, let's check what the user owns
find /home -user testdev 2>/dev/null
find /tmp -user testdev 2>/dev/null

# Delete user but keep home directory
sudo userdel testdev

# Verify user is gone
grep testdev /etc/passwd

# Home directory still exists
ls /home/testdev

# Clean up manually if desired
sudo rm -rf /home/testdev
```

**6.6 Delete a user with home directory**

```bash
# Create a temporary user for this exercise
sudo useradd -m tempuser
sudo passwd tempuser

# Delete user AND home directory
sudo userdel -r tempuser

# Verify both are gone
grep tempuser /etc/passwd
ls /home/tempuser
```

**6.7 Clean up test accounts**

```bash
# Delete testuser and home directory
sudo userdel -r testuser

# Delete the groups we created
sudo groupdel developers
sudo groupdel testers

# Clean up directories
sudo rm -rf /projects

# Verify cleanup
grep testuser /etc/passwd
grep developers /etc/group
ls /home/
```

<details>
<summary>💡 Hints</summary>

- `usermod -s` changes shell
- `usermod -aG` adds to group (keep `-a`!)
- `usermod -L` locks, `-U` unlocks
- `usermod -c` changes comment/full name
- `userdel` removes user
- `userdel -r` removes user AND home directory
- Always check for files owned by user before deleting

</details>

<details>
<summary>✅ Solutions</summary>

**6.1 Changing shell:**
```bash
$ grep testuser /etc/passwd
testuser:x:1001:1001::/home/testuser:/bin/sh

$ sudo usermod -s /bin/bash testuser

$ grep testuser /etc/passwd
testuser:x:1001:1001::/home/testuser:/bin/bash
```

**6.3 Locked account:**
```bash
$ sudo grep testdev /etc/shadow
testdev:!$y$j9T$...:19691:0:99999:7:::

# The ! before the hash indicates the account is locked

$ su - testdev
Password: 
su: Authentication failure
```

After unlocking:
```bash
$ sudo grep testdev /etc/shadow
testdev:$y$j9T$...:19691:0:99999:7:::

# No ! means the account is unlocked
```

**6.5 Safe deletion:**
```bash
$ sudo userdel testdev
$ grep testdev /etc/passwd
# (no output - user is gone)

$ ls /home/testdev
# Home directory still exists!
```

**6.6 Complete deletion:**
```bash
$ sudo userdel -r tempuser
$ ls /home/tempuser
ls: cannot access '/home/tempuser': No such file or directory
```

</details>

---

## Challenge Exercise: Set Up a Project Team

### Objective
Apply everything you've learned to set up a complete project team environment.

### Scenario

You're a system administrator setting up access for a new project:
- **Team members**: dev1, dev2, manager
- **Groups**: developers (dev1, dev2), managers (manager), project-alpha (everyone)
- **Shared directories**: `/projects/alpha/code` (developers), `/projects/alpha/reports` (managers), `/projects/alpha/shared` (everyone)

### Tasks

**Step 1: Create the users**

```bash
# Create dev1 with home directory and bash shell
sudo useradd -m -s /bin/bash -c "Developer One" dev1
sudo passwd dev1

# Create dev2
sudo useradd -m -s /bin/bash -c "Developer Two" dev2
sudo passwd dev2

# Create manager
sudo useradd -m -s /bin/bash -c "Project Manager" manager
sudo passwd manager

# Verify all users
grep -E 'dev1|dev2|manager' /etc/passwd
```

**Step 2: Create the groups**

```bash
# Create developers group
sudo groupadd developers

# Create managers group
sudo groupadd managers

# Create project-alpha group (for everyone)
sudo groupadd project-alpha

# Verify groups
grep -E 'developers|managers|project-alpha' /etc/group
```

**Step 3: Add users to appropriate groups**

```bash
# Add dev1 and dev2 to developers and project-alpha
sudo usermod -aG developers,project-alpha dev1
sudo usermod -aG developers,project-alpha dev2

# Add manager to managers and project-alpha
sudo usermod -aG managers,project-alpha manager

# Verify memberships
id dev1
id dev2
id manager
```

**Step 4: Create shared project directories**

```bash
# Create directory structure
sudo mkdir -p /projects/alpha/{code,reports,shared}

# Verify
ls -la /projects/alpha/
```

**Step 5: Set up proper permissions**

```bash
# Code directory - developers only
sudo chown :developers /projects/alpha/code
sudo chmod 2770 /projects/alpha/code

# Reports directory - managers only
sudo chown :managers /projects/alpha/reports
sudo chmod 2770 /projects/alpha/reports

# Shared directory - everyone in project-alpha
sudo chown :project-alpha /projects/alpha/shared
sudo chmod 2770 /projects/alpha/shared

# Verify permissions
ls -la /projects/alpha/
```

**Step 6: Test access as each user**

```bash
# Test as dev1
echo "Testing as dev1..."
su - dev1 -c "touch /projects/alpha/code/dev1-test.py && echo 'Code: OK' || echo 'Code: FAIL'"
su - dev1 -c "touch /projects/alpha/reports/dev1-test.txt 2>/dev/null && echo 'Reports: OK' || echo 'Reports: FAIL (expected)'"
su - dev1 -c "touch /projects/alpha/shared/dev1-test.txt && echo 'Shared: OK' || echo 'Shared: FAIL'"

# Test as dev2
echo "Testing as dev2..."
su - dev2 -c "touch /projects/alpha/code/dev2-test.py && echo 'Code: OK' || echo 'Code: FAIL'"
su - dev2 -c "touch /projects/alpha/reports/dev2-test.txt 2>/dev/null && echo 'Reports: OK' || echo 'Reports: FAIL (expected)'"
su - dev2 -c "touch /projects/alpha/shared/dev2-test.txt && echo 'Shared: OK' || echo 'Shared: FAIL'"

# Test as manager
echo "Testing as manager..."
su - manager -c "touch /projects/alpha/code/manager-test.py 2>/dev/null && echo 'Code: OK' || echo 'Code: FAIL (expected)'"
su - manager -c "touch /projects/alpha/reports/manager-test.txt && echo 'Reports: OK' || echo 'Reports: FAIL'"
su - manager -c "touch /projects/alpha/shared/manager-test.txt && echo 'Shared: OK' || echo 'Shared: FAIL'"
```

**Step 7: Document the setup**

Create a summary of what you set up:

```bash
echo "=== Project Alpha Access Summary ==="
echo ""
echo "Users:"
grep -E 'dev1|dev2|manager' /etc/passwd | awk -F: '{print "  " $1 " (UID: " $3 ") - " $5}'
echo ""
echo "Groups:"
grep -E 'developers|managers|project-alpha' /etc/group
echo ""
echo "Directory Permissions:"
ls -la /projects/alpha/
echo ""
echo "Directory Contents:"
ls -laR /projects/alpha/
```

**Step 8: Clean up (when done practicing)**

```bash
# Delete users
sudo userdel -r dev1
sudo userdel -r dev2
sudo userdel -r manager

# Delete groups
sudo groupdel developers
sudo groupdel managers
sudo groupdel project-alpha

# Delete project directory
sudo rm -rf /projects

# Verify cleanup
echo "Users remaining:"
grep -E 'dev1|dev2|manager' /etc/passwd
echo "Groups remaining:"
grep -E 'developers|managers|project-alpha' /etc/group
```

<details>
<summary>💡 Hints</summary>

- Use `-m` to create home directories
- Use `-aG` to add to groups (always include `-a`!)
- Use `2770` permissions: owner and group rwx, SGID set
- SGID (the `2` in 2770) makes new files inherit the directory's group
- Test access by switching users with `su - username`
- Remember: users need to log out/in for group changes to take effect
- Use `newgrp groupname` to activate new group membership immediately

</details>

<details>
<summary>✅ Expected Results</summary>

**Step 5: Directory permissions:**
```bash
$ ls -la /projects/alpha/
total 20
drwxr-xr-x 5 root root          4096 Dec  1 10:30 .
drwxr-xr-x 3 root root          4096 Dec  1 10:30 ..
drwxrws--- 2 root developers    4096 Dec  1 10:30 code
drwxrws--- 2 root managers      4096 Dec  1 10:30 reports
drwxrws--- 2 root project-alpha 4096 Dec  1 10:30 shared
```

**Step 6: Access test results:**
```
Testing as dev1...
Code: OK
Reports: FAIL (expected)
Shared: OK

Testing as dev2...
Code: OK
Reports: FAIL (expected)
Shared: OK

Testing as manager...
Code: FAIL (expected)
Reports: OK
Shared: OK
```

This shows that:
- Developers (dev1, dev2) can access code and shared, but NOT reports
- Manager can access reports and shared, but NOT code
- Everyone can access shared

**Step 7: Final directory contents:**
```bash
$ ls -laR /projects/alpha/
/projects/alpha/:
drwxrws--- 2 root developers    4096 Dec  1 10:30 code
drwxrws--- 2 root managers      4096 Dec  1 10:30 reports
drwxrws--- 2 root project-alpha 4096 Dec  1 10:30 shared

/projects/alpha/code:
-rw-r--r-- 1 dev1 developers 0 Dec  1 10:30 dev1-test.py
-rw-r--r-- 1 dev2 developers 0 Dec  1 10:30 dev2-test.py

/projects/alpha/reports:
-rw-r--r-- 1 manager managers 0 Dec  1 10:30 manager-test.txt

/projects/alpha/shared:
-rw-r--r-- 1 dev1    project-alpha 0 Dec  1 10:30 dev1-test.txt
-rw-r--r-- 1 dev2    project-alpha 0 Dec  1 10:30 dev2-test.txt
-rw-r--r-- 1 manager project-alpha 0 Dec  1 10:30 manager-test.txt
```

Notice how all files have the directory's group ownership (thanks to SGID).

</details>

---

## Bonus Challenges

### Challenge A: Security Audit

Find potential security issues:

```bash
# Find users with UID 0 (should only be root)
awk -F: '($3 == 0) {print $1}' /etc/passwd

# Find users with no password set
sudo awk -F: '($2 == "") {print $1}' /etc/shadow

# Find users with password never set
sudo awk -F: '($2 == "!") {print $1}' /etc/shadow

# Find users with login shells who aren't system accounts
awk -F: '($7 !~ /nologin|false/ && $3 >= 1000) {print $1, $7}' /etc/passwd

# Check who's in the sudo group
getent group sudo
```

### Challenge B: Create an Onboarding Script

Create a script that automates new user creation:

```bash
cat > create_user.sh << 'EOF'
#!/bin/bash
# Simple user creation script

if [ $# -lt 2 ]; then
    echo "Usage: $0 username 'Full Name' [group1,group2,...]"
    exit 1
fi

USERNAME=$1
FULLNAME=$2
GROUPS=${3:-users}

# Create user
sudo useradd -m -s /bin/bash -c "$FULLNAME" -G "$GROUPS" "$USERNAME"

# Set password
echo "Set password for $USERNAME:"
sudo passwd "$USERNAME"

# Force password change on first login
sudo passwd -e "$USERNAME"

# Show result
echo ""
echo "User created:"
id "$USERNAME"
echo "Home directory:"
ls -la /home/"$USERNAME"/
EOF

chmod +x create_user.sh
```

Test it:
```bash
./create_user.sh newdev "New Developer" "developers,users"
```

### Challenge C: User Activity Report

Create a script to report on user activity:

```bash
cat > user_report.sh << 'EOF'
#!/bin/bash
echo "=== User Activity Report ==="
echo "Generated: $(date)"
echo ""

echo "Currently logged in users:"
who
echo ""

echo "Recent logins (last 10):"
last -10
echo ""

echo "Users with sudo access:"
getent group sudo | cut -d: -f4
echo ""

echo "Regular users on system:"
awk -F: '$3 >= 1000 && $7 !~ /nologin|false/ {print $1, "(" $5 ")"}' /etc/passwd
EOF

chmod +x user_report.sh
./user_report.sh
```

---

## 📝 Summary

After completing these exercises, you should be able to:

- ✅ View user information with `whoami`, `id`, `groups`, `who`, `w`, `last`
- ✅ Read and interpret `/etc/passwd` entries
- ✅ Identify system users vs regular users
- ✅ Use `sudo` to run commands as root
- ✅ Create users with `useradd` and `adduser`
- ✅ Set and manage passwords with `passwd`
- ✅ Create and manage groups with `groupadd` and `gpasswd`
- ✅ Modify users with `usermod` (especially `-aG` for groups)
- ✅ Lock and unlock user accounts
- ✅ Safely delete users with `userdel`
- ✅ Set up shared directories with proper group permissions
- ✅ Apply user management to real-world team scenarios

## Cleanup

To clean up after all exercises:

```bash
# Delete any remaining test users
for user in testuser testdev dev1 dev2 manager newdev tempuser; do
    sudo userdel -r $user 2>/dev/null
done

# Delete test groups
for group in developers managers project-alpha testers; do
    sudo groupdel $group 2>/dev/null
done

# Remove test directories
sudo rm -rf /projects

# Remove scripts
rm -f create_user.sh user_report.sh

echo "Cleanup complete!"