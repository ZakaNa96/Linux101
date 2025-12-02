# Module 5 Exercises: File Permissions and Ownership

## 🎯 Exercise Goals

These exercises will help you:
- Read and interpret permission strings
- Change permissions using symbolic and numeric modes
- Make scripts executable
- Manage file ownership
- Apply permissions to real-world scenarios

---

## Exercise 1: Reading Permissions

### Objective
Learn to interpret permission strings from `ls -l` output.

### Tasks

**1.1 Explore system directories**

Run `ls -l` on the following directories and observe the permissions:

```bash
ls -l /etc/passwd
ls -l /etc/shadow
ls -l /bin/ls
ls -l /tmp
ls -ld /home
ls -ld /root
```

**1.2 Answer these questions for each file:**

For each file/directory above, fill in this table:

| Path | File Type | Owner Perms | Group Perms | Others Perms | Owner | Group |
|------|-----------|-------------|-------------|--------------|-------|-------|
| /etc/passwd | | | | | | |
| /etc/shadow | | | | | | |
| /bin/ls | | | | | | |
| /tmp | | | | | | |
| /home | | | | | | |
| /root | | | | | | |

**1.3 Interpret these permission strings:**

What do these permission strings mean? Who can do what?

1. `-rw-r--r--`
2. `drwxr-xr-x`
3. `-rwx------`
4. `-rw-rw----`
5. `lrwxrwxrwx`

<details>
<summary>💡 Hints</summary>

- The first character indicates file type (`-` = file, `d` = directory, `l` = link)
- Positions 2-4 are owner permissions
- Positions 5-7 are group permissions
- Positions 8-10 are others permissions
- `r` = read, `w` = write, `x` = execute, `-` = no permission

</details>

<details>
<summary>✅ Solutions</summary>

**1.1 Expected Output (may vary slightly):**

```bash
$ ls -l /etc/passwd
-rw-r--r-- 1 root root 2925 Dec  1 10:30 /etc/passwd

$ ls -l /etc/shadow
-rw-r----- 1 root shadow 1460 Dec  1 10:30 /etc/shadow

$ ls -l /bin/ls
-rwxr-xr-x 1 root root 138856 Dec  1 10:30 /bin/ls

$ ls -ld /tmp
drwxrwxrwt 15 root root 4096 Dec  1 10:30 /tmp

$ ls -ld /home
drwxr-xr-x 3 root root 4096 Dec  1 10:30 /home

$ ls -ld /root
drwx------ 14 root root 4096 Dec  1 10:30 /root
```

**1.2 Table Answers:**

| Path | File Type | Owner Perms | Group Perms | Others Perms | Owner | Group |
|------|-----------|-------------|-------------|--------------|-------|-------|
| /etc/passwd | File | rw- | r-- | r-- | root | root |
| /etc/shadow | File | rw- | r-- | --- | root | shadow |
| /bin/ls | File | rwx | r-x | r-x | root | root |
| /tmp | Directory | rwx | rwx | rwt | root | root |
| /home | Directory | rwx | r-x | r-x | root | root |
| /root | Directory | rwx | --- | --- | root | root |

**1.3 Permission String Meanings:**

1. `-rw-r--r--`: Regular file. Owner can read/write. Group and others can only read.
2. `drwxr-xr-x`: Directory. Owner has full access. Group and others can read and enter.
3. `-rwx------`: Regular file. Only owner can read, write, and execute. Nobody else has access.
4. `-rw-rw----`: Regular file. Owner and group can read/write. Others have no access.
5. `lrwxrwxrwx`: Symbolic link. All permissions shown (links always show 777, actual permissions depend on target).

</details>

---

## Exercise 2: Create and Modify Permissions (Symbolic Mode)

### Objective
Practice changing permissions using symbolic notation.

### Setup

```bash
# Create a practice directory
mkdir -p ~/permissions_practice
cd ~/permissions_practice

# Create test files
touch file1.txt file2.txt file3.txt file4.txt
echo "Test content" > file1.txt
echo "Test content" > file2.txt
echo "Test content" > file3.txt
echo "Test content" > file4.txt

# Check initial permissions
ls -l
```

