# 📖 Capstone Project Solution Guide

## ⚠️ Important Notice

**Try to complete the project on your own first!**

This solution guide is provided as a reference. Using it without attempting the tasks yourself will:
- Reduce your learning
- Leave gaps in your understanding
- Make you less prepared for real sysadmin work

**Recommended approach:**
1. Attempt each phase independently
2. If stuck for 20+ minutes, check only that specific section
3. After completing, compare your solution with this guide

---

## Complete Solution

### Prerequisites Check

Before starting, verify your environment:

```bash
# Check you have root access
sudo whoami  # Should output: root

# Check network connectivity
ping -c 2 google.com

# Take a VM snapshot!
# VirtualBox: Machine → Take Snapshot
# VMware: VM → Snapshot → Take Snapshot
```

---

# Phase 1: Initial System Setup

## Task 1.1: Document System Information

### Commands

```bash
# Create documentation directory
sudo mkdir -p /root/documentation

# Create system information file with all required data
cat << 'EOF' | sudo tee /root/documentation/system-info.txt
=== TechStartup GmbH Server Documentation ===
Generated: $(date)

=== Basic Information ===
EOF

# Append system information
echo "Date: $(date)" | sudo tee -a /root/documentation/system-info.txt
echo "Hostname: $(hostname)" | sudo tee -a /root/documentation/system-info.txt
echo "" | sudo tee -a /root/documentation/system-info.txt

echo "=== Operating System ===" | sudo tee -a /root/documentation/system-info.txt
cat /etc/os-release | sudo tee -a /root/documentation/system-info.txt
echo "" | sudo tee -a /root/documentation/system-info.txt

echo "=== Kernel Version ===" | sudo tee -a /root/documentation/system-info.txt
uname -r | sudo tee -a /root/documentation/system-info.txt
echo "" | sudo tee -a /root/documentation/system-info.txt

echo "=== CPU Information ===" | sudo tee -a /root/documentation/system-info.txt
lscpu | head -15 | sudo tee -a /root/documentation/system-info.txt
echo "" | sudo tee -a /root/documentation/system-info.txt

echo "=== Memory Information ===" | sudo tee -a /root/documentation/system-info.txt
free -h | sudo tee -a /root/documentation/system-info.txt
echo "" | sudo tee -a /root/documentation/system-info.txt

echo "=== Disk Space ===" | sudo tee -a /root/documentation/system-info.txt
df -h | sudo tee -a /root/documentation/system-info.txt
echo "" | sudo tee -a /root/documentation/system-info.txt

echo "=== Current User ===" | sudo tee -a /root/documentation/system-info.txt
whoami | sudo tee -a /root/documentation/system-info.txt
```

### Expected Output

Your system-info.txt should look similar to:

```
=== TechStartup GmbH Server Documentation ===
Date: Mon Dec  2 12:00:00 CET 2024
Hostname: kali

=== Operating System ===
PRETTY_NAME="Kali GNU/Linux Rolling"
NAME="Kali GNU/Linux"
VERSION_ID="2024.3"
...

=== Kernel Version ===
6.5.0-kali3-amd64

=== CPU Information ===
Architecture:            x86_64
CPU(s):                  2
...

=== Memory Information ===
              total        used        free
Mem:          3.8Gi       1.2Gi       2.0Gi
...

=== Disk Space ===
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   15G   33G  31% /
...
```

### Why This Matters

System documentation is the foundation of good IT operations:
- **Troubleshooting**: When something breaks, you need to know what "normal" looks like
- **Compliance**: Many industries require documented baselines
- **Handover**: Your successor needs to understand the system
- **Capacity planning**: Tracking resources over time reveals trends

---

## Task 1.2: Explore the Filesystem

### Commands

```bash
# Check home directory structure
ls -la /home

# Check log directory
ls -la /var/log

# Check configuration directory
ls -la /etc | head -20

# Check disk usage by directory
du -sh /var/*

# Check disk usage of /home
du -sh /home/*
```

### Expected Output

```bash
# ls -la /home might show:
total 12
drwxr-xr-x  3 root root 4096 Dec  2 10:00 .
drwxr-xr-x 18 root root 4096 Dec  2 09:00 ..
drwxr-xr-x 15 kali kali 4096 Dec  2 10:00 kali

# du -sh /var/* might show:
1.2G    /var/cache
100M    /var/log
50M     /var/lib
...
```

### Why This Matters

Exploring before making changes prevents:
- Overwriting existing important files
- Creating duplicate structures
- Misunderstanding the current configuration

---

## Task 1.3: Create Project Directory Structure

### Commands

