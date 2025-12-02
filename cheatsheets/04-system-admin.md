# System Administration Cheatsheet

> Quick reference for user, package, process, and service management.  
> *For detailed explanations, see Modules 6-8 and 10 of this course.*

---

## User Management

### View User Information

| Command | Description |
|---------|-------------|
| `whoami` | Current username |
| `id` | Current user's UID, GID, groups |
| `id username` | Specific user's info |
| `who` | Users currently logged in |
| `w` | Logged in users with activity |
| `last` | Login history |
| `cat /etc/passwd` | All user accounts |

### Create Users

```bash
# Basic user creation
sudo useradd username

# Create with home directory
sudo useradd -m username

# Create with specific shell
sudo useradd -m -s /bin/bash username

# Create with comment (full name)
sudo useradd -m -c "John Doe" john

# Create with specific UID
sudo useradd -m -u 1500 username

# Create with specific groups
sudo useradd -m -G sudo,developers username

# Full example
sudo useradd -m -s /bin/bash -c "John Doe" -G sudo john
```

### Modify Users

| Command | Description |
|---------|-------------|
| `sudo usermod -l newname oldname` | Change username |
| `sudo usermod -d /new/home user` | Change home directory |
| `sudo usermod -s /bin/zsh user` | Change shell |
| `sudo usermod -aG group user` | Add to group (keep existing) |
| `sudo usermod -G group1,group2 user` | Set groups (replace) |
| `sudo usermod -L user` | Lock account |
| `sudo usermod -U user` | Unlock account |

### Delete Users

```bash
# Delete user (keep home directory)
sudo userdel username

# Delete user AND home directory
sudo userdel -r username
```

### Password Management

| Command | Description |
|---------|-------------|
| `passwd` | Change your password |
| `sudo passwd user` | Change user's password |
| `sudo passwd -l user` | Lock password |
| `sudo passwd -u user` | Unlock password |
| `sudo passwd -e user` | Expire password (force change) |
| `sudo chage -l user` | View password aging info |

---

## Group Management

### View Groups

| Command | Description |
|---------|-------------|
| `groups` | Your groups |
| `groups username` | User's groups |
| `cat /etc/group` | All groups |
| `getent group groupname` | Group details |

### Create/Modify/Delete Groups

```bash
# Create group
sudo groupadd groupname

# Create with specific GID
sudo groupadd -g 1500 groupname

# Delete group
sudo groupdel groupname

# Rename group
sudo groupmod -n newname oldname

# Add user to group
sudo usermod -aG groupname username

# Remove user from group
sudo gpasswd -d username groupname
```

---

## Package Management (APT)

### Update & Upgrade

| Command | Description |
|---------|-------------|
| `sudo apt update` | Update package lists |
| `sudo apt upgrade` | Upgrade installed packages |
| `sudo apt full-upgrade` | Upgrade with dependency changes |
| `sudo apt update && sudo apt upgrade -y` | Update and upgrade (auto-yes) |

### Install & Remove

| Command | Description |
|---------|-------------|
| `sudo apt install package` | Install package |
| `sudo apt install pkg1 pkg2` | Install multiple |
| `sudo apt install -y package` | Install without prompting |
| `sudo apt remove package` | Remove package (keep config) |
| `sudo apt purge package` | Remove package and config |
| `sudo apt autoremove` | Remove unused dependencies |

### Search & Info

| Command | Description |
|---------|-------------|
| `apt search keyword` | Search for packages |
| `apt show package` | Package information |
| `apt list --installed` | List installed packages |
| `apt list --upgradable` | List upgradable packages |
| `dpkg -l` | List all installed (detailed) |
| `dpkg -L package` | List files in package |
| `dpkg -S /path/to/file` | Find which package owns file |

### Package Cache

```bash
# Clean downloaded package files
sudo apt clean

# Remove old package files
sudo apt autoclean

# Fix broken dependencies
sudo apt --fix-broken install
```

---

## Process Management

### View Processes