### Tasks

**2.1 Add execute permission for the owner to file1.txt**

```bash
# Your command here
```

Verify with `ls -l file1.txt`

**2.2 Remove write permission from group for file2.txt**

```bash
# Your command here
```

Verify the change.

**2.3 Make file3.txt readable by everyone, but only writable by owner**

```bash
# Your command here
```

Expected result: `-rw-r--r--`

**2.4 Remove ALL permissions for others from file4.txt**

```bash
# Your command here
```

**2.5 Create a new file and set complex permissions**

Create `secret.txt` with:
- Owner: read, write, execute
- Group: read only
- Others: no permissions

```bash
# Create the file and set permissions
```

<details>
<summary>💡 Hints</summary>

- Use `u`, `g`, `o`, or `a` to specify who
- Use `+` to add, `-` to remove, `=` to set exactly
- You can combine multiple changes: `chmod u+x,g-w file`
- To set exact permissions, use `=`: `chmod o= file` removes all for others

</details>

<details>
<summary>✅ Solutions</summary>

**2.1 Add execute for owner:**
```bash
chmod u+x file1.txt
ls -l file1.txt
# -rwxr--r-- 1 kali kali 13 Dec  1 10:30 file1.txt
```

**2.2 Remove write from group:**
```bash
chmod g-w file2.txt
ls -l file2.txt
# -rw-r--r-- 1 kali kali 13 Dec  1 10:30 file2.txt
# (Note: default files often don't have group write, so this might not change anything visible)
```

**2.3 Make readable by everyone, writable only by owner:**
```bash
chmod u=rw,g=r,o=r file3.txt
# OR
chmod 644 file3.txt
ls -l file3.txt
# -rw-r--r-- 1 kali kali 13 Dec  1 10:30 file3.txt
```

**2.4 Remove all permissions for others:**
```bash
chmod o= file4.txt
# OR
chmod o-rwx file4.txt
ls -l file4.txt
# -rw-r----- 1 kali kali 13 Dec  1 10:30 file4.txt
```

**2.5 Create secret.txt with specific permissions:**
```bash
touch secret.txt
chmod u=rwx,g=r,o= secret.txt
ls -l secret.txt
# -rwxr----- 1 kali kali 0 Dec  1 10:30 secret.txt
```

</details>

---

## Exercise 3: Numeric Permissions

### Objective
Master numeric (octal) permission notation.

### Tasks

**3.1 Set permissions using numeric mode**

Using the files from Exercise 2 (or create new ones):

```bash
cd ~/permissions_practice

# Set permissions to 755 on file1.txt
chmod 755 file1.txt

# Verify and explain what 755 means
ls -l file1.txt
```

**3.2 Apply these numeric permissions:**

| File | Numeric | What should it mean? |
|------|---------|---------------------|
| file1.txt | 755 | |
| file2.txt | 644 | |
| file3.txt | 700 | |
| file4.txt | 640 | |

Apply each permission and verify with `ls -l`.

**3.3 Calculate numeric permissions**

Convert these symbolic permissions to numeric:

1. `rwxr-xr-x` = ?
2. `rw-r--r--` = ?
3. `rwx------` = ?
4. `rw-rw-r--` = ?
5. `r-x--x---` = ?

**3.4 Convert numeric to symbolic**

What symbolic permission do these represent?

1. 777 = ?
2. 600 = ?
3. 751 = ?
4. 444 = ?
5. 111 = ?

<details>
<summary>💡 Hints</summary>

**Remember the values:**
- r = 4
- w = 2
- x = 1
- no permission = 0

**Add them up for each category:**
- rwx = 4+2+1 = 7
- rw- = 4+2+0 = 6
- r-x = 4+0+1 = 5
- r-- = 4+0+0 = 4

</details>

<details>
<summary>✅ Solutions</summary>