```bash
# Create all required directories
sudo mkdir -p /home/projects/webapp
sudo mkdir -p /home/projects/scripts
sudo mkdir -p /var/log/company

# Create README for /home/projects
sudo tee /home/projects/README.md << 'EOF'
# TechStartup GmbH - Projects Directory

## Purpose
This directory contains all company projects and collaborative workspaces.

## Structure
- `webapp/` - Web application source code and assets
- `scripts/` - System administration and automation scripts

## Access
- **Developers**: Read/Write to all subdirectories
- **Team Leads**: Full access including scripts management

## Guidelines
1. All code should be version controlled with git
2. Update documentation when making significant changes
3. Follow company coding standards

## Contact
System Administrator: admin@techstartup.de

---
Created: $(date)
Created by: Junior Sysadmin
EOF

# Create README for webapp
sudo tee /home/projects/webapp/README.md << 'EOF'
# Web Application Directory

## Purpose
Storage for web application source code, assets, and configuration.

## Contents
- Source code files
- HTML/CSS/JavaScript assets
- Application configuration
- Development resources

## Access Permissions
- Group: developers
- Permissions: Read/Write for all developers

## Git Repository
Initialize git repository for version control:
```bash
git init
git add .
git commit -m "Initial commit"
```

---
Created: $(date)
EOF

# Create README for scripts
sudo tee /home/projects/scripts/README.md << 'EOF'
# System Administration Scripts

## Purpose
Central repository for system administration and automation scripts.

## Contents
- System health monitoring scripts
- Backup automation
- User management scripts
- Maintenance utilities

## Access Permissions
- Group: leads
- Permissions: Read for developers, Read/Write for leads

## Script Standards
1. Include shebang line (#!/bin/bash)
2. Add description header with:
   - Purpose
   - Author
   - Date created
   - Usage instructions
3. Include error handling
4. Log important actions

## Available Scripts
(To be populated as scripts are created)

---
Created: $(date)
EOF

# Create README for company logs
sudo tee /var/log/company/README.md << 'EOF'
# Company Application Logs

## Purpose
Centralized location for company-specific application and script logs.

## Contents
- Daily system reports
- Health check outputs
- Application logs
- Audit logs

## Log Rotation
Logs in this directory should be rotated to prevent disk space issues.
Consider implementing logrotate configuration.

## Retention Policy
- Daily reports: Keep 30 days
- Health checks: Keep 14 days
- Error logs: Keep 90 days

## Monitoring
Review logs regularly for:
- Disk space warnings
- Service failures
- Security alerts

---
Created: $(date)
EOF
```

### Verify

```bash
# Check structure
tree /home/projects 2>/dev/null || ls -laR /home/projects
ls -la /var/log/company

# Verify READMEs exist
cat /home/projects/README.md
```

### Why This Matters

Well-organized directories:
- Make it easy for team members to find files
- Reduce confusion and mistakes
- Enable proper permission structures
- Scale better as the team grows

---

# Phase 2: User and Access Management

## Task 2.1: Create Groups

### Commands

```bash
# Create developer group
sudo groupadd developers

# Create leads group
sudo groupadd leads

# Verify groups were created
cat /etc/group | grep -E "developers|leads"
```

### Expected Output

```
developers:x:1001:
leads:x:1002:
```

### Why This Matters

Groups simplify permission management:
- Set permissions once for the group
- Add/remove users without changing file permissions
- Easier auditing of who has access to what

---

## Task 2.2: Create User Accounts

### Commands

```bash
# Create developer1
sudo useradd -m -s /bin/bash -G developers -c "Anna Schmidt - Developer" developer1

# Create developer2
sudo useradd -m -s /bin/bash -G developers -c "Max Müller - Developer" developer2

# Create teamlead (member of both groups)
sudo useradd -m -s /bin/bash -G developers,leads -c "Lisa Fischer - Team Lead" teamlead

# Set passwords (use "Linux123!" for testing)
echo "developer1:Linux123!" | sudo chpasswd
echo "developer2:Linux123!" | sudo chpasswd
echo "teamlead:Linux123!" | sudo chpasswd

# Verify users
id developer1
id developer2
id teamlead
```

### Expected Output

```bash
# id developer1
uid=1001(developer1) gid=1001(developer1) groups=1001(developer1),1002(developers)

# id developer2
uid=1002(developer2) gid=1002(developer2) groups=1002(developer2),1002(developers)

# id teamlead
uid=1003(teamlead) gid=1003(teamlead) groups=1003(teamlead),1002(developers),1003(leads)
```

### Command Explanation

| Option | Meaning |
|--------|---------|
| `-m` | Create home directory |
| `-s /bin/bash` | Set default shell |
| `-G groups` | Add to supplementary groups |
| `-c "comment"` | Add user description |

### Why This Matters

Proper user accounts:
- Enable accountability (who did what)
- Allow individual access control
- Support compliance requirements
- Enable proper permission inheritance

---

## Task 2.3: Configure Home Directories

### Commands

```bash
# Verify home directories were created
ls -la /home

# Create welcome files
echo "Welcome to TechStartup GmbH, Anna (developer1)!" | sudo tee /home/developer1/welcome.txt
echo "Welcome to TechStartup GmbH, Max (developer2)!" | sudo tee /home/developer2/welcome.txt
echo "Welcome to TechStartup GmbH, Lisa (teamlead)!" | sudo tee /home/teamlead/welcome.txt

# Set correct ownership
sudo chown developer1:developer1 /home/developer1/welcome.txt
sudo chown developer2:developer2 /home/developer2/welcome.txt
sudo chown teamlead:teamlead /home/teamlead/welcome.txt

# Verify
ls -la /home/developer1/
ls -la /home/developer2/
ls -la /home/teamlead/
```

### Expected Output

```
-rw-r--r-- 1 developer1 developer1 44 Dec  2 12:00 welcome.txt
```

### Why This Matters

Each user needs:
- A private workspace for personal files
- Proper ownership for security
- A welcoming environment (onboarding matters!)

---

## Task 2.4: Set Up Shared Project Folder

### Commands

```bash
# Set ownership for project directories
# Main projects folder - owned by root, group developers
sudo chown root:developers /home/projects
# Webapp - owned by root, group developers
sudo chown root:developers /home/projects/webapp
# Scripts - owned by root, group leads (more restricted)
sudo chown root:leads /home/projects/scripts

# Set permissions with setgid bit
# 2775 = setgid + rwxrwxr-x (group can write, new files inherit group)
sudo chmod 2775 /home/projects
sudo chmod 2775 /home/projects/webapp

# 2755 = setgid + rwxr-xr-x (only group owner can write)
sudo chmod 2755 /home/projects/scripts

# Set up company log directory
sudo chown root:leads /var/log/company
sudo chmod 2775 /var/log/company

# Update README ownership
sudo chown root:developers /home/projects/README.md
sudo chown root:developers /home/projects/webapp/README.md
sudo chown root:leads /home/projects/scripts/README.md
sudo chown root:leads /var/log/company/README.md
```