| Command | Description |
|---------|-------------|
| `ps` | Your processes |
| `ps aux` | All processes (detailed) |
| `ps -ef` | All processes (full format) |
| `ps aux --sort=-%mem` | Sort by memory usage |
| `ps aux --sort=-%cpu` | Sort by CPU usage |
| `pgrep name` | Find PID by name |
| `pidof program` | Get PID of program |

### Interactive Process Viewers

| Command | Description |
|---------|-------------|
| `top` | Interactive process viewer |
| `htop` | Enhanced process viewer |
| `btop` | Modern resource monitor |

#### top Commands
| Key | Action |
|-----|--------|
| `q` | Quit |
| `k` | Kill process |
| `r` | Renice process |
| `M` | Sort by memory |
| `P` | Sort by CPU |
| `h` | Help |

### Kill Processes

| Command | Description |
|---------|-------------|
| `kill PID` | Send SIGTERM (graceful) |
| `kill -9 PID` | Send SIGKILL (force) |
| `kill -15 PID` | Send SIGTERM (default) |
| `killall name` | Kill by process name |
| `pkill name` | Kill by name pattern |
| `pkill -u user` | Kill all user's processes |

#### Common Signals
| Signal | Number | Description |
|--------|--------|-------------|
| SIGHUP | 1 | Hangup / reload config |
| SIGINT | 2 | Interrupt (Ctrl+C) |
| SIGKILL | 9 | Force kill (cannot ignore) |
| SIGTERM | 15 | Graceful termination |
| SIGSTOP | 19 | Pause process |
| SIGCONT | 18 | Resume process |

### Background & Foreground

| Command | Description |
|---------|-------------|
| `command &` | Run in background |
| `Ctrl+Z` | Suspend current process |
| `bg` | Resume in background |
| `fg` | Bring to foreground |
| `jobs` | List background jobs |
| `fg %1` | Bring job #1 to foreground |
| `nohup cmd &` | Run immune to hangups |

---

## Service Management (systemctl)

### Basic Commands

| Command | Description |
|---------|-------------|
| `sudo systemctl start service` | Start service |
| `sudo systemctl stop service` | Stop service |
| `sudo systemctl restart service` | Restart service |
| `sudo systemctl reload service` | Reload config |
| `sudo systemctl status service` | Check status |
| `systemctl is-active service` | Check if running |
| `systemctl is-enabled service` | Check if enabled |

### Enable/Disable

| Command | Description |
|---------|-------------|
| `sudo systemctl enable service` | Enable at boot |
| `sudo systemctl disable service` | Disable at boot |
| `sudo systemctl enable --now service` | Enable and start |
| `sudo systemctl disable --now service` | Disable and stop |

### List Services

```bash
# List all services
systemctl list-units --type=service

# List running services
systemctl list-units --type=service --state=running

# List enabled services
systemctl list-unit-files --type=service --state=enabled

# List failed services
systemctl --failed
```

### System Commands

| Command | Description |
|---------|-------------|
| `sudo systemctl reboot` | Reboot system |
| `sudo systemctl poweroff` | Shutdown system |
| `sudo systemctl suspend` | Suspend system |
| `systemctl daemon-reload` | Reload systemd config |

---

## Cron (Scheduled Tasks)

### Crontab Commands

| Command | Description |
|---------|-------------|
| `crontab -e` | Edit your crontab |
| `crontab -l` | List your cron jobs |
| `crontab -r` | Remove your crontab |
| `sudo crontab -u user -e` | Edit user's crontab |
| `sudo crontab -u user -l` | List user's crontab |

### Cron Syntax

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 6) (Sunday = 0)
│ │ │ │ │
* * * * * command to execute
```

### Special Characters

| Character | Meaning | Example |
|-----------|---------|---------|
| `*` | Any value | `* * * * *` = every minute |
| `,` | Value list | `1,15 * * * *` = min 1 and 15 |
| `-` | Range | `1-5 * * * *` = min 1 through 5 |
| `/` | Step values | `*/15 * * * *` = every 15 min |

### Common Examples

```bash
# Every minute
* * * * * /path/to/script.sh

# Every hour at minute 0
0 * * * * /path/to/script.sh

# Every day at midnight
0 0 * * * /path/to/script.sh

# Every day at 2:30 AM
30 2 * * * /path/to/script.sh