**3.2 Numeric permissions applied:**

```bash
chmod 755 file1.txt  # rwxr-xr-x - Owner: full, Group/Others: read+execute
chmod 644 file2.txt  # rw-r--r-- - Owner: read/write, Group/Others: read
chmod 700 file3.txt  # rwx------ - Owner: full, nobody else
chmod 640 file4.txt  # rw-r----- - Owner: read/write, Group: read, Others: none

ls -l
# -rwxr-xr-x 1 kali kali 13 Dec  1 10:30 file1.txt
# -rw-r--r-- 1 kali kali 13 Dec  1 10:30 file2.txt
# -rwx------ 1 kali kali 13 Dec  1 10:30 file3.txt
# -rw-r----- 1 kali kali 13 Dec  1 10:30 file4.txt
```

**3.3 Symbolic to Numeric:**

1. `rwxr-xr-x` = 755 (7|5|5)
2. `rw-r--r--` = 644 (6|4|4)
3. `rwx------` = 700 (7|0|0)
4. `rw-rw-r--` = 664 (6|6|4)
5. `r-x--x---` = 510 (5|1|0)

**3.4 Numeric to Symbolic:**

1. 777 = `rwxrwxrwx` (everyone can do everything - DANGEROUS!)
2. 600 = `rw-------` (owner read/write only)
3. 751 = `rwxr-x--x` (owner: full, group: read/execute, others: execute)
4. 444 = `r--r--r--` (everyone can only read)
5. 111 = `--x--x--x` (everyone can only execute)

</details>

---

## Exercise 4: Make a Script Executable

### Objective
Understand why scripts need execute permission and how to set it.

### Tasks

**4.1 Create a simple bash script**

```bash
cd ~/permissions_practice

# Create the script
cat > hello.sh << 'EOF'
#!/bin/bash
echo "Hello, $(whoami)!"
echo "Today is $(date)"
echo "You are in: $(pwd)"
EOF

# Check the file was created
cat hello.sh
```

**4.2 Try to run it (this will fail)**

```bash
./hello.sh
```

What error message do you see? Why?

**4.3 Check current permissions**

```bash
ls -l hello.sh
```

What permissions does it have? Does it have execute permission?

**4.4 Add execute permission**

```bash
# Method 1: Symbolic
chmod u+x hello.sh

# OR Method 2: Numeric (for rwxr-xr-x)
chmod 755 hello.sh
```

**4.5 Run the script**

```bash
./hello.sh
```

**4.6 Try alternative ways to run scripts**

Even without execute permission, you can run scripts using the interpreter directly:

```bash
# Remove execute permission
chmod -x hello.sh

# This will fail
./hello.sh

# But this works (calling bash directly)
bash hello.sh
```

⚠️ **Why does `bash hello.sh` work without execute permission?**

<details>
<summary>💡 Hints</summary>

- When you run `./script.sh`, you're asking the system to execute the file
- When you run `bash script.sh`, you're running bash and telling it to read the file
- The file only needs to be readable for bash to read it
- Execute permission is only needed for direct execution (`./`)

</details>

<details>
<summary>✅ Solutions</summary>

**4.2 Expected error:**
```bash
$ ./hello.sh
bash: ./hello.sh: Permission denied
```

The error occurs because the file doesn't have execute permission.

**4.3 Initial permissions:**
```bash
$ ls -l hello.sh
-rw-r--r-- 1 kali kali 78 Dec  1 10:30 hello.sh
```

No `x` in the permission string = no execute permission.

**4.5 After adding execute permission:**
```bash
$ chmod u+x hello.sh
$ ls -l hello.sh
-rwxr--r-- 1 kali kali 78 Dec  1 10:30 hello.sh

$ ./hello.sh
Hello, kali!
Today is Mon Dec  1 10:30:00 UTC 2024
You are in: /home/kali/permissions_practice
```

**4.6 Why `bash hello.sh` works:**

When you run `bash hello.sh`:
1. You're executing `/bin/bash` (which you have permission to run)
2. Bash reads `hello.sh` as input (only needs read permission)
3. Bash interprets the commands in the file

