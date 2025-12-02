# Module 7 Exercises: Package Management with APT

## 🎯 Exercise Goals

These exercises will help you:
- Update and upgrade your system safely
- Search for and explore packages
- Install and verify software installations
- Query package information using dpkg
- Remove software properly
- Handle dependencies
- Set up a development environment

---

## Exercise 1: Update Your System

### Objective
Learn to update package lists and upgrade installed packages.

### Tasks

**1.1 Update package lists**

```bash
# Update the package lists from repositories
sudo apt update
```

**Questions to answer:**
- How many packages can be upgraded?
- Did you see any "Hit", "Get", or "Ign" entries?
- How long did the update take?

**1.2 Check for upgradable packages**

```bash
# List packages that can be upgraded
apt list --upgradable
```

Write down 3 packages that have updates available (if any).

**1.3 Understand the upgrade preview**

```bash
# Preview what upgrade will do (don't confirm yet)
sudo apt upgrade
```

When prompted, look at:
- How many packages will be upgraded?
- How many new packages will be installed?
- How many packages will be removed?
- How much disk space will be used?

Type `n` to cancel for now.

**1.4 Perform the upgrade**

```bash
# Now actually upgrade (use full-upgrade for Kali)
sudo apt full-upgrade
```

Review the changes and type `y` to confirm.

**1.5 Verify the upgrade**

```bash
# Check that there are no more upgradable packages
apt list --upgradable

# Should show: "Listing... Done" with no packages
```

<details>
<summary>💡 Hints</summary>