### Verify Permissions

```bash
ls -la /home/projects/
```

### Expected Output

```
total 20
drwxrwsr-x 4 root developers 4096 Dec  2 12:00 .
drwxr-xr-x 6 root root       4096 Dec  2 12:00 ..
-rw-r--r-- 1 root developers  512 Dec  2 12:00 README.md
drwxrwsr-x 2 root developers 4096 Dec  2 12:00 webapp
drwxr-sr-x 2 root leads      4096 Dec  2 12:00 scripts
```

Notice the `s` in the group permission field - this is the setgid bit.

### Permission Breakdown

| Permission | Numeric | Meaning |
|------------|---------|---------|
| `rwxrwsr-x` | 2775 | Owner: rwx, Group: rws (setgid), Others: r-x |
| `rwxr-sr-x` | 2755 | Owner: rwx, Group: r-s (setgid), Others: r-x |

The setgid bit (`s`) ensures:
- New files inherit the directory's group
- Enables proper collaboration without manual chown

### Why This Matters

Proper shared permissions:
- Enable collaboration without security risks
- Prevent "permission denied" errors for teammates
- Maintain proper access control
- Simplify administration

---

## Task 2.5: Test Permissions

### Test Commands

```bash
echo "=== Testing Permissions ==="

# Test 1: developer1 writing to webapp (should succeed)
echo "Test 1: developer1 → webapp"
sudo -u developer1 touch /home/projects/webapp/test-dev1.txt && echo "✓ SUCCESS" || echo "✗ FAILED"

# Test 2: developer1 writing to scripts (should FAIL)
echo "Test 2: developer1 → scripts (should fail)"
sudo -u developer1 touch /home/projects/scripts/test-dev1.txt 2>&1 && echo "✗ SHOULD HAVE FAILED" || echo "✓ CORRECTLY DENIED"

# Test 3: teamlead writing to scripts (should succeed)
echo "Test 3: teamlead → scripts"
sudo -u teamlead touch /home/projects/scripts/test-lead.txt && echo "✓ SUCCESS" || echo "✗ FAILED"

# Test 4: teamlead writing to webapp (should succeed)
echo "Test 4: teamlead → webapp"
sudo -u teamlead touch /home/projects/webapp/test-lead.txt && echo "✓ SUCCESS" || echo "✗ FAILED"

# Test 5: Check group inheritance (setgid)
echo "Test 5: File group inheritance"
ls -la /home/projects/webapp/test-dev1.txt
# Should show group as 'developers', not 'developer1'

# Clean up test files
sudo rm -f /home/projects/webapp/test-*.txt
sudo rm -f /home/projects/scripts/test-*.txt

echo "=== Permission Tests Complete ==="
```

### Expected Results

```
Test 1: developer1 → webapp
✓ SUCCESS
Test 2: developer1 → scripts (should fail)
✓ CORRECTLY DENIED
Test 3: teamlead → scripts
✓ SUCCESS
Test 4: teamlead → webapp
✓ SUCCESS
Test 5: File group inheritance
-rw-r--r-- 1 developer1 developers 0 Dec  2 12:00 test-dev1.txt
```

### Why This Matters

Testing permissions:
- Confirms your configuration is correct
- Prevents security vulnerabilities
- Avoids user frustration later
- Documents expected behavior

---

# Phase 3: System Configuration

## Task 3.1: Update System Packages

### Commands

```bash
# Update package lists
sudo apt update

# Check upgradable packages
apt list --upgradable

# Count upgradable packages
apt list --upgradable 2>/dev/null | wc -l

# Upgrade packages (optional but recommended)
sudo apt upgrade -y

# Document the update
echo "=== Package Update Log ===" | sudo tee -a /root/documentation/system-info.txt
echo "Date: $(date)" | sudo tee -a /root/documentation/system-info.txt
echo "Packages upgraded: $(apt list --upgradable 2>/dev/null | wc -l)" | sudo tee -a /root/documentation/system-info.txt
```

### Why This Matters

System updates:
- Fix security vulnerabilities
- Improve stability
- Add new features
- Are required for compliance

---

## Task 3.2: Install Development Tools

### Commands

```bash
# Install all required packages
sudo apt install -y git vim curl wget htop tree ufw net-tools

# Verify installations
echo "=== Installed Tool Versions ==="
git --version
vim --version | head -1
curl --version | head -1
wget --version | head -1
htop --version
tree --version
ufw version
ifconfig --version 2>&1 | head -1
```

### Expected Output

```
=== Installed Tool Versions ===
git version 2.43.0
VIM - Vi IMproved 9.0
curl 8.4.0 (x86_64-pc-linux-gnu)
GNU Wget 1.21.3
htop 3.2.2
tree v2.1.1
ufw 0.36.2
net-tools 2.10-alpha
```

### Why This Matters

Standard tooling:
- Ensures consistency across team
- Eliminates "works on my machine" problems
- Reduces setup time for new team members
- Enables collaboration (everyone has git)

---

## Task 3.3: Create Package Documentation

### Commands

```bash
# Create complete package list
dpkg --get-selections | sudo tee /root/documentation/installed-packages.txt

# Create manually installed tools documentation
cat << 'EOF' | sudo tee /root/documentation/dev-tools.txt
=== TechStartup GmbH - Development Tools ===
Installed: $(date)
Installed by: Junior Sysadmin

=== Version Control ===
git - Distributed version control system
  Version: $(git --version)
  Purpose: Source code management, collaboration

=== Text Editors ===
vim - Vi IMproved text editor
  Version: $(vim --version | head -1)
  Purpose: Editing configuration files, scripting

=== Data Transfer ===
curl - Command-line data transfer
  Version: $(curl --version | head -1)
  Purpose: API testing, downloading files

wget - Network file downloader
  Version: $(wget --version | head -1)
  Purpose: Downloading files, mirroring

=== System Monitoring ===
htop - Interactive process viewer
  Version: $(htop --version)
  Purpose: Monitoring system resources

tree - Directory listing in tree format
  Version: $(tree --version)
  Purpose: Visualizing directory structures

=== Security ===
ufw - Uncomplicated Firewall
  Version: $(ufw version)
  Purpose: Managing firewall rules

=== Networking ===
net-tools - Network utilities
  Purpose: Network diagnostics (ifconfig, netstat)

=== Total Packages ===
$(dpkg --get-selections | wc -l) packages installed
EOF
```