When you run `./hello.sh`:
1. The kernel tries to execute the file directly
2. The kernel checks if you have execute permission
3. Only then does it read the shebang (`#!/bin/bash`) and run bash

</details>

---

## Exercise 5: Change Ownership

### Objective
Practice changing file ownership with `chown` and `chgrp`.

### Setup

```bash
cd ~/permissions_practice

# Create test files
touch owned_by_me.txt
echo "This is my file" > owned_by_me.txt

# Check current ownership
ls -l owned_by_me.txt
```

### Tasks

**5.1 View current ownership**

```bash
ls -l owned_by_me.txt
```

Who is the owner? What is the group?

**5.2 View your user and groups**

```bash
# Your username
whoami

# Groups you belong to
groups

# Your user and group IDs
id
```

**5.3 Try to change ownership (this will fail without sudo)**

```bash
# Try without sudo
chown root owned_by_me.txt
```

What error do you get? Why?

**5.4 Change ownership using sudo**

```bash
# Change owner to root
sudo chown root owned_by_me.txt
ls -l owned_by_me.txt

# Change owner back to yourself
sudo chown $USER owned_by_me.txt
ls -l owned_by_me.txt
```

**5.5 Change group ownership**

```bash
# Create a test file
touch grouptest.txt

# Change group only (using chgrp)
sudo chgrp root grouptest.txt
ls -l grouptest.txt

# Change both owner and group (using chown)
sudo chown kali:kali grouptest.txt
ls -l grouptest.txt
```

**5.6 Recursive ownership change**

```bash
# Create a directory structure
mkdir -p project/{src,docs,config}
touch project/src/main.py
touch project/docs/readme.md
touch project/config/settings.ini

# Change ownership recursively
sudo chown -R root:root project/
ls -lR project/

# Change back
sudo chown -R $USER:$USER project/
ls -lR project/
```

<details>
<summary>💡 Hints</summary>

- Only root can change owner to another user
- You can change group to any group you belong to
- Use `chown user:group` to change both at once
- Use `chown :group` to change only the group
- The `-R` flag applies changes recursively

</details>

<details>
<summary>✅ Solutions</summary>

**5.1 Current ownership:**
```bash
$ ls -l owned_by_me.txt
-rw-r--r-- 1 kali kali 17 Dec  1 10:30 owned_by_me.txt
```
Owner: kali, Group: kali

**5.2 User and groups:**
```bash
$ whoami
kali

$ groups
kali sudo users

$ id
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo),100(users)
```

**5.3 Error without sudo:**
```bash
$ chown root owned_by_me.txt
chown: changing ownership of 'owned_by_me.txt': Operation not permitted
```

Only root can change file ownership to another user.

**5.4-5.6 Commands work as shown in the tasks.**

</details>

---

## Exercise 6: Secure a Configuration Directory

### Objective
Apply permissions and ownership to protect sensitive configuration files.

### Scenario

You need to create a secure configuration directory that:
- Only your user can access
- Contains sensitive configuration files
- Others cannot read, write, or enter the directory

### Tasks

**6.1 Create the configuration structure**

```bash
cd ~
mkdir -p secure_config
cd secure_config

# Create mock configuration files
cat > database.conf << 'EOF'
[database]
host=localhost
port=5432
username=admin
password=SuperSecret123!
EOF

cat > api_keys.conf << 'EOF'
[api]
google_api_key=AIza1234567890abcdef
aws_access_key=AKIA1234567890EXAMPLE
aws_secret_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
EOF

cat > app.conf << 'EOF'
[app]
debug=false
log_level=info
secret_key=my-super-secret-key-123
EOF
```

**6.2 Check current permissions**

```bash
ls -la ~/secure_config/
```

Are these files secure? Can others read them?

**6.3 Secure the files (owner read/write only)**

```bash
# Set permissions on all config files
chmod 600 *.conf

# Verify
ls -l
```