- `apt update` refreshes the list of available packages
- `apt list --upgradable` shows what can be upgraded
- `apt upgrade` upgrades packages (safe, won't remove packages)
- `apt full-upgrade` upgrades and handles dependency changes
- On Kali (rolling release), use `full-upgrade` regularly

</details>

<details>
<summary>✅ Expected Results</summary>

**1.1 Update output:**
```bash
$ sudo apt update
Hit:1 http://http.kali.org/kali kali-rolling InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
X packages can be upgraded. Run 'apt list --upgradable' to see them.
```

**1.2 Upgradable packages:**
```bash
$ apt list --upgradable
Listing... Done
curl/kali-rolling 7.88.1-1 amd64 [upgradable from: 7.87.0-2]
vim/kali-rolling 2:9.0.1000-1 amd64 [upgradable from: 2:9.0.0900-1]
...
```

**1.5 After upgrade:**
```bash
$ apt list --upgradable
Listing... Done
# No packages listed means everything is up to date
```

</details>

---

## Exercise 2: Search and Explore

### Objective
Learn to find and examine packages before installing.

### Tasks

**2.1 Search for text editor packages**

```bash
# Search for packages related to "text editor"
apt search "text editor"
```

How many results did you get? Name 3 text editors you found.

**2.2 Search for a specific tool**

```bash
# Search for htop
apt search htop
```

**2.3 Get detailed package information**

```bash
# View details about htop
apt show htop
```

Fill in this information about htop:

| Property | Value |
|----------|-------|
| Version | |
| Installed Size | |
| Homepage | |
| Number of dependencies | |

**2.4 List all installed packages**

```bash
# List installed packages
apt list --installed

# Count how many packages are installed
apt list --installed | wc -l
```

How many packages are installed on your system?

**2.5 Search for packages containing "network"**

```bash
# Search for network-related packages
apt search network

# More specific: network tools
apt search "network scanner"
apt search "network analyzer"
```

Name 2 network-related packages you found.

**2.6 Check if a specific package is installed**

```bash
# Check if vim is installed
apt list --installed vim

# Check if nmap is installed
apt list --installed nmap

# Alternative method using dpkg
dpkg -l vim
dpkg -l nmap
```

<details>
<summary>💡 Hints</summary>

- `apt search` searches package names AND descriptions
- `apt show` gives detailed information about a package
- `apt list --installed` shows only installed packages
- Use `grep` to filter results: `apt search network | grep -i scanner`
- Package count includes all packages (including libraries)

</details>

<details>
<summary>✅ Expected Results</summary>

**2.2 htop search:**
```bash
$ apt search htop
Sorting... Done
Full Text Search... Done
htop/kali-rolling 3.2.1-1 amd64
  interactive processes viewer

aha/kali-rolling 0.5.1-2 amd64
  ANSI color to HTML converter
```

**2.3 htop details:**
```bash
$ apt show htop
Package: htop
Version: 3.2.1-1
Installed-Size: 372 kB
Depends: libc6 (>= 2.29), libncursesw6 (>= 6), libtinfo6 (>= 6)
Homepage: https://htop.dev/
Description: interactive processes viewer
 Htop is an ncursed-based process viewer similar to top...
```

| Property | Value |
|----------|-------|
| Version | 3.2.1-1 |
| Installed Size | 372 kB |
| Homepage | https://htop.dev/ |
| Number of dependencies | 3 (libc6, libncursesw6, libtinfo6) |

**2.4 Package count:**
```bash
$ apt list --installed | wc -l
# Typically 1000-3000 packages on Kali
```

</details>

---

## Exercise 3: Install Software

### Objective
Practice installing packages and verifying installations.

### Tasks

**3.1 Install htop (system monitor)**

```bash
# First, update package lists
sudo apt update

# Install htop
sudo apt install htop
```

Review what will be installed, then confirm.

**3.2 Verify the installation**

```bash
# Check if htop is now listed as installed
apt list --installed htop

# Check the version
htop --version

# Run htop to test it
htop
# Press 'q' to quit htop
```

**3.3 Install tree (directory visualization)**

```bash
# Install tree
sudo apt install tree

# Verify installation
tree --version

# Test it
tree -L 2 /home
```

**3.4 Install multiple packages at once**

```bash
# Install multiple packages in one command
sudo apt install cowsay figlet

# Test them
cowsay "Hello Linux!"
figlet "APT"
```

**3.5 View what was installed**

```bash
# See what files cowsay installed
dpkg -L cowsay | head -20

# See what files figlet installed
dpkg -L figlet | head -20
```

**3.6 Try installing an already-installed package**

```bash
# What happens if you try to install something already installed?
sudo apt install htop
```

Note what APT tells you.

<details>
<summary>💡 Hints</summary>

- Always run `apt update` before installing
- The installation output shows dependencies being installed
- Use `--version` or `-v` to check if a program is installed
- `dpkg -L` shows all files installed by a package
- Installing an already-installed package does nothing harmful

</details>

<details>
<summary>✅ Expected Results</summary>

**3.1 htop installation:**
```bash
$ sudo apt install htop
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  htop
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 144 kB of archives.
After this operation, 372 kB of additional disk space will be used.
Get:1 http://http.kali.org/kali kali-rolling/main amd64 htop amd64 3.2.1-1 [144 kB]
Fetched 144 kB in 0s (1,234 kB/s)
Selecting previously unselected package htop.
(Reading database ... 123456 files and directories currently installed.)
Preparing to unpack .../htop_3.2.1-1_amd64.deb ...
Unpacking htop (3.2.1-1) ...
Setting up htop (3.2.1-1) ...
Processing triggers for man-db (2.9.4-2) ...
```

**3.2 Verification:**
```bash
$ apt list --installed htop
Listing... Done
htop/kali-rolling,now 3.2.1-1 amd64 [installed]

$ htop --version
htop 3.2.1
```

**3.4 Fun output:**
```bash
$ cowsay "Hello Linux!"
 ______________
< Hello Linux! >
 --------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

$ figlet "APT"
     _    ____ _____ 
    / \  |  _ \_   _|
   / _ \ | |_) || |  
  / ___ \|  __/ | |  
 /_/   \_\_|    |_|  
```

**3.6 Already installed:**
```bash
$ sudo apt install htop
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
htop is already the newest version (3.2.1-1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

</details>

---

## Exercise 4: Package Information

### Objective
Learn to query information about installed packages using dpkg.

### Tasks

**4.1 Find which package provides `/bin/ls`**

```bash
# What package installed /bin/ls?
dpkg -S /bin/ls
```

What package owns `/bin/ls`?

**4.2 Find which package provides other files**

```bash
# What package provides /bin/grep?
dpkg -S /bin/grep

# What package provides /usr/bin/vim?
dpkg -S /usr/bin/vim

# What package provides /usr/bin/python3?
dpkg -S /usr/bin/python3
```

**4.3 List all files installed by a package**

```bash
# What files did htop install?
dpkg -L htop

# What files did tree install?
dpkg -L tree

# How many files did cowsay install?
dpkg -L cowsay | wc -l
```

**4.4 Check the version of installed packages**

```bash
# Check htop version via dpkg
dpkg -l htop

# Check curl version
dpkg -l curl

# More detailed with apt
apt show htop | grep Version
apt show curl | grep Version
```

**4.5 Find package dependencies**

```bash
# What does htop depend on?
apt show htop | grep Depends

# What does nmap depend on?
apt show nmap | grep -E "Depends|Recommends"
```

**4.6 List installed packages matching a pattern**

```bash
# List all packages with "python" in the name
dpkg -l | grep python

# List all packages starting with "lib"
dpkg -l "lib*" | head -20

# Count packages with "python" in the name
dpkg -l | grep python | wc -l
```

<details>
<summary>💡 Hints</summary>

- `dpkg -S /path/to/file` finds which package owns a file
- `dpkg -L package` lists files installed by a package
- `dpkg -l` lists installed packages with status
- `apt show` gives more human-readable information
- Pipe to `grep` to find specific information

</details>

<details>
<summary>✅ Expected Results</summary>

**4.1 Owner of /bin/ls:**
```bash
$ dpkg -S /bin/ls
coreutils: /bin/ls
```
The `coreutils` package provides `/bin/ls`.

**4.2 Other file owners:**
```bash
$ dpkg -S /bin/grep
grep: /bin/grep

$ dpkg -S /usr/bin/vim
vim: /usr/bin/vim
# Or vim-tiny, vim-gtk3, depending on which is installed
```

**4.3 Files installed by htop:**
```bash
$ dpkg -L htop
/.
/usr
/usr/bin
/usr/bin/htop
/usr/share
/usr/share/doc
/usr/share/doc/htop
/usr/share/doc/htop/AUTHORS
/usr/share/doc/htop/changelog.gz
/usr/share/doc/htop/copyright
/usr/share/man
/usr/share/man/man1
/usr/share/man/man1/htop.1.gz
...
```

**4.4 Version check:**
```bash
$ dpkg -l htop
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name           Version      Architecture Description
+++-==============-============-============-================================
ii  htop           3.2.1-1      amd64        interactive processes viewer
```

**4.5 Dependencies:**
```bash
$ apt show htop | grep Depends
Depends: libc6 (>= 2.29), libncursesw6 (>= 6), libtinfo6 (>= 6)
```

</details>

---

## Exercise 5: Remove Software

### Objective
Practice removing packages and cleaning up the system.

### Tasks

**5.1 Check disk space before cleanup**

```bash
# Check available disk space
df -h /

# Check the package cache size
du -sh /var/cache/apt/archives/
```

Record these values.

**5.2 Remove a test package (keep config)**

```bash
# Remove cowsay (keeps configuration files)
sudo apt remove cowsay

# Verify it's removed
which cowsay
cowsay "test"
```

**5.3 Remove a package completely (purge)**

```bash
# Purge figlet (removes program AND config files)
sudo apt purge figlet

# Verify it's removed
which figlet
figlet "test"
```

**5.4 Clean up orphaned dependencies**

```bash
# See what autoremove will do
sudo apt autoremove

# Review and confirm
```

How many packages were removed by autoremove?

**5.5 Clean the package cache**

```bash
# Check cache size before
du -sh /var/cache/apt/archives/

# Clean the cache
sudo apt clean

# Check cache size after
du -sh /var/cache/apt/archives/
```

How much space did you save?

**5.6 Alternative: autoclean**

```bash
# autoclean only removes old versions (if you had something cached)
sudo apt autoclean
```

**5.7 Check disk space after cleanup**

```bash
# Compare with your initial values
df -h /
```

<details>
<summary>💡 Hints</summary>

- `remove` keeps configuration files (in case you reinstall)
- `purge` removes everything including configuration
- `autoremove` removes packages that were installed as dependencies but are no longer needed
- `clean` clears the entire package cache
- `autoclean` only removes old/outdated cached packages

</details>

<details>
<summary>✅ Expected Results</summary>

**5.1 Initial disk space:**
```bash
$ df -h /
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   15G   33G  31% /

$ du -sh /var/cache/apt/archives/
256M    /var/cache/apt/archives/
```

**5.2 Remove cowsay:**
```bash
$ sudo apt remove cowsay
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be REMOVED:
  cowsay
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 200 kB disk space will be freed.
Do you want to continue? [Y/n] y
(Reading database ... 123456 files and directories currently installed.)
Removing cowsay (3.03+dfsg2-8) ...

$ which cowsay
# No output - cowsay is gone

$ cowsay "test"
bash: cowsay: command not found
```

**5.5 Cache cleanup:**
```bash
$ du -sh /var/cache/apt/archives/
256M    /var/cache/apt/archives/

$ sudo apt clean

$ du -sh /var/cache/apt/archives/
4.0K    /var/cache/apt/archives/
# Saved ~256MB!
```

</details>

---

## Exercise 6: Handle Dependencies

### Objective
Understand how APT manages package dependencies.

### Tasks

**6.1 Explore a package with many dependencies**

```bash
# Look at what nmap needs
apt show nmap | grep -E "Depends|Recommends|Suggests"

# See full dependency tree (if apt-rdepends is installed)
# First install it:
sudo apt install apt-rdepends

# Then view nmap's dependencies
apt-rdepends nmap | head -30
```

**6.2 Install a package and observe dependencies**

```bash
# Install wireshark (has many dependencies)
sudo apt install wireshark

# Watch how many packages are installed
# Note: Wireshark may ask about non-root capture - select Yes or No based on preference
```

How many additional packages were installed as dependencies?

**6.3 Understand the installation plan**

When APT shows you what will be installed:

```
The following additional packages will be installed:
  libcap2-bin libpcap0.8 libwireshark-data libwireshark14 ...
Suggested packages:
  wireshark-doc
The following NEW packages will be installed:
  libcap2-bin libpcap0.8 wireshark ...
```

Identify:
- Which packages are direct dependencies
- Which packages are suggested (optional)
- Total disk space required

**6.4 See reverse dependencies**

```bash
# What packages depend on libpcap0.8?
apt-cache rdepends libpcap0.8 | head -20

# What depends on curl?
apt-cache rdepends curl | head -20
```

**6.5 Simulate an installation (dry run)**

```bash
# See what would be installed without actually installing
sudo apt install --dry-run apache2

# Or use -s flag
sudo apt install -s nginx
```

This is useful for planning without making changes.

**6.6 Clean up the test installation**

```bash
# Remove wireshark and its dependencies
sudo apt remove wireshark
sudo apt autoremove

# Remove apt-rdepends if you don't need it
sudo apt remove apt-rdepends
sudo apt autoremove
```

<details>
<summary>💡 Hints</summary>

- Dependencies are packages required for the software to work
- Recommends are packages that enhance functionality
- Suggests are optional packages
- `apt-cache rdepends` shows what depends ON a package
- `--dry-run` or `-s` simulates the operation
- Always run `autoremove` after removing packages with dependencies

</details>

<details>
<summary>✅ Expected Results</summary>

**6.1 nmap dependencies:**
```bash
$ apt show nmap | grep Depends
Depends: libc6 (>= 2.14), libgcc-s1 (>= 3.0), liblinear4 (>= 2.01+dfsg), liblua5.3-0, libpcap0.8 (>= 1.0.0), libpcre3, libssl1.1 (>= 1.1.0), libstdc++6 (>= 9), nmap-common (= 7.91+dfsg1-2)
```

**6.2 Wireshark installation:**
```bash
$ sudo apt install wireshark
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libmaxminddb0 libpcap0.8 libwireshark-data libwireshark14 ...
The following NEW packages will be installed:
  libmaxminddb0 libpcap0.8 wireshark wireshark-common ...
0 upgraded, 25 newly installed, 0 to remove and 0 not upgraded.
# 25+ packages installed as dependencies!
```

**6.5 Dry run:**
```bash
$ sudo apt install --dry-run nginx
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  nginx-common nginx-full
Suggested packages:
  fcgiwrap nginx-doc
The following NEW packages will be installed:
  nginx nginx-common nginx-full
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Inst nginx-common (1.18.0-6.1 Debian:11.0/stable [all])
Inst nginx-full (1.18.0-6.1 Debian:11.0/stable [amd64])
Inst nginx (1.18.0-6.1 Debian:11.0/stable [all])
Conf nginx-common (1.18.0-6.1 Debian:11.0/stable [all])
Conf nginx-full (1.18.0-6.1 Debian:11.0/stable [amd64])
Conf nginx (1.18.0-6.1 Debian:11.0/stable [all])
# Note: Nothing is actually installed in dry-run mode
```

</details>

---

## Challenge Exercise: Set Up a Development Environment

### Objective
Apply everything you've learned to set up a complete development environment.

### Scenario

You're setting up a fresh Kali Linux system for development and security testing. You need to install essential development tools and document the process.

### Tasks

**Step 1: Update the system**

```bash
# Update package lists
sudo apt update

# Upgrade all packages
sudo apt full-upgrade

# Verify no packages are waiting for upgrade
apt list --upgradable
```

**Step 2: Install essential development tools**

```bash
# Install the build-essential package (includes gcc, g++, make, etc.)
sudo apt install build-essential

# Install version control
sudo apt install git

# Install text editors
sudo apt install vim nano

# Install network tools
sudo apt install curl wget

# Install all at once (alternative):
# sudo apt install build-essential git vim nano curl wget
```

**Step 3: Verify all installations**

```bash
# Verify each tool is installed
gcc --version
g++ --version
make --version
git --version
vim --version | head -1
nano --version
curl --version | head -1
wget --version | head -1
```

Create a checklist:
| Tool | Installed? | Version |
|------|------------|---------|
| gcc | | |
| g++ | | |
| make | | |
| git | | |
| vim | | |
| nano | | |
| curl | | |
| wget | | |

**Step 4: Explore build-essential**

```bash
# What packages does build-essential include?
apt show build-essential

# What dependencies does it pull in?
apt-cache depends build-essential
```

List 5 packages that are part of (or required by) build-essential.

**Step 5: Document installed packages**

```bash
# Save a list of manually installed packages
apt-mark showmanual > ~/installed-packages.txt

# View the list
cat ~/installed-packages.txt

# Count how many
wc -l ~/installed-packages.txt
```

**Step 6: Clean up**

```bash
# Clean up package cache
sudo apt clean

# Check for orphaned packages
sudo apt autoremove

# Final disk space check
df -h /
```

**Step 7: Create a setup script**

Create a file called `setup-dev.sh`:

```bash
cat > ~/setup-dev.sh << 'EOF'
#!/bin/bash
# Development Environment Setup Script

echo "=== Development Environment Setup ==="
echo "Starting at: $(date)"
echo ""

# Update system
echo "[1/4] Updating system..."
sudo apt update
sudo apt full-upgrade -y

# Install development tools
echo "[2/4] Installing development tools..."
sudo apt install -y build-essential git vim nano curl wget

# Verify installations
echo "[3/4] Verifying installations..."
echo "gcc: $(gcc --version | head -1)"
echo "git: $(git --version)"
echo "vim: $(vim --version | head -1 | cut -d' ' -f1-5)"
echo "curl: $(curl --version | head -1 | cut -d' ' -f1-2)"

# Cleanup
echo "[4/4] Cleaning up..."
sudo apt autoremove -y
sudo apt clean

echo ""
echo "=== Setup Complete ==="
echo "Finished at: $(date)"
EOF

# Make it executable
chmod +x ~/setup-dev.sh

# View the script
cat ~/setup-dev.sh
```

**Step 8: Test your script (optional)**

```bash
# The script is already set up, but you could run it on a fresh system
# ./setup-dev.sh
```

### Deliverables

Create a summary file:

```bash
cat > ~/dev-environment-summary.txt << 'EOF'
Development Environment Summary
===============================
Date: [Your Date]
System: Kali Linux

Installed Packages:
- build-essential (gcc, g++, make, etc.)
- git
- vim
- nano  
- curl
- wget

Package Counts:
- Manually installed packages: [Number]
- Total installed packages: [Number]

Disk Space:
- Before setup: [Value]
- After setup: [Value]
- Cache cleaned: Yes

Notes:
- All packages installed from official Kali repositories
- System updated to latest versions
- Setup script created for future use
EOF

# Edit with actual values
nano ~/dev-environment-summary.txt
```

<details>
<summary>💡 Hints</summary>

- `build-essential` is a meta-package that installs common build tools
- Use `-y` flag in scripts to auto-confirm (but review first!)
- `apt-mark showmanual` lists packages you explicitly installed
- Creating setup scripts makes it easy to replicate environments
- Always document what you installed and why

</details>

<details>
<summary>✅ Expected Results</summary>

**Step 3: Version checklist:**
```bash
$ gcc --version
gcc (Debian 11.2.0-10) 11.2.0

$ git --version
git version 2.34.1

$ vim --version | head -1
VIM - Vi IMproved 8.2

$ curl --version | head -1
curl 7.79.1 (x86_64-pc-linux-gnu)
```

**Step 4: build-essential contents:**
```bash
$ apt-cache depends build-essential
build-essential
  Depends: libc6-dev
  Depends: gcc
  Depends: g++
  Depends: make
  Depends: dpkg-dev
```

**Step 5: Package list:**
```bash
$ wc -l ~/installed-packages.txt
150 /home/kali/installed-packages.txt
# Number varies based on your system
```

**Step 7: Script verification:**
```bash
$ cat ~/setup-dev.sh
#!/bin/bash
# Development Environment Setup Script
...
```

</details>

---

## Bonus Challenges

### Challenge A: Package Investigation

Investigate a package you're curious about:

```bash
# Choose a package (e.g., nmap, metasploit-framework, john)
PACKAGE="nmap"

echo "=== Package Investigation: $PACKAGE ==="
echo ""
echo "Description:"
apt show $PACKAGE | grep -A5 "Description"
echo ""
echo "Version available:"
apt show $PACKAGE | grep Version
echo ""
echo "Dependencies:"
apt show $PACKAGE | grep Depends
echo ""
echo "Installed size:"
apt show $PACKAGE | grep "Installed-Size"
echo ""
echo "Files installed (if installed):"
dpkg -L $PACKAGE 2>/dev/null | head -10 || echo "Not installed"
```

### Challenge B: Find and Install Alternatives

Find alternative packages for common tasks:

```bash
# Find text editors
apt search "text editor" | grep -E "^[a-z]" | head -10

# Find PDF viewers
apt search "pdf viewer" | grep -E "^[a-z]" | head -10

# Find image editors
apt search "image editor" | grep -E "^[a-z]" | head -10
```

Try installing one alternative and compare it to what you usually use.

### Challenge C: Create a Cleanup Script

Create a system cleanup script:

```bash
cat > ~/cleanup.sh << 'EOF'
#!/bin/bash
# System Cleanup Script

echo "=== System Cleanup ==="
echo ""

echo "Before cleanup:"
df -h / | grep -v Filesystem
echo "Cache: $(du -sh /var/cache/apt/archives/ 2>/dev/null | cut -f1)"
echo ""

echo "Removing orphaned packages..."
sudo apt autoremove -y

echo ""
echo "Cleaning package cache..."
sudo apt clean

echo ""
echo "After cleanup:"
df -h / | grep -v Filesystem
echo "Cache: $(du -sh /var/cache/apt/archives/ 2>/dev/null | cut -f1)"

echo ""
echo "Cleanup complete!"
EOF

chmod +x ~/cleanup.sh
```

### Challenge D: Compare Package Versions

Check if packages are at the latest version:

```bash
# Update and check for upgrades
sudo apt update

# Check specific packages
for pkg in git curl vim; do
    echo "=== $pkg ==="
    echo "Installed: $(dpkg -l $pkg 2>/dev/null | grep "^ii" | awk '{print $3}')"
    echo "Available: $(apt show $pkg 2>/dev/null | grep Version | head -1 | cut -d: -f2)"
    echo ""
done
```

---

## 📝 Summary

After completing these exercises, you should be able to:

- ✅ Update and upgrade your system safely
- ✅ Search for packages using `apt search` and `apt show`
- ✅ Install single and multiple packages
- ✅ Query package information with `dpkg -l`, `dpkg -L`, `dpkg -S`
- ✅ Remove packages with `remove`, `purge`, and `autoremove`
- ✅ Clean up disk space with `apt clean`
- ✅ Understand and manage dependencies
- ✅ Create setup and cleanup scripts
- ✅ Document your installed packages

## Cleanup

To remove all test packages installed during these exercises:

```bash
# Remove test packages
sudo apt remove htop tree cowsay figlet apt-rdepends 2>/dev/null
sudo apt autoremove

# Clean cache
sudo apt clean

# Remove created files
rm -f ~/setup-dev.sh ~/cleanup.sh ~/installed-packages.txt ~/dev-environment-summary.txt

echo "Cleanup complete!"
```

Note: If you want to keep `htop`, `tree`, or other useful tools, remove them from the cleanup command!