# Every Monday at 9 AM
0 9 * * 1 /path/to/script.sh

# Every 15 minutes
*/15 * * * * /path/to/script.sh

# First day of month at noon
0 12 1 * * /path/to/script.sh

# Weekdays at 6 PM
0 18 * * 1-5 /path/to/script.sh

# Every Sunday at 3 AM
0 3 * * 0 /path/to/script.sh
```

### Special Strings

| String | Equivalent |
|--------|------------|
| `@reboot` | Run once at startup |
| `@yearly` | `0 0 1 1 *` |
| `@monthly` | `0 0 1 * *` |
| `@weekly` | `0 0 * * 0` |
| `@daily` | `0 0 * * *` |
| `@hourly` | `0 * * * *` |

### Cron Directories

```bash
/etc/cron.d/        # System cron jobs
/etc/cron.daily/    # Run daily
/etc/cron.hourly/   # Run hourly
/etc/cron.weekly/   # Run weekly
/etc/cron.monthly/  # Run monthly
```

---

## Disk Management

### Disk Space

| Command | Description |
|---------|-------------|
| `df -h` | Disk space usage (human-readable) |
| `df -hT` | Include filesystem type |
| `df -i` | Inode usage |

### Directory Size

| Command | Description |
|---------|-------------|
| `du -sh directory/` | Directory size summary |
| `du -sh *` | Size of each item |
| `du -h --max-depth=1` | One level deep |
| `du -ah directory/` | All files recursively |

### Find Large Files

```bash
# Find files > 100MB
find / -type f -size +100M 2>/dev/null

# Top 10 largest directories
du -h /home | sort -rh | head -10

# Largest files in directory
find . -type f -exec du -h {} + | sort -rh | head -10

# NCurses disk usage (if installed)
ncdu /home
```

### Disk Information

| Command | Description |
|---------|-------------|
| `lsblk` | List block devices |
| `blkid` | Block device attributes |
| `fdisk -l` | Partition information |
| `mount` | Show mounted filesystems |

---

## System Information

| Command | Description |
|---------|-------------|
| `uname -a` | Kernel information |
| `hostnamectl` | System hostname info |
| `uptime` | System uptime |
| `free -h` | Memory usage |
| `lscpu` | CPU information |
| `lsb_release -a` | Distribution info |
| `cat /etc/os-release` | OS information |

---

## Log Files

### Important Logs

| Log File | Description |
|----------|-------------|
| `/var/log/syslog` | General system log |
| `/var/log/auth.log` | Authentication log |
| `/var/log/kern.log` | Kernel log |
| `/var/log/dmesg` | Boot messages |
| `/var/log/apt/` | APT package logs |

### View Logs

```bash
# View with less
less /var/log/syslog

# Follow log in real-time
tail -f /var/log/syslog

# Last 50 lines
tail -50 /var/log/auth.log

# Search in logs
grep "error" /var/log/syslog

# Journalctl (systemd logs)
journalctl                    # All logs
journalctl -u service         # Service logs
journalctl -f                 # Follow logs
journalctl --since "1 hour ago"
journalctl -p err             # Only errors
```

---

## Quick Reference Card

```
USERS & GROUPS:
  sudo useradd -m user     # Create user with home
  sudo userdel -r user     # Delete user completely
  sudo usermod -aG grp usr # Add user to group
  sudo passwd user         # Set password

PACKAGES (APT):
  sudo apt update          # Update package lists
  sudo apt upgrade         # Upgrade packages
  sudo apt install pkg     # Install package
  sudo apt remove pkg      # Remove package

PROCESSES:
  ps aux                   # List all processes
  kill PID                 # Graceful kill
  kill -9 PID              # Force kill
  top / htop               # Interactive viewer

SERVICES:
  sudo systemctl start svc   # Start service
  sudo systemctl enable svc  # Enable at boot
  sudo systemctl status svc  # Check status

CRON:
  crontab -e               # Edit cron jobs
  * * * * * command        # min hr day mon dow

DISK:
  df -h                    # Disk space
  du -sh dir/              # Directory size
```

---

*Reference: Modules 6-8 and 10 of Linux 101 Course*