**6.4 Secure the directory**

```bash
# Go up one level
cd ~

# Set directory permissions (owner only)
chmod 700 secure_config

# Verify
ls -ld secure_config
```

**6.5 Test the security**

If you have access to another user or can use sudo:

```bash
# Try to access as root (should work)
sudo ls ~/secure_config/
sudo cat ~/secure_config/database.conf

# The key point: if 'others' tried to access, they would get 'Permission denied'
```

**6.6 Verify the final state**

```bash
ls -la ~/secure_config/
```

Expected output should show:
- Directory: `drwx------` (700)
- Files: `-rw-------` (600)

<details>
<summary>💡 Hints</summary>

- 600 = rw------- (owner read/write only)
- 700 = rwx------ (owner full access only)
- For directories, you need execute (x) to enter them
- Always check both directory AND file permissions

</details>

<details>
<summary>✅ Solutions</summary>

**6.2 Initial permissions (insecure):**
```bash
$ ls -la ~/secure_config/
total 20
drwxr-xr-x  2 kali kali 4096 Dec  1 10:30 .
drwxr-xr-x 20 kali kali 4096 Dec  1 10:30 ..
-rw-r--r--  1 kali kali   95 Dec  1 10:30 api_keys.conf
-rw-r--r--  1 kali kali   67 Dec  1 10:30 app.conf
-rw-r--r--  1 kali kali   89 Dec  1 10:30 database.conf
```

⚠️ These files are readable by everyone! That's a security risk.

**6.6 Final secure state:**
```bash
$ ls -la ~/secure_config/
total 20
drwx------  2 kali kali 4096 Dec  1 10:30 .
drwxr-xr-x 20 kali kali 4096 Dec  1 10:30 ..
-rw-------  1 kali kali   95 Dec  1 10:30 api_keys.conf
-rw-------  1 kali kali   67 Dec  1 10:30 app.conf
-rw-------  1 kali kali   89 Dec  1 10:30 database.conf
```

Now only the owner can access the directory and read the files.

</details>

---

## Challenge Exercise: Set Up a Shared Project Folder

### Objective
Create a shared directory where multiple team members can collaborate.

### Scenario

You're setting up a shared project folder for a development team:
- Team members are in the `devteam` group
- All team members need read and write access
- Files should not be accessible to non-team members
- New files should automatically belong to the group

### Tasks

**7.1 Create the group (requires sudo)**

```bash
# Create the devteam group
sudo groupadd devteam

# Add your user to the group
sudo usermod -aG devteam $USER

# Verify (you may need to log out and back in to see the group)
groups
```

💡 **Note**: You might need to log out and log back in for group membership to take effect, or use `newgrp devteam`.

**7.2 Create the shared project directory**

```bash
# Create the directory
sudo mkdir -p /projects/shared_app

# Verify creation
ls -ld /projects/shared_app
```

**7.3 Set up ownership**

```bash
# Set group ownership to devteam
sudo chown :devteam /projects/shared_app

# Verify
ls -ld /projects/shared_app
```

**7.4 Set permissions**

```bash
# Set permissions: owner and group have full access, others have none
sudo chmod 770 /projects/shared_app

# Verify
ls -ld /projects/shared_app
```

**7.5 Enable SGID for group inheritance**

```bash
# Set SGID so new files inherit the group
sudo chmod g+s /projects/shared_app

# Verify (notice the 's' in group permissions)
ls -ld /projects/shared_app
```

Expected: `drwxrws---`

**7.6 Test the setup**

```bash
# Navigate to the shared directory
cd /projects/shared_app

# Create a test file
touch my_contribution.txt

# Check its permissions and ownership
ls -l my_contribution.txt
```

The file should belong to the `devteam` group (thanks to SGID).

**7.7 Create a complete project structure**

```bash
cd /projects/shared_app

# Create project directories
mkdir -p src tests docs

# Create some files
echo "# Shared App" > README.md
echo "print('Hello')" > src/main.py
echo "# Tests" > tests/test_main.py

# Check all permissions
ls -laR
```