### Why This Matters

Package documentation:
- Enables recreation of the environment
- Required for security audits
- Helps troubleshoot dependency issues
- Assists disaster recovery

---

## Task 3.4: Configure Message of the Day (MOTD)

### Commands

```bash
# Create custom MOTD
sudo tee /etc/motd << 'EOF'

╔════════════════════════════════════════════════════════════════╗
║                                                                ║
║           Welcome to TechStartup GmbH Dev Server               ║
║                                                                ║
╠════════════════════════════════════════════════════════════════╣
║                                                                ║
║  This is a company server. All activity may be monitored       ║
║  and logged for security purposes.                             ║
║                                                                ║
║  Unauthorized access is strictly prohibited and may be         ║
║  subject to legal action.                                      ║
║                                                                ║
╠════════════════════════════════════════════════════════════════╣
║                                                                ║
║  Support Contact: admin@techstartup.de                         ║
║  Documentation:   /root/documentation/                         ║
║  Projects:        /home/projects/                              ║
║                                                                ║
╚════════════════════════════════════════════════════════════════╝

EOF

# Test by viewing as a user
sudo -u developer1 cat /etc/motd
```

### Why This Matters

MOTD serves multiple purposes:
- **Legal notice**: Warning that activity is monitored
- **Deterrent**: Unauthorized users are warned
- **Information**: Users know where to find resources
- **Professionalism**: Creates a polished environment

---

# Phase 4: Services and Automation

## Task 4.1: Check and Document Running Services

### Commands

```bash
# Create services documentation
cat << 'EOF' | sudo tee /root/documentation/services.txt
=== TechStartup GmbH - Services Documentation ===
Generated: $(date)

=== Currently Running Services ===
EOF

systemctl list-units --type=service --state=running | sudo tee -a /root/documentation/services.txt

echo "" | sudo tee -a /root/documentation/services.txt
echo "=== Enabled Services (Start at Boot) ===" | sudo tee -a /root/documentation/services.txt
systemctl list-unit-files --type=service --state=enabled | sudo tee -a /root/documentation/services.txt
```

### Why This Matters

Service documentation helps:
- Identify unnecessary services (security)
- Troubleshoot startup issues
- Plan capacity
- Understand system dependencies

---

## Task 4.2: Configure SSH Service

### Commands

```bash
# Check SSH status
sudo systemctl status ssh

# If not running, start it
sudo systemctl start ssh

# Enable SSH to start at boot
sudo systemctl enable ssh

# Verify SSH is listening
sudo ss -tlnp | grep :22

# Document SSH configuration
cat << 'EOF' | sudo tee -a /root/documentation/services.txt

=== SSH Service Configuration ===
Status: $(systemctl is-active ssh)
Enabled at boot: $(systemctl is-enabled ssh)
Listening port: $(sudo ss -tlnp | grep ssh | awk '{print $4}')
Config file: /etc/ssh/sshd_config

Key settings:
$(grep -E "^(Port|PermitRootLogin|PasswordAuthentication)" /etc/ssh/sshd_config || echo "Using defaults")
EOF
```

### Expected Output

```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled)
     Active: active (running) since Mon 2024-12-02 12:00:00 CET
```

### Why This Matters

SSH is critical for:
- Remote administration
- Secure access to the server
- File transfers (SCP/SFTP)
- Tunneling and port forwarding

---

## Task 4.3: Create Daily Backup Report Cron Job

### Commands

```bash
# Create the daily report script
sudo tee /home/projects/scripts/daily-report.sh << 'EOF'
#!/bin/bash
#================================================================
# TechStartup GmbH - Daily System Report
#================================================================
# Description: Generates daily system status report
# Author: Junior Sysadmin
# Schedule: Daily at 6:00 AM via cron
#================================================================

# Configuration
REPORT_DIR="/var/log/company"
DATE=$(date +%Y%m%d)
REPORT_FILE="$REPORT_DIR/daily-report-$DATE.txt"

# Ensure report directory exists
mkdir -p "$REPORT_DIR"

# Generate report
{
    echo "╔════════════════════════════════════════════════════════════╗"
    echo "║        TechStartup GmbH - Daily System Report              ║"
    echo "╚════════════════════════════════════════════════════════════╝"
    echo ""
    echo "Generated: $(date)"
    echo "Hostname: $(hostname)"
    echo ""
    
    echo "=== Disk Usage ==="
    df -h
    echo ""
    
    echo "=== Memory Usage ==="
    free -h
    echo ""
    
    echo "=== Top 5 Processes by Memory ==="
    ps aux --sort=-%mem | head -6
    echo ""
    
    echo "=== Top 5 Processes by CPU ==="
    ps aux --sort=-%cpu | head -6
    echo ""
    
    echo "=== Service Status (Running) ==="
    systemctl list-units --type=service --state=running --no-pager | head -15
    echo ""
    
    echo "=== Recent Logins ==="
    last -n 5
    echo ""
    
    echo "=== System Uptime ==="
    uptime
    echo ""
    
    echo "Report complete."
    
} > "$REPORT_FILE"

echo "Daily report saved to: $REPORT_FILE"
EOF

# Make executable
sudo chmod +x /home/projects/scripts/daily-report.sh

# Set ownership
sudo chown root:leads /home/projects/scripts/daily-report.sh

# Test the script
sudo /home/projects/scripts/daily-report.sh

# View the generated report
cat /var/log/company/daily-report-$(date +%Y%m%d).txt

# Add to cron
(sudo crontab -l 2>/dev/null; echo "0 6 * * * /home/projects/scripts/daily-report.sh") | sudo crontab -

# Verify cron entry
sudo crontab -l
```

