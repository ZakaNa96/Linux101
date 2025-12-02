# Module 7: Package Management with APT

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Understand what package managers are and why they're essential
- Use APT to search, install, update, and remove software
- Understand package repositories and sources
- Manage dependencies effectively
- Use dpkg for low-level package operations
- Troubleshoot common package management issues
- Follow best practices for keeping your system secure and up-to-date

---

## Prerequisites

Before starting this module, you should be comfortable with:
- ✅ Module 1: Basic terminal navigation
- ✅ Module 2: Navigating the filesystem
- ✅ Module 3: Working with files and directories
- ✅ Module 4: Viewing and editing text files
- ✅ Module 5: File permissions and ownership
- ✅ Module 6: User management and sudo

---

## 1. What is a Package Manager?

### The Problem: Installing Software Without a Package Manager

Imagine you want to install a program on your computer. Without a package manager, you would need to:

```
┌─────────────────────────────────────────────────────────────────┐
│           INSTALLING SOFTWARE THE HARD WAY                       │
├─────────────────────────────────────────────────────────────────┤
│  1. Find the software source code online                         │
│  2. Download it manually                                         │
│  3. Verify it's safe and authentic                               │
│  4. Find and install ALL dependencies (other required software)  │
│  5. Compile the source code                                      │
│  6. Configure the installation                                   │
│  7. Install to the correct locations                             │
│  8. Repeat for every dependency!                                 │
│  9. Manually track updates                                       │
│ 10. Manually remove files when uninstalling                      │
└─────────────────────────────────────────────────────────────────┘
```

This is tedious, error-prone, and potentially insecure!

### The Solution: Package Managers

A **package manager** automates all of this:

```
┌─────────────────────────────────────────────────────────────────┐
│           WHAT PACKAGE MANAGERS DO                               │
├─────────────────────────────────────────────────────────────────┤
│  📦 Download     - Fetch software from trusted sources           │
│  🔐 Verify       - Check authenticity with cryptographic keys    │
│  🔗 Dependencies - Automatically install required software       │
│  📂 Install      - Place files in correct locations              │
│  🔄 Update       - Keep everything up-to-date easily             │
│  🗑️  Remove      - Clean uninstall without leftover files        │
│  📋 Track        - Know exactly what's installed                 │
└─────────────────────────────────────────────────────────────────┘
```

### What is a Package?

A **package** is a bundled collection of files that make up a piece of software:

```
┌─────────────────────────────────────────────────────────────────┐
│                    ANATOMY OF A PACKAGE                          │
├─────────────────────────────────────────────────────────────────┤
│  📁 Program files      - The actual software binaries            │
│  📁 Configuration      - Default configuration files             │
│  📁 Documentation      - Man pages, help files                   │
│  📋 Metadata           - Name, version, description              │
│  📋 Dependencies       - List of required packages               │
│  📜 Scripts            - Pre/post install/remove scripts         │
└─────────────────────────────────────────────────────────────────┘
```

### Debian Packages (.deb files)

On Debian-based systems (Debian, Ubuntu, Kali Linux, Linux Mint), packages come in the `.deb` format:

```bash
# Example package name format:
package-name_version-revision_architecture.deb

# Real example:
htop_3.2.1-1_amd64.deb
│    │     │ │
│    │     │ └── Architecture (amd64 = 64-bit Intel/AMD)
│    │     └── Revision (Debian-specific changes)
│    └── Version number
└── Package name
```

💡 **Tip**: While you can manually download and install `.deb` files, using APT to install from repositories is much safer and easier.

### APT vs dpkg

There are two levels of package management on Debian systems:

```
┌─────────────────────────────────────────────────────────────────┐
│                    APT vs dpkg                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  dpkg (Debian Package)                                           │
│  ─────────────────────                                           │
│  • Low-level tool                                                │
│  • Installs individual .deb files                                │
│  • Does NOT handle dependencies                                  │
│  • Does NOT download packages                                    │
│  • Use: dpkg -i package.deb                                      │
│                                                                  │
│  APT (Advanced Package Tool)                                     │
│  ───────────────────────────                                     │
│  • High-level tool (uses dpkg underneath)                        │
│  • Downloads from repositories                                   │
│  • Automatically resolves dependencies                           │
│  • Handles updates and upgrades                                  │
│  • Use: apt install package                                      │
│                                                                  │
│  Relationship:                                                   │
│  ┌─────────────────────────────────┐                            │
│  │           APT                   │ ← You interact with this   │
│  │    ┌─────────────────────┐      │                            │
│  │    │        dpkg         │      │ ← APT uses this internally │
│  │    └─────────────────────┘      │                            │
│  └─────────────────────────────────┘                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

💡 **Tip**: For most tasks, use APT. Only use dpkg directly when you need to install a local `.deb` file or troubleshoot package issues.

---

## 2. Understanding APT

### What is APT?

**APT** (Advanced Package Tool) is a package management system used by Debian-based Linux distributions. It provides a high-level interface for managing software packages.

### apt vs apt-get vs aptitude

You might see different commands in tutorials:

| Command | Description | When to Use |
|---------|-------------|-------------|
| `apt` | Modern, user-friendly | ✅ **Use this** (recommended) |
| `apt-get` | Traditional, more options | Scripts, compatibility |
| `aptitude` | Interactive interface | Advanced users |

```bash
# These do the same thing:
apt install htop
apt-get install htop

# But apt is friendlier:
apt install htop      # Shows progress bar, colored output
apt-get install htop  # Plain output
```

💡 **Tip**: For everyday use, stick with `apt`. It's newer, simpler, and provides nicer output. The examples in this module use `apt`.

### How APT Works: Repositories

APT downloads packages from **repositories** - servers that host packages:

```
┌─────────────────────────────────────────────────────────────────┐
│                    HOW APT WORKS                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Your Computer                        Internet                  │
│   ┌────────────────┐                  ┌─────────────────────┐   │
│   │                │   apt update     │ Repository Server   │   │
│   │   Package      │ ───────────────► │ ┌─────────────────┐ │   │
│   │   Index        │ ◄─────────────── │ │ Package Index   │ │   │
│   │   (what's      │  Package list    │ │ (list of all    │ │   │
│   │    available)  │                  │ │  packages)      │ │   │
│   │                │                  │ └─────────────────┘ │   │
│   │                │   apt install    │ ┌─────────────────┐ │   │
│   │   Installed    │ ───────────────► │ │ Package Files   │ │   │
│   │   Packages     │ ◄─────────────── │ │ (.deb files)    │ │   │
│   │                │   .deb download  │ └─────────────────┘ │   │
│   └────────────────┘                  └─────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### The `/etc/apt/sources.list` File

The repositories APT uses are configured in `/etc/apt/sources.list` and files in `/etc/apt/sources.list.d/`:

```bash
# View your repository configuration
cat /etc/apt/sources.list

# On Kali Linux, you'll see something like:
deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

**Understanding the format:**
```
deb http://http.kali.org/kali kali-rolling main contrib non-free
│   │                         │            │    │       │
│   │                         │            │    │       └── Non-free packages
│   │                         │            │    └── Free additions
│   │                         │            └── Main packages
│   │                         └── Distribution/release name
│   └── Repository URL
└── Package type (deb = binary packages, deb-src = source code)
```

### Repository Sections

```
┌─────────────────────────────────────────────────────────────────┐
│                 REPOSITORY SECTIONS                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  main           - Officially supported, free software            │
│                   (Most of what you'll use)                      │
│                                                                  │
│  contrib        - Free software that depends on non-free         │
│                   (e.g., free game that needs non-free assets)  │
│                                                                  │
│  non-free       - Software with restrictive licenses             │
│                   (e.g., firmware, proprietary drivers)         │
│                                                                  │
│  non-free-firmware - Hardware firmware with restrictive licenses │
│                   (e.g., WiFi firmware, graphics firmware)       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

⚠️ **Kali Linux Note**: Kali Linux is a **rolling release** distribution. This means packages are continuously updated rather than waiting for major releases. Always keep your system updated!

---

## 3. Updating Package Information

### `sudo apt update` - Refresh Package Lists

Before installing or upgrading anything, always update your package lists:

```bash
sudo apt update
```

**What this does:**
1. Contacts all configured repositories
2. Downloads the latest package index
3. Updates your local database of available packages

```
┌─────────────────────────────────────────────────────────────────┐
│         apt update vs apt upgrade - DON'T CONFUSE THEM!         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  apt update     = Updates the PACKAGE LIST (what's available)    │
│                   Does NOT install anything                      │
│                   Like refreshing a catalog                      │
│                                                                  │
│  apt upgrade    = Updates the PACKAGES themselves                │
│                   Actually downloads and installs updates        │
│                   Like ordering from the catalog                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Understanding the Output

```bash
$ sudo apt update
Hit:1 http://http.kali.org/kali kali-rolling InRelease
Get:2 http://http.kali.org/kali kali-rolling/main amd64 Packages [19.0 MB]
Get:3 http://http.kali.org/kali kali-rolling/contrib amd64 Packages [116 kB]
Fetched 19.2 MB in 5s (3,842 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
45 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

| Output | Meaning |
|--------|---------|
| `Hit` | Package list is already up-to-date |
| `Get` | Downloading updated package list |
| `Ign` | Repository was ignored (possibly unavailable) |
| `Err` | Error accessing repository |

### How Often to Update

```
┌─────────────────────────────────────────────────────────────────┐
│                    WHEN TO RUN apt update                        │
├─────────────────────────────────────────────────────────────────┤
│  ✓ Before installing new software                                │
│  ✓ Before running apt upgrade                                    │
│  ✓ After adding new repositories                                 │
│  ✓ At least once a week on a rolling release like Kali           │
│  ✓ When troubleshooting package issues                           │
└─────────────────────────────────────────────────────────────────┘
```

💡 **Tip**: Make it a habit to run `sudo apt update` before any package operation. It only takes a few seconds and ensures you have the latest information.

---

## 4. Upgrading Packages

### `sudo apt upgrade` - Upgrade Installed Packages

After updating your package lists, upgrade your installed packages:

```bash
# Update package lists first
sudo apt update

# Then upgrade packages
sudo apt upgrade
```

**What this does:**
1. Checks which installed packages have newer versions
2. Downloads the new versions
3. Installs the updates
4. **Does NOT remove any packages**

### `sudo apt full-upgrade` - Handle Dependency Changes

Sometimes upgrades require installing new packages or removing old ones:

```bash
sudo apt full-upgrade
```

**Difference from `apt upgrade`:**
- `apt upgrade`: Won't remove packages or install new ones as dependencies
- `apt full-upgrade`: Will remove/install packages if needed for the upgrade

### Understanding Upgrade Output

```bash
$ sudo apt upgrade
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  curl libcurl4 wget
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 1,256 kB of archives.
After this operation, 12.3 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```

**What to look at:**
- Which packages will be upgraded
- How many new packages will be installed
- How many packages will be removed (⚠️ review these!)
- Disk space changes

### When to Use Each

| Scenario | Command |
|----------|---------|
| Regular updates | `sudo apt upgrade` |
| Kali rolling updates | `sudo apt full-upgrade` |
| Major distribution upgrade | `sudo apt full-upgrade` |
| Conservative/production servers | `sudo apt upgrade` |

⚠️ **Kali Linux Note**: For Kali's rolling release, `sudo apt full-upgrade` is recommended to ensure all packages are properly updated with their new dependencies.

### The Complete Update Workflow

```bash
# The standard update process:
sudo apt update           # Refresh package lists
sudo apt full-upgrade     # Upgrade all packages

# Or as one command:
sudo apt update && sudo apt full-upgrade
```

---

## 5. Searching for Packages

### `apt search` - Find Packages

Find packages by name or description:

```bash
# Search for packages containing "editor"
apt search editor

# Search for image-related packages
apt search image viewer

# Case-insensitive by default
apt search VIM
```

**Output:**
```
Sorting... Done
Full Text Search... Done
vim/kali-rolling 2:9.0.1378-2 amd64
  Vi IMproved - enhanced vi editor

vim-gtk3/kali-rolling 2:9.0.1378-2 amd64
  Vi IMproved - enhanced vi editor - with GTK3 GUI

vim-tiny/kali-rolling 2:9.0.1378-2 amd64
  Vi IMproved - enhanced vi editor - compact version
```

### `apt show` - Package Details

Get detailed information about a package:

```bash
apt show htop
```

**Output:**
```
Package: htop
Version: 3.2.1-1
Priority: optional
Section: utils
Maintainer: Daniel Lange <DLange@debian.org>
Installed-Size: 372 kB
Depends: libc6 (>= 2.29), libncursesw6 (>= 6), libtinfo6 (>= 6)
Homepage: https://htop.dev/
Description: interactive processes viewer
 Htop is an ncursed-based process viewer similar to top, but it allows
 one to scroll the list vertically and horizontally to see all processes
 and their full command lines.
```

**Useful information:**
- **Version**: What version is available
- **Installed-Size**: How much disk space it uses
- **Depends**: What other packages it needs
- **Description**: What the package does

### `apt list` - List Packages

List packages in various ways:

```bash
# List all available packages
apt list

# List installed packages only
apt list --installed

# List packages that can be upgraded
apt list --upgradable

# Search for packages by name pattern
apt list vim*
apt list *python*
```

**Examples:**
```bash
# Check if a package is installed
apt list --installed | grep htop

# Count installed packages
apt list --installed | wc -l

# Find all packages with "network" in the name
apt list *network*
```

### Practical Search Examples

```bash
# Find a text editor
apt search "text editor"

# Find network scanning tools
apt search network scanner

# Find PDF tools
apt search pdf

# Find Python packages
apt search python3-

# Check if vim is installed
apt list --installed vim
```

💡 **Tip**: Use `apt show` to verify you're installing the right package before running `apt install`.

---

## 6. Installing Packages

### `sudo apt install` - Install Packages

Install one or more packages:

```bash
# Install a single package
sudo apt install htop

# Install multiple packages at once
sudo apt install htop tree curl wget

# Install a specific version
sudo apt install nginx=1.18.0-0ubuntu1
```

### Understanding the Installation Process

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

### Understanding Dependencies

When you install a package, APT automatically handles **dependencies** - other packages your software needs:

```bash
$ sudo apt install nmap
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libblas3 liblinear4 liblua5.3-0 lua-lpeg nmap-common
Suggested packages:
  liblinear-tools liblinear-dev ncat ndiff zenmap
The following NEW packages will be installed:
  libblas3 liblinear4 liblua5.3-0 lua-lpeg nmap nmap-common
0 upgraded, 6 newly installed, 0 to remove and 0 not upgraded.
```

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEPENDENCY TYPES                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Depends         - Required to run (automatically installed)     │
│  Recommends      - Strongly suggested (usually installed)        │
│  Suggests        - Optional enhancements (not installed)         │
│  Conflicts       - Cannot be installed together                  │
│  Replaces        - Takes over files from another package         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Installing Local .deb Files

If you have a `.deb` file downloaded:

```bash
# Using apt (handles dependencies from repositories)
sudo apt install ./package.deb

# Note: The ./ is important - it tells apt this is a local file
```

### The `-y` Flag

The `-y` flag auto-confirms installation:

```bash
sudo apt install -y htop
```

⚠️ **Warning**: Use `-y` carefully! It skips the confirmation prompt, which is where you review what will be installed or removed. Best practices:

```
┌─────────────────────────────────────────────────────────────────┐
│                    THE -y FLAG                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✓ OK to use for single, known packages                          │
│      sudo apt install -y htop                                    │
│                                                                  │
│  ✗ Avoid for upgrades - always review what changes               │
│      sudo apt upgrade -y        ← Risky!                         │
│                                                                  │
│  ✗ Never use blindly in scripts without testing                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Installation Tips

```bash
# Always update first
sudo apt update
sudo apt install package-name

# Reinstall a package (if corrupted)
sudo apt install --reinstall package-name

# Fix missing dependencies
sudo apt install -f

# Dry run - see what would happen without doing it
sudo apt install --dry-run package-name
```

💡 **Tip**: Before installing, use `apt show package-name` to verify it's what you want and see its dependencies.

---

## 7. Removing Packages

### `sudo apt remove` - Remove Packages

Remove a package but keep its configuration files:

```bash
sudo apt remove htop
```

**What happens:**
- Program files are deleted
- Configuration files are **kept** (in case you reinstall)

### `sudo apt purge` - Complete Removal

Remove a package AND its configuration files:

```bash
sudo apt purge htop
```

**What happens:**
- Program files are deleted
- Configuration files are **also deleted**

### Understanding Remove vs Purge

```
┌─────────────────────────────────────────────────────────────────┐
│                    REMOVE vs PURGE                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  apt remove                                                      │
│  ───────────                                                     │
│  • Removes program files                                         │
│  • KEEPS configuration files in /etc                             │
│  • Good if you might reinstall later                             │
│  • Reinstalling will use old configuration                       │
│                                                                  │
│  apt purge                                                       │
│  ──────────                                                      │
│  • Removes program files                                         │
│  • REMOVES configuration files                                   │
│  • Clean slate if you reinstall                                  │
│  • Use when you want to completely remove something              │
│                                                                  │
│  Neither removes:                                                │
│  • User data/files (e.g., documents, databases)                  │
│  • Files in home directories                                     │
│  • Dependencies (use autoremove)                                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### `sudo apt autoremove` - Clean Up Dependencies

Remove packages that were installed as dependencies but are no longer needed:

```bash
sudo apt autoremove
```

**When packages become orphaned:**
```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│  1. You install Package A (which needs Dep1 and Dep2)            │
│     apt install A → installs A, Dep1, Dep2                       │
│                                                                  │
│  2. You remove Package A                                         │
│     apt remove A → removes only A                                │
│                                                                  │
│  3. Dep1 and Dep2 are now "orphaned"                             │
│     (installed automatically, no longer needed)                  │
│                                                                  │
│  4. apt autoremove cleans them up                                │
│     apt autoremove → removes Dep1, Dep2                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Recommended Removal Workflow

```bash
# Remove a package you no longer need
sudo apt remove package-name

# Clean up orphaned dependencies
sudo apt autoremove

# Or do both in one command
sudo apt remove package-name && sudo apt autoremove
```

⚠️ **Warning**: Always review what `autoremove` will delete before confirming!

---

## 8. Cleaning Up

### `sudo apt clean` - Clear Package Cache

APT keeps downloaded `.deb` files in a cache. Clear it to free disk space:

```bash
sudo apt clean
```

**What this does:**
- Removes all cached package files from `/var/cache/apt/archives/`
- Does NOT affect installed packages
- Frees disk space

### `sudo apt autoclean` - Remove Old Cached Versions

Remove only outdated cached packages (keeps current versions):

```bash
sudo apt autoclean
```

**Difference:**
- `apt clean`: Removes ALL cached `.deb` files
- `apt autoclean`: Removes only outdated cached `.deb` files

### The Package Cache Directory

```bash
# View the cache directory
ls -la /var/cache/apt/archives/

# Check cache size
du -sh /var/cache/apt/archives/
```

### Why Cleaning Matters

```
┌─────────────────────────────────────────────────────────────────┐
│                    DISK SPACE MANAGEMENT                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Package cache can grow large over time!                         │
│                                                                  │
│  Every apt install downloads .deb files that are kept.           │
│  Every apt upgrade downloads new versions.                       │
│  Old versions accumulate in the cache.                           │
│                                                                  │
│  On systems with limited disk space (VMs, small SSDs):           │
│  • Run apt clean periodically                                    │
│  • Or apt autoclean to keep current versions                     │
│                                                                  │
│  Check your cache size:                                          │
│    du -sh /var/cache/apt/archives/                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Complete Cleanup Command

```bash
# Full cleanup routine
sudo apt autoremove      # Remove orphaned packages
sudo apt clean           # Clear package cache
```

💡 **Tip**: After a major upgrade or cleaning unused software, run these commands to reclaim disk space.

---

## 9. Working with dpkg (Low-Level)

### When to Use dpkg

While APT handles most tasks, sometimes you need dpkg:

- Installing a local `.deb` file that's not in repositories
- Querying detailed package information
- Troubleshooting package problems
- Finding which package owns a file

### `dpkg -i` - Install Local Package

```bash
# Install a downloaded .deb file
sudo dpkg -i package.deb
```

⚠️ **Warning**: dpkg doesn't automatically install dependencies! If installation fails due to missing dependencies:

```bash
# Install the package (might fail with dependency errors)
sudo dpkg -i package.deb

# Fix missing dependencies
sudo apt install -f
```

### `dpkg -l` - List Installed Packages

```bash
# List all installed packages
dpkg -l

# Search for specific packages
dpkg -l | grep vim

# Show details about one package
dpkg -l htop
```

**Output format:**
```
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name           Version      Architecture Description
+++-==============-============-============-=====================================
ii  htop           3.2.1-1      amd64        interactive processes viewer
```

The `ii` means: desired=Install, status=Installed.

### `dpkg -L` - List Package Files

See what files a package installed:

```bash
# What files did htop install?
dpkg -L htop
```

**Output:**
```
/.
/usr
/usr/bin
/usr/bin/htop
/usr/share
/usr/share/doc
/usr/share/doc/htop
/usr/share/man
/usr/share/man/man1
/usr/share/man/man1/htop.1.gz
```

### `dpkg -S` - Find Package Owning a File

Find which package installed a specific file:

```bash
# What package provides /bin/ls?
dpkg -S /bin/ls
```

**Output:**
```
coreutils: /bin/ls
```

### `dpkg --configure -a` - Fix Broken Packages

If a package installation was interrupted:

```bash
sudo dpkg --configure -a
```

This configures any packages that were unpacked but not configured.

### dpkg Quick Reference

| Command | Description |
|---------|-------------|
| `dpkg -i file.deb` | Install a .deb file |
| `dpkg -r package` | Remove a package |
| `dpkg -P package` | Purge a package |
| `dpkg -l` | List installed packages |
| `dpkg -l pattern` | List packages matching pattern |
| `dpkg -L package` | List files in a package |
| `dpkg -S /path/file` | Find which package owns a file |
| `dpkg --configure -a` | Configure unpacked packages |
| `dpkg --get-selections` | List package selections |

---

## 10. Managing Repositories

### Adding Repositories

Sometimes you need software not in the default repositories. You can add additional repositories:

```bash
# View current sources
cat /etc/apt/sources.list

# Additional sources in this directory
ls /etc/apt/sources.list.d/
```

⚠️ **Warning**: Only add repositories from trusted sources! Malicious repositories can compromise your system.

### Adding a Repository (Example)

```bash
# Add a repository (example structure)
echo "deb http://repository.url/path distribution component" | sudo tee /etc/apt/sources.list.d/repo-name.list

# Update to get packages from new repository
sudo apt update
```

### GPG Keys

Repositories use GPG keys to verify package authenticity:

```bash
# Add a repository's GPG key
wget -qO - https://repository.url/key.gpg | sudo apt-key add -

# Or the modern way (apt-key is deprecated)
wget -qO - https://repository.url/key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/repo-name.gpg
```

### Kali-Specific Repositories

Kali Linux has its own repository structure:

```bash
# View Kali sources
cat /etc/apt/sources.list

# Should show:
# deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

⚠️ **Kali Linux Note**: 
- Kali doesn't use PPAs (Personal Package Archives) like Ubuntu
- Stick to the official Kali repositories when possible
- Third-party repositories may cause conflicts with Kali's rolling release

### Security Considerations

```
┌─────────────────────────────────────────────────────────────────┐
│              REPOSITORY SECURITY CHECKLIST                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✓ Only add repositories from trusted sources                    │
│  ✓ Verify GPG keys match what the provider advertises            │
│  ✓ Use HTTPS when possible for repository URLs                   │
│  ✓ Regularly review your sources.list files                      │
│  ✓ Remove repositories you no longer use                         │
│                                                                  │
│  ✗ Don't add random repositories from untrusted tutorials        │
│  ✗ Don't ignore GPG key warnings                                 │
│  ✗ Don't add repositories that override system packages          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 11. Troubleshooting Common Issues

### "Could not get lock" Error

**Problem:**
```
E: Could not get lock /var/lib/dpkg/lock-frontend - open (11: Resource temporarily unavailable)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), is another process using it?
```

**Solution:**
1. Wait for the other process to finish (often automatic updates)
2. Check for running apt/dpkg processes:
```bash
ps aux | grep -E 'apt|dpkg'
```
3. If safe to stop them:
```bash
sudo killall apt apt-get
```
4. As last resort (be careful!):
```bash
sudo rm /var/lib/dpkg/lock-frontend
sudo rm /var/lib/apt/lists/lock
sudo dpkg --configure -a
```

### Broken Packages

**Problem:**
```
dpkg: error processing package xyz (--configure):
 dependency problems - leaving unconfigured
```

**Solution:**
```bash
# Try to fix dependencies
sudo apt --fix-broken install

# Or
sudo dpkg --configure -a

# Then update and try again
sudo apt update
sudo apt upgrade
```

### Missing Dependencies

**Problem:**
```
The following packages have unmet dependencies:
 package : Depends: dependency but it is not installable
```

**Solution:**
```bash
# Update package lists
sudo apt update

# Try to fix dependencies
sudo apt --fix-broken install

# If that fails, check if dependency is in different repository
apt search dependency-name
```

### GPG Key Errors

**Problem:**
```
W: GPG error: http://repository.url Release: The following signatures couldn't be verified
```

**Solution:**
```bash
# Add the missing key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys KEY_ID

# Or download and add manually
wget -qO - https://repository.url/key.gpg | sudo apt-key add -
```

### Network Issues

**Problem:**
```
Err:1 http://http.kali.org/kali kali-rolling InRelease
  Could not connect to http.kali.org:80
```

**Solution:**
```bash
# Check network connectivity
ping -c 3 http.kali.org

# Try a different mirror
# Edit /etc/apt/sources.list if needed

# Clear and retry
sudo apt clean
sudo apt update
```

### Troubleshooting Flowchart

```
┌─────────────────────────────────────────────────────────────────┐
│                    TROUBLESHOOTING STEPS                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. First, always try:                                           │
│     sudo apt update                                              │
│     sudo apt --fix-broken install                                │
│                                                                  │
│  2. If lock file error:                                          │
│     → Wait for other process to finish                           │
│     → Or kill apt/dpkg processes                                 │
│                                                                  │
│  3. If dependency error:                                         │
│     → sudo apt --fix-broken install                              │
│     → sudo dpkg --configure -a                                   │
│                                                                  │
│  4. If still broken:                                             │
│     → Check /var/log/apt/history.log for what changed            │
│     → Consider reinstalling problematic package                  │
│                                                                  │
│  5. Nuclear option (last resort):                                │
│     → Remove and purge the problematic package                   │
│     → Clean caches                                               │
│     → Reinstall                                                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 12. Best Practices

### Before Installing

```
┌─────────────────────────────────────────────────────────────────┐
│                    BEFORE INSTALLING SOFTWARE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. Always update first                                          │
│     sudo apt update                                              │
│                                                                  │
│  2. Search and verify you have the right package                 │
│     apt search packagename                                       │
│     apt show packagename                                         │
│                                                                  │
│  3. Review what will be installed                                │
│     (Don't just blindly press Y!)                                │
│                                                                  │
│  4. Check dependencies and disk space requirements               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Keeping Your System Updated

```bash
# Regular update routine (run weekly or more on Kali)
sudo apt update          # Refresh package lists
sudo apt full-upgrade    # Upgrade all packages
sudo apt autoremove      # Remove orphaned packages
sudo apt clean           # Clear package cache (optional)
```

⚠️ **Warning**: Security updates are critical! On a rolling release like Kali, updates include security fixes. Regular updates protect against vulnerabilities.

### Repository Safety

- ✅ Use official repositories whenever possible
- ✅ Verify GPG keys for third-party repositories
- ✅ Keep the number of third-party repositories minimal
- ❌ Don't add repositories you don't trust
- ❌ Don't ignore GPG warnings

### Kali Rolling Release Considerations

```
┌─────────────────────────────────────────────────────────────────┐
│              KALI ROLLING RELEASE NOTES                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Kali Linux uses a rolling release model:                        │
│                                                                  │
│  ✓ Packages are continuously updated                             │
│  ✓ No need to upgrade to a new "version"                         │
│  ✓ Always have latest tools and security fixes                   │
│                                                                  │
│  But this means:                                                 │
│  • Update frequently (at least weekly)                           │
│  • Use full-upgrade instead of just upgrade                      │
│  • Packages may occasionally break (rare but possible)           │
│  • Test critical tools after major updates                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### VM Snapshot Recommendation

💡 **Tip**: Before major upgrades, take a VM snapshot! If something goes wrong, you can quickly restore to a working state.

---

## Quick Reference

### APT Command Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│                APT COMMAND REFERENCE                          │
├──────────────────────────────────────────────────────────────┤
│  INFORMATION                                                  │
│  ───────────                                                  │
│  apt search keyword      Search for packages                  │
│  apt show package        Show package details                 │
│  apt list                List all packages                    │
│  apt list --installed    List installed packages              │
│  apt list --upgradable   List upgradable packages             │
├──────────────────────────────────────────────────────────────┤
│  UPDATING                                                     │
│  ────────                                                     │
│  apt update              Refresh package lists                │
│  apt upgrade             Upgrade packages (safe)              │
│  apt full-upgrade        Upgrade with dependency changes      │
├──────────────────────────────────────────────────────────────┤
│  INSTALLING                                                   │
│  ──────────                                                   │
│  apt install pkg         Install a package                    │
│  apt install p1 p2 p3    Install multiple packages            │
│  apt install ./file.deb  Install local .deb file              │
│  apt install -y pkg      Install without confirmation         │
│  apt reinstall pkg       Reinstall a package                  │
├──────────────────────────────────────────────────────────────┤
│  REMOVING                                                     │
│  ────────                                                     │
│  apt remove pkg          Remove package (keep config)         │
│  apt purge pkg           Remove package and config            │
│  apt autoremove          Remove orphaned dependencies         │
├──────────────────────────────────────────────────────────────┤
│  CLEANING                                                     │
│  ────────                                                     │
│  apt clean               Clear all cached packages            │
│  apt autoclean           Clear old cached packages            │
├──────────────────────────────────────────────────────────────┤
│  TROUBLESHOOTING                                              │
│  ───────────────                                              │
│  apt --fix-broken install    Fix broken dependencies          │
└──────────────────────────────────────────────────────────────┘
```

### dpkg Command Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│                dpkg COMMAND REFERENCE                         │
├──────────────────────────────────────────────────────────────┤
│  dpkg -i file.deb        Install a .deb file                  │
│  dpkg -r package         Remove a package                     │
│  dpkg -P package         Purge a package                      │
│  dpkg -l                 List all installed packages          │
│  dpkg -l pattern         List packages matching pattern       │
│  dpkg -L package         List files installed by package      │
│  dpkg -S /path/to/file   Find which package owns a file       │
│  dpkg --configure -a     Configure unpacked packages          │
│  dpkg --get-selections   Get package selection states         │
└──────────────────────────────────────────────────────────────┘
```

### Common Workflows

```bash
# Install new software
sudo apt update
sudo apt install package-name

# Regular system update (Kali)
sudo apt update && sudo apt full-upgrade

# Remove software completely
sudo apt purge package-name
sudo apt autoremove

# Find and install a tool
apt search keyword
apt show package-name
sudo apt install package-name

# Fix broken packages
sudo apt --fix-broken install
sudo dpkg --configure -a

# Clean up disk space
sudo apt autoremove
sudo apt clean
```

---

## ✅ What You Learned

In this module, you learned:

- [x] What package managers are and why they're important
- [x] The difference between APT and dpkg
- [x] How repositories work and the sources.list file
- [x] How to update package lists with `apt update`
- [x] How to upgrade packages with `apt upgrade` and `apt full-upgrade`
- [x] How to search for packages with `apt search` and `apt show`
- [x] How to install packages with `apt install`
- [x] How to remove packages with `apt remove`, `apt purge`, and `apt autoremove`
- [x] How to clean up disk space with `apt clean`
- [x] How to use dpkg for low-level package operations
- [x] Basic repository management
- [x] How to troubleshoot common package issues
- [x] Best practices for keeping your system secure and updated

---

## Next Steps

In the next module, we'll explore **process management** - how to view running processes, manage background jobs, and control system services.

Practice the exercises to reinforce your understanding of package management with APT!