**7.8 Verify the complete setup**

Answer these questions:
1. What are the permissions on `/projects/shared_app`?
2. What group owns the directory?
3. Will new files automatically belong to `devteam`?
4. Can users outside `devteam` access the files?

<details>
<summary>💡 Hints</summary>

- SGID (set group ID) makes new files inherit the directory's group
- 770 means owner and group have rwx, others have nothing
- The 's' in `drwxrws---` indicates SGID is set
- Use `newgrp devteam` to activate group membership without logging out

</details>

<details>
<summary>✅ Solutions</summary>

**7.6 Test file ownership:**
```bash
$ ls -l my_contribution.txt
-rw-r--r-- 1 kali devteam 0 Dec  1 10:30 my_contribution.txt
```

Notice the group is `devteam`, not your personal group!

**7.7 Complete project structure:**
```bash
$ ls -laR
/projects/shared_app:
total 16
drwxrws---  5 root devteam 4096 Dec  1 10:30 .
drwxr-xr-x  3 root root    4096 Dec  1 10:30 ..
-rw-r--r--  1 kali devteam   14 Dec  1 10:30 README.md
drwxr-sr-x  2 kali devteam 4096 Dec  1 10:30 docs
drwxr-sr-x  2 kali devteam 4096 Dec  1 10:30 src
drwxr-sr-x  2 kali devteam 4096 Dec  1 10:30 tests

/projects/shared_app/src:
total 4
-rw-r--r-- 1 kali devteam 16 Dec  1 10:30 main.py

/projects/shared_app/tests:
total 4
-rw-r--r-- 1 kali devteam 8 Dec  1 10:30 test_main.py
```

**7.8 Answers:**

1. Permissions: `drwxrws---` (770 with SGID)
2. Group owner: `devteam`
3. Yes, SGID ensures new files belong to `devteam`
4. No, others have no permissions (the `---` at the end)

</details>

---

## Bonus Challenges

### Challenge A: Audit File Permissions

Write a command to find all world-writable files in your home directory:

```bash
find ~ -type f -perm -002 2>/dev/null
```

⚠️ World-writable files are a security risk!

### Challenge B: Fix Insecure SSH Keys

SSH private keys must have strict permissions (600). Practice:

```bash
# Create a fake SSH directory
mkdir -p ~/fake_ssh
touch ~/fake_ssh/id_rsa
touch ~/fake_ssh/id_rsa.pub

# Set correct permissions
chmod 700 ~/fake_ssh
chmod 600 ~/fake_ssh/id_rsa
chmod 644 ~/fake_ssh/id_rsa.pub

# Verify
ls -la ~/fake_ssh/
```

### Challenge C: Create a Drop Box Directory

Create a directory where:
- Anyone can add files (write)
- Only the owner can see what's inside (no read for others)
- Only file owners can delete their files (sticky bit)

```bash
mkdir ~/dropbox
chmod 733 ~/dropbox  # -wx for group and others
chmod +t ~/dropbox   # sticky bit

ls -ld ~/dropbox
# drwx-wx-wt
```

---

## 📝 Summary

After completing these exercises, you should be able to:

- ✅ Read and interpret permission strings from `ls -l`
- ✅ Use symbolic mode: `chmod u+x`, `chmod g-w`, `chmod o=r`
- ✅ Use numeric mode: `chmod 755`, `chmod 644`, `chmod 600`
- ✅ Convert between symbolic and numeric permissions
- ✅ Make scripts executable
- ✅ Change file ownership with `chown` and `chgrp`
- ✅ Set up secure private directories
- ✅ Create shared directories with proper group permissions
- ✅ Understand and apply SGID for group inheritance

## Cleanup

To clean up after these exercises:

```bash
rm -rf ~/permissions_practice
rm -rf ~/secure_config
rm -rf ~/fake_ssh
rm -rf ~/dropbox
sudo rm -rf /projects/shared_app
sudo groupdel devteam 2>/dev/null