### Expected Cron Output

```
0 6 * * * /home/projects/scripts/daily-report.sh
```

### Cron Syntax Explanation

```
0 6 * * * command
│ │ │ │ │
│ │ │ │ └─ Day of week (0-7, 0 and 7 are Sunday)
│ │ │ └─── Month (1-12)
│ │ └───── Day of month (1-31)
│ └─────── Hour (0-23)
└───────── Minute (0-59)
```

So `0 6 * * *` means "at minute 0 of hour 6, every day, every month, every day of the week" = 6:00 AM daily.

### Why This Matters

Automated reports:
- Catch problems early (before users notice)
- Provide historical data for trending
- Reduce manual monitoring effort
- Enable proactive maintenance

---

## Task 4.4: Log Rotation Awareness

### Commands

```bash
# Check logrotate configuration
cat /etc/logrotate.conf

# List existing rotation rules
ls /etc/logrotate.d/

# Document log rotation
cat << 'EOF' | sudo tee -a /root/documentation/services.txt

=== Log Rotation Configuration ===
Main config: /etc/logrotate.conf
Rule files: /etc/logrotate.d/

Key settings from /etc/logrotate.conf:
- Rotation frequency: weekly
- Keep: 4 rotations
- Create new log files after rotation
- Compress old logs

Company logs (/var/log/company/):
Note: May need custom logrotate rule. Consider creating:
/etc/logrotate.d/company with rules for daily-report and health-check logs.

Recommended rotation policy:
- Daily reports: Keep 30 days
- Health checks: Keep 14 days
EOF
```

### Why This Matters

Without log rotation:
- Logs can fill up disk space
- System can crash due to full disk
- Performance degrades
- Important logs become hard to find

---

# Phase 5: Network and Security

## Task 5.1: Document Network Configuration

### Commands

```bash
# Create network documentation
cat << 'EOF' | sudo tee /root/documentation/network.txt
=== TechStartup GmbH - Network Documentation ===
Generated: $(date)

=== Hostname ===
$(hostname)

=== IP Addresses ===
EOF

ip addr | sudo tee -a /root/documentation/network.txt

cat << 'EOF' | sudo tee -a /root/documentation/network.txt

=== Routing Table ===
EOF

ip route | sudo tee -a /root/documentation/network.txt

cat << 'EOF' | sudo tee -a /root/documentation/network.txt

=== DNS Configuration ===
EOF

cat /etc/resolv.conf | sudo tee -a /root/documentation/network.txt

cat << 'EOF' | sudo tee -a /root/documentation/network.txt

=== Listening Ports ===
EOF

sudo ss -tlnp | sudo tee -a /root/documentation/network.txt

cat << 'EOF' | sudo tee -a /root/documentation/network.txt

=== Active Connections ===
EOF

sudo ss -tunp | head -20 | sudo tee -a /root/documentation/network.txt
```

### Why This Matters

Network documentation helps:
- Troubleshoot connectivity issues
- Identify listening services
- Plan firewall rules
- Assist disaster recovery

---

## Task 5.2: Test Network Connectivity

### Commands

```bash
# Test DNS resolution and connectivity
echo "=== Connectivity Tests ===" | sudo tee -a /root/documentation/network.txt
echo "Date: $(date)" | sudo tee -a /root/documentation/network.txt

# Test DNS
if ping -c 2 google.com &> /dev/null; then
    echo "DNS Resolution: PASS (google.com reachable)" | sudo tee -a /root/documentation/network.txt
else
    echo "DNS Resolution: FAIL" | sudo tee -a /root/documentation/network.txt
fi

# Test gateway
GATEWAY=$(ip route | grep default | awk '{print $3}')
if ping -c 2 $GATEWAY &> /dev/null; then
    echo "Gateway ($GATEWAY): PASS" | sudo tee -a /root/documentation/network.txt
else
    echo "Gateway ($GATEWAY): FAIL" | sudo tee -a /root/documentation/network.txt
fi

# Test HTTP
if curl -s -I https://www.google.com | head -1 | grep -q "200\|301\|302"; then
    echo "HTTP/HTTPS: PASS" | sudo tee -a /root/documentation/network.txt
else
    echo "HTTP/HTTPS: FAIL" | sudo tee -a /root/documentation/network.txt
fi
```

### Why This Matters

Connectivity tests:
- Verify the network is functioning
- Establish a baseline
- Help identify issues quickly
- Confirm DNS and routing work

---

## Task 5.3: Configure UFW Firewall

### Commands

```bash
# Check initial UFW status
sudo ufw status

# Reset UFW to defaults (clean slate)
sudo ufw --force reset

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH (CRITICAL - do this before enabling!)
sudo ufw allow ssh
# Or explicitly: sudo ufw allow 22/tcp

# Allow HTTP for web development
sudo ufw allow 80/tcp

# Allow HTTPS
sudo ufw allow 443/tcp

# Enable UFW
sudo ufw --force enable

# Check status
sudo ufw status verbose

# Document firewall rules
cat << 'EOF' | sudo tee -a /root/documentation/network.txt

=== Firewall Configuration ===
EOF

sudo ufw status verbose | sudo tee -a /root/documentation/network.txt

cat << 'EOF' | sudo tee -a /root/documentation/network.txt

=== Firewall Rules Explanation ===
- Default deny incoming: Block all unsolicited connections
- Default allow outgoing: Allow server to reach internet
- SSH (22/tcp): Remote administration
- HTTP (80/tcp): Web development/testing
- HTTPS (443/tcp): Secure web traffic
EOF
```

### Expected Output

```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
443/tcp (v6)               ALLOW IN    Anywhere (v6)
```

### Why This Matters

Firewall configuration:
- Blocks unauthorized access attempts
- Reduces attack surface
- Required for compliance
- First line of defense

---

# Phase 6: Automation Script

## Task 6.1: Create System Health Check Script

### Complete Script

```bash
# Create the health check script
sudo tee /home/projects/scripts/health-check.sh << 'SCRIPT'
#!/bin/bash
#================================================================
# TechStartup GmbH - System Health Check Script
#================================================================
# Description: Comprehensive system health monitoring
# Author: Junior Sysadmin
# Version: 1.0
# Schedule: Daily at 7:00 AM via cron
#================================================================

# Configuration
REPORT_DIR="/var/log/company"
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
REPORT_FILE="$REPORT_DIR/health-check-$TIMESTAMP.txt"
DISK_THRESHOLD=80
MEMORY_THRESHOLD=80

# Colors for terminal output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

#================================================================
# Functions
#================================================================

print_header() {
    echo "╔════════════════════════════════════════════════════════════╗"
    echo "║          TechStartup GmbH - System Health Check            ║"
    echo "╠════════════════════════════════════════════════════════════╣"
    echo "║  Date: $(date '+%Y-%m-%d %H:%M:%S')                             ║"
    echo "║  Hostname: $(hostname)                                      "
    echo "╚════════════════════════════════════════════════════════════╝"
    echo ""
}

check_disk_usage() {
    echo "=== Disk Usage Check ==="
    echo ""
    
    # Get disk usage for root partition
    DISK_USAGE=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
    
    if [ "$DISK_USAGE" -ge "$DISK_THRESHOLD" ]; then
        echo -e "${RED}[WARNING]${NC} Disk usage is at ${DISK_USAGE}% (threshold: ${DISK_THRESHOLD}%)"
        echo "[WARNING] Disk usage is at ${DISK_USAGE}% (threshold: ${DISK_THRESHOLD}%)" >> "$REPORT_FILE.alerts"
    else
        echo -e "${GREEN}[OK]${NC} Disk usage is at ${DISK_USAGE}%"
    fi
    
    echo ""
    echo "Detailed disk usage:"
    df -h
    echo ""
}

check_memory_usage() {
    echo "=== Memory Usage Check ==="
    echo ""
    
    # Get memory usage percentage
    MEMORY_USAGE=$(free | awk 'NR==2 {printf "%.0f", $3*100/$2}')
    
    if [ "$MEMORY_USAGE" -ge "$MEMORY_THRESHOLD" ]; then
        echo -e "${RED}[WARNING]${NC} Memory usage is at ${MEMORY_USAGE}% (threshold: ${MEMORY_THRESHOLD}%)"
        echo "[WARNING] Memory usage is at ${MEMORY_USAGE}% (threshold: ${MEMORY_THRESHOLD}%)" >> "$REPORT_FILE.alerts"
    else
        echo -e "${GREEN}[OK]${NC} Memory usage is at ${MEMORY_USAGE}%"
    fi
    
    echo ""
    echo "Detailed memory info:"
    free -h
    echo ""
}

check_running_services() {
    echo "=== Critical Services Check ==="
    echo ""
    
    # Check SSH
    if systemctl is-active --quiet ssh 2>/dev/null; then
        echo -e "${GREEN}[OK]${NC} SSH service is running"
    else
        echo -e "${RED}[CRITICAL]${NC} SSH service is NOT running!"
        echo "[CRITICAL] SSH service is NOT running!" >> "$REPORT_FILE.alerts"
    fi
    
    # Check cron
    if systemctl is-active --quiet cron 2>/dev/null; then
        echo -e "${GREEN}[OK]${NC} Cron service is running"
    else
        echo -e "${YELLOW}[WARNING]${NC} Cron service is not running"
        echo "[WARNING] Cron service is not running" >> "$REPORT_FILE.alerts"
    fi

    # Check UFW
    if sudo ufw status | grep -q "Status: active"; then
        echo -e "${GREEN}[OK]${NC} UFW firewall is active"
    else
        echo -e "${YELLOW}[WARNING]${NC} UFW firewall is not active"
        echo "[WARNING] UFW firewall is not active" >> "$REPORT_FILE.alerts"
    fi
    
    echo ""
    echo "Running services summary:"
    systemctl list-units --type=service --state=running --no-pager | head -10
    echo ""
}

check_network_connectivity() {
    echo "=== Network Connectivity Check ==="
    echo ""
    
    # Check DNS resolution
    if ping -c 1 -W 5 google.com &> /dev/null; then
        echo -e "${GREEN}[OK]${NC} DNS resolution working (google.com reachable)"
    else
        echo -e "${RED}[CRITICAL]${NC} DNS resolution failed!"
        echo "[CRITICAL] DNS resolution failed!" >> "$REPORT_FILE.alerts"
    fi
    
    # Check gateway
    GATEWAY=$(ip route | grep default | awk '{print $3}' | head -1)
    if [ -n "$GATEWAY" ]; then
        if ping -c 1 -W 5 "$GATEWAY" &> /dev/null; then
            echo -e "${GREEN}[OK]${NC} Gateway ($GATEWAY) is reachable"
        else
            echo -e "${RED}[CRITICAL]${NC} Gateway ($GATEWAY) is NOT reachable!"
            echo "[CRITICAL] Gateway ($GATEWAY) is NOT reachable!" >> "$REPORT_FILE.alerts"
        fi
    else
        echo -e "${RED}[CRITICAL]${NC} No default gateway found!"
        echo "[CRITICAL] No default gateway found!" >> "$REPORT_FILE.alerts"
    fi
    
    echo ""
    echo "Network interfaces:"
    ip -brief addr
    echo ""
}

check_last_logins() {
    echo "=== Login Information ==="
    echo ""
    
    echo "Last 5 successful logins:"
    last -n 5 2>/dev/null || echo "No login data available"
    echo ""
    
    echo "Failed login attempts (last 5):"
    sudo lastb -n 5 2>/dev/null || echo "No failed login data available"
    echo ""
}

check_security_updates() {
    echo "=== Security Updates Check ==="
    echo ""
    
    # Update package lists quietly
    sudo apt update -qq 2>/dev/null
    
    # Count upgradable packages
    UPGRADABLE=$(apt list --upgradable 2>/dev/null | grep -c upgradable || echo "0")
    
    if [ "$UPGRADABLE" -gt 0 ]; then
        echo -e "${YELLOW}[INFO]${NC} $UPGRADABLE package(s) can be upgraded"
        echo "[INFO] $UPGRADABLE package(s) can be upgraded" >> "$REPORT_FILE.alerts"
        echo ""
        echo "Upgradable packages:"
        apt list --upgradable 2>/dev/null | head -10
    else
        echo -e "${GREEN}[OK]${NC} System is up to date"
    fi
    echo ""
}

check_system_load() {
    echo "=== System Load ==="
    echo ""
    
    echo "Current load average:"
    uptime
    echo ""
    
    # Check if load is high (simple check: load > number of CPUs)
    LOAD=$(uptime | awk -F'load average:' '{print $2}' | awk -F',' '{print $1}' | tr -d ' ')
    CPUS=$(nproc)
    
    # Convert to integer for comparison (multiply by 100)
    LOAD_INT=$(echo "$LOAD * 100" | bc 2>/dev/null | cut -d'.' -f1 || echo "0")
    CPUS_INT=$((CPUS * 100))
    
    if [ "$LOAD_INT" -gt "$CPUS_INT" ]; then
        echo -e "${YELLOW}[WARNING]${NC} System load ($LOAD) exceeds CPU count ($CPUS)"
    fi
    
    echo "Top 5 processes by CPU:"
    ps aux --sort=-%cpu | head -6
    echo ""
    
    echo "Top 5 processes by Memory:"
    ps aux --sort=-%mem | head -6
    echo ""
}

generate_summary() {
    echo "╔════════════════════════════════════════════════════════════╗"
    echo "║                    Health Check Summary                    ║"
    echo "╚════════════════════════════════════════════════════════════╝"
    echo ""
    
    # Count alerts
    if [ -f "$REPORT_FILE.alerts" ]; then
        ALERT_COUNT=$(wc -l < "$REPORT_FILE.alerts")
        echo "Alerts generated: $ALERT_COUNT"
        echo ""
        echo "Alert details:"
        cat "$REPORT_FILE.alerts"
        rm "$REPORT_FILE.alerts"
    else
        echo "Alerts generated: 0"
        echo "All systems operating normally."
    fi
    
    echo ""
    echo "Report saved to: $REPORT_FILE"
    echo "Script completed at: $(date)"
}

#================================================================
# Main Script
#================================================================

# Ensure report directory exists
mkdir -p "$REPORT_DIR"

# Clear any previous alerts file
rm -f "$REPORT_FILE.alerts"

# Run all checks and save to report file
{
    print_header
    check_disk_usage
    check_memory_usage
    check_system_load
    check_running_services
    check_network_connectivity
    check_last_logins
    check_security_updates
    generate_summary
} 2>&1 | tee "$REPORT_FILE"

# Final message
echo ""
echo "Health check complete. Full report: $REPORT_FILE"
SCRIPT

# Make executable
sudo chmod +x /home/projects/scripts/health-check.sh

# Set ownership
sudo chown root:leads /home/projects/scripts/health-check.sh

# Test the script
sudo /home/projects/scripts/health-check.sh
```

### Why This Matters

A comprehensive health check script:
- Automates routine monitoring
- Catches problems early
- Reduces manual checking effort
- Provides consistent, reproducible checks
- Generates audit trail

---

## Task 6.2: Schedule Health Check Script

### Commands

```bash
# Add health check to cron (7 AM daily)
(sudo crontab -l 2>/dev/null; echo "0 7 * * * /home/projects/scripts/health-check.sh > /dev/null 2>&1") | sudo crontab -

# Verify all cron jobs
sudo crontab -l
```

### Expected Output

```
0 6 * * * /home/projects/scripts/daily-report.sh
0 7 * * * /home/projects/scripts/health-check.sh > /dev/null 2>&1
```

---

## Task 6.3: Document Your Scripts

### Commands

```bash
# Update scripts README
sudo tee /home/projects/scripts/README.md << 'EOF'
# TechStartup GmbH - System Administration Scripts

## Overview
This directory contains system administration and automation scripts
for the TechStartup GmbH development server.

## Scripts

### daily-report.sh
- **Purpose:** Generates daily system status report
- **Schedule:** Daily at 6:00 AM via cron
- **Output:** `/var/log/company/daily-report-YYYYMMDD.txt`
- **Run manually:** `sudo /home/projects/scripts/daily-report.sh`

**What it reports:**
- Disk usage
- Memory usage
- Top processes (CPU and memory)
- Running services
- Recent logins
- System uptime

### health-check.sh
- **Purpose:** Comprehensive system health monitoring with alerting
- **Schedule:** Daily at 7:00 AM via cron
- **Output:** `/var/log/company/health-check-TIMESTAMP.txt`
- **Run manually:** `sudo /home/projects/scripts/health-check.sh`

**Checks performed:**
| Check | Threshold | Alert Level |
|-------|-----------|-------------|
| Disk usage | 80% | WARNING |
| Memory usage | 80% | WARNING |
| SSH service | Running | CRITICAL |
| Cron service | Running | WARNING |
| UFW firewall | Active | WARNING |
| DNS resolution | Working | CRITICAL |
| Gateway connectivity | Reachable | CRITICAL |
| Package updates | Available | INFO |

## Cron Schedule

```cron
# Daily report at 6:00 AM
0 6 * * * /home/projects/scripts/daily-report.sh

# Health check at 7:00 AM
0 7 * * * /home/projects/scripts/health-check.sh > /dev/null 2>&1
```

## Permissions

| Script | Owner | Group | Mode |
|--------|-------|-------|------|
| daily-report.sh | root | leads | 755 |
| health-check.sh | root | leads | 755 |

## Log Files Location

All output files are stored in `/var/log/company/`:
- `daily-report-YYYYMMDD.txt` - Daily reports
- `health-check-TIMESTAMP.txt` - Health check reports

## Maintenance

### Regular Tasks
- Review logs weekly for patterns
- Update thresholds as system usage changes
- Test scripts after system updates
- Archive old logs monthly

### Troubleshooting
- **Script not running:** Check cron service: `systemctl status cron`
- **Permission denied:** Verify script permissions and ownership
- **No output:** Check `/var/log/company/` directory exists

## Future Enhancements
- [ ] Email alerts for critical issues
- [ ] Integration with monitoring system
- [ ] Performance trending graphs
- [ ] Automated remediation for common issues

---
Last updated: $(date)
Author: Junior Sysadmin
EOF
```

---

# Common Issues and Troubleshooting

## Issue: "Permission denied" when creating files

**Cause:** Not using sudo or wrong user

**Solution:**
```bash
# Use sudo for system directories
sudo touch /etc/motd

# Or switch to root
sudo -i
```

## Issue: User can't write to shared directory

**Cause:** Incorrect group membership or permissions

**Solution:**
```bash
# Check user's groups
id username

# Check directory permissions
ls -la /path/to/directory

# Fix group membership
sudo usermod -aG groupname username

# User must log out and back in for group changes to take effect
```

## Issue: Setgid not working

**Cause:** Setgid bit not set properly

**Solution:**
```bash
# Set setgid bit
sudo chmod g+s /path/to/directory

# Verify (should show 's' in group permissions)
ls -la /path/to/directory
```

## Issue: Cron job not running

**Cause:** Various - syntax, permissions, path issues

**Solution:**
```bash
# Check cron service
sudo systemctl status cron

# Check cron syntax
sudo crontab -l

# Test script manually
sudo /path/to/script.sh

# Check cron log
grep CRON /var/log/syslog
```

## Issue: UFW locked me out

**Cause:** Enabled UFW without allowing SSH

**Solution:**
```bash
# If you have console access (VM window):
sudo ufw allow ssh
sudo ufw enable

# Or reset UFW
sudo ufw --force reset
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw --force enable
```

## Issue: Script works manually but not in cron

**Cause:** Different environment in cron

**Solution:**
```bash
# Use full paths in scripts
/usr/bin/df instead of df

# Or set PATH at top of script
#!/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

---

# Evaluation Criteria

## Functionality (40 points)

| Item | Points | Criteria |
|------|--------|----------|
| Users exist with correct groups | 10 | All 3 users, correct group memberships |
| Directories created | 5 | All required directories exist |
| Permissions correct | 10 | Proper chmod, setgid working |
| Services configured | 5 | SSH running and enabled |
| Firewall configured | 5 | UFW active with correct rules |
| Scripts work | 5 | Both scripts execute without errors |

## Security (20 points)

| Item | Points | Criteria |
|------|--------|----------|
| No 777 permissions | 5 | No overly permissive directories |
| Proper group access | 5 | Developers can't write to scripts/ |
| Firewall enabled | 5 | UFW active, deny incoming default |
| SSH secured | 5 | SSH running on port 22, enabled |

## Documentation (20 points)

| Item | Points | Criteria |
|------|--------|----------|
| System documentation | 5 | Complete system-info.txt |
| Network documentation | 5 | IP, routing, DNS documented |
| README files | 5 | All directories have READMEs |
| Script documentation | 5 | Scripts README comprehensive |

## Code Quality (10 points)

| Item | Points | Criteria |
|------|--------|----------|
| Script readability | 3 | Clear structure, formatting |
| Comments | 3 | Purpose and usage documented |
| Error handling | 2 | Scripts handle edge cases |
| Naming conventions | 2 | Consistent, descriptive names |

## Completeness (10 points)

| Item | Points | Criteria |
|------|--------|----------|
| All phases completed | 5 | No skipped phases |
| Cron jobs configured | 3 | Both scripts scheduled |
| Tests performed | 2 | Permission tests documented |

---

# Final Checklist

Before considering your project complete:

- [ ] All users exist and can log in
- [ ] Groups have correct members
- [ ] Shared directories work for collaboration
- [ ] Permission tests pass
- [ ] All packages installed
- [ ] MOTD displays on login
- [ ] SSH service running and enabled
- [ ] Both scripts work correctly
- [ ] Cron jobs scheduled
- [ ] UFW firewall active
- [ ] All documentation files exist
- [ ] README files are informative

---

## Congratulations!

If you've completed all tasks, you have successfully demonstrated:

1. **Linux administration skills** - Users, groups, permissions
2. **System configuration** - Packages, services, networking
3. **Security awareness** - Firewall, access control
4. **Automation** - Scripting, cron scheduling
5. **Documentation** - Professional system documentation

These are real skills used by professional system administrators every day. Well done!

---

*This solution guide was created for the Linux Fundamentals Capstone Project.*