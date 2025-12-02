# 📝 Capstone Project Tasks

## Your Mission

You are a new junior system administrator at **TechStartup GmbH**. Your team lead has given you access to a fresh Linux server and the following requirements. Complete each phase in order, documenting your work as you go.

---

## 🎬 The Scenario

*It's Monday morning at TechStartup GmbH. You arrive at your desk to find this email:*

> **From:** Thomas Weber (IT Team Lead)  
> **To:** You (Junior Sysadmin)  
> **Subject:** Server Setup for Development Team
>
> Guten Morgen!
>
> Welcome to TechStartup GmbH! As discussed in your onboarding, we need you to set up our new development server. The server (your Kali VM) is freshly installed and waiting for configuration.
>
> Here's what we need:
> - A proper directory structure for our projects
> - User accounts for our three team members joining next month
> - Proper access controls so developers can collaborate
> - Basic security hardening
> - Automated monitoring and maintenance
> - Complete documentation of everything
>
> I've attached detailed requirements below. Please complete this by Friday and document everything you do. If you have questions, check our internal wiki first (the course materials).
>
> Viel Erfolg!
> Thomas

---

# Phase 1: Initial System Setup
**Modules Used:** 1, 2, 3  
**Estimated Time:** 30-45 minutes

## Context
Before doing anything else, a good sysadmin documents the current state of the system. This helps with troubleshooting and provides a baseline for future reference.

---

### Task 1.1: Document System Information

**Objective:** Create a system documentation file with essential information about the server.

**Steps:**

1. Create a documentation directory:
   ```bash
   sudo mkdir -p /root/documentation
   ```

2. Create a system information file (`/root/documentation/system-info.txt`) containing:
   - Current date and time
   - Hostname
   - Operating system version
   - Kernel version
   - CPU information
   - Total memory
   - Disk space available
   - Current user

**Hints:**
- Use commands like `hostname`, `cat /etc/os-release`, `uname -r`, `lscpu`, `free -h`, `df -h`, `whoami`
- Use output redirection (`>` and `>>`) to save to file

**Why This Matters:**  
*Every professional environment requires documentation. When something goes wrong at 3 AM, you'll be glad you documented the baseline system state. This is also often required for compliance and auditing.*

---

### Task 1.2: Explore the Filesystem

**Objective:** Familiarize yourself with the system and understand its current state.

**Steps:**

1. Check the current directory structure in `/home`
2. List what's in `/var/log`
3. Check `/etc` for configuration files
4. Identify how much space is used in key directories

**Commands to explore:**
```bash
ls -la /home
ls -la /var/log
ls -la /etc | head -20
du -sh /var/*
```

**Document your findings** by appending notable observations to your system-info.txt file.

**Why This Matters:**  
*Understanding what already exists prevents you from accidentally overwriting important files and helps you plan where to put new things.*

---

### Task 1.3: Create Project Directory Structure

**Objective:** Set up a proper directory structure for the development team.

**Required Structure:**
```
/home/projects/
├── README.md
├── webapp/
│   └── README.md
└── scripts/
    └── README.md

/var/log/company/
└── README.md
```

**Steps:**

1. Create the main project directories:
   ```bash
   sudo mkdir -p /home/projects/webapp
   sudo mkdir -p /home/projects/scripts
   sudo mkdir -p /var/log/company
   ```

2. Create README.md files in each directory explaining its purpose:

   **/home/projects/README.md** should contain:
   - Directory purpose (main project storage)
   - Who should have access
   - Subdirectory descriptions
   - Date created and by whom

   **/home/projects/webapp/README.md** should contain:
   - Purpose: Web application code storage
   - Intended users: Development team

   **/home/projects/scripts/README.md** should contain:
   - Purpose: System administration and automation scripts
   - Intended users: Sysadmin and team lead

   **/var/log/company/README.md** should contain:
   - Purpose: Company-specific application logs
   - Log rotation policy (to be implemented)

**Why This Matters:**  
*Clear directory organization makes it easy for team members to find things. README files ensure anyone who comes after you understands the purpose of each directory.*

---

### ✅ Phase 1 Checkpoint

Before moving on, verify:
- [ ] System documentation file exists at `/root/documentation/system-info.txt`
- [ ] All required directories are created
- [ ] Each directory has a README.md file
- [ ] README files contain useful information

---

# Phase 2: User and Access Management
**Modules Used:** 5, 6  
**Estimated Time:** 45-60 minutes

## Context
The development team needs user accounts. TechStartup GmbH has the following team members joining:

| Name | Role | Username |
|------|------|----------|
| Anna Schmidt | Developer | developer1 |
| Max Müller | Developer | developer2 |
| Lisa Fischer | Team Lead | teamlead |

---

### Task 2.1: Create Groups

**Objective:** Set up groups for organizing users and managing permissions.

**Required Groups:**
- `developers` - For all development team members
- `leads` - For team leads only (access to sensitive files)

**Steps:**

1. Create the groups:
   ```bash
   sudo groupadd developers
   sudo groupadd leads
   ```

2. Verify the groups were created:
   ```bash
   cat /etc/group | grep -E "developers|leads"
   ```

**Why This Matters:**  
*Groups simplify permission management. Instead of setting permissions for each user individually, you set them once for the group. When new team members join, you just add them to the appropriate group.*

---

### Task 2.2: Create User Accounts

**Objective:** Create user accounts for each team member with proper configurations.

**Requirements:**
| User | Primary Group | Additional Groups | Shell | Home Dir |
|------|---------------|-------------------|-------|----------|
| developer1 | developer1 | developers | /bin/bash | /home/developer1 |
| developer2 | developer2 | developers | /bin/bash | /home/developer2 |
| teamlead | teamlead | developers, leads | /bin/bash | /home/teamlead |

**Steps:**

1. Create developer1:
   ```bash
   sudo useradd -m -s /bin/bash -G developers developer1
   ```

2. Create developer2:
   ```bash
   sudo useradd -m -s /bin/bash -G developers developer2
   ```

3. Create teamlead with membership in both groups:
   ```bash
   sudo useradd -m -s /bin/bash -G developers,leads teamlead
   ```

4. Set passwords for each user:
   ```bash
   sudo passwd developer1
   sudo passwd developer2
   sudo passwd teamlead
   ```
   *(Use simple passwords for this exercise, like "Linux123!")*

5. Verify users were created correctly:
   ```bash
   id developer1
   id developer2
   id teamlead
   ```

**Expected Output Example:**
```
uid=1001(developer1) gid=1001(developer1) groups=1001(developer1),1002(developers)
```

**Why This Matters:**  
*Proper user management is fundamental to system security. Each person gets their own account for accountability, and group membership determines what they can access.*

---

### Task 2.3: Configure Home Directories

**Objective:** Ensure home directories are properly set up and secured.

**Steps:**

1. Verify home directories exist:
   ```bash
   ls -la /home
   ```

2. Check default permissions on home directories (should be 700 or 755)

3. Create a welcome file in each user's home directory:
   ```bash
   echo "Welcome to TechStartup GmbH, developer1!" | sudo tee /home/developer1/welcome.txt
   echo "Welcome to TechStartup GmbH, developer2!" | sudo tee /home/developer2/welcome.txt
   echo "Welcome to TechStartup GmbH, teamlead!" | sudo tee /home/teamlead/welcome.txt
   ```

4. Set proper ownership:
   ```bash
   sudo chown developer1:developer1 /home/developer1/welcome.txt
   sudo chown developer2:developer2 /home/developer2/welcome.txt
   sudo chown teamlead:teamlead /home/teamlead/welcome.txt
   ```

**Why This Matters:**  
*Each user needs their private space to work. Home directories should be owned by the user and protected from other users by default.*

---

### Task 2.4: Set Up Shared Project Folder

**Objective:** Configure the /home/projects directory so the team can collaborate effectively.

**Requirements:**
- All developers can read and write to `/home/projects`
- All developers can read and write to `/home/projects/webapp`
- Only leads can write to `/home/projects/scripts` (developers can read)
- New files should inherit group ownership

**Steps:**

1. Set ownership and group for project directories:
   ```bash
   sudo chown root:developers /home/projects
   sudo chown root:developers /home/projects/webapp
   sudo chown root:leads /home/projects/scripts
   ```

2. Set permissions:
   ```bash
   # Main projects folder - developers can read/write
   sudo chmod 2775 /home/projects
   
   # Webapp - developers can read/write
   sudo chmod 2775 /home/projects/webapp
   
   # Scripts - leads can write, developers can read
   sudo chmod 2755 /home/projects/scripts
   ```

   > **Note:** The `2` in `2775` sets the setgid bit, which makes new files inherit the group ownership of the directory.

3. Set up the company log directory:
   ```bash
   sudo chown root:leads /var/log/company
   sudo chmod 2775 /var/log/company
   ```

4. Verify permissions:
   ```bash
   ls -la /home/projects
   ls -la /home/projects/
   ls -la /var/log/
   ```

**Why This Matters:**  
*Collaboration requires shared access, but security requires limits. The setgid bit ensures files created in shared directories maintain proper group ownership, preventing "permission denied" errors when teammates try to access each other's files.*

---

### Task 2.5: Test Permissions

**Objective:** Verify that permissions work as expected.

**Testing Steps:**

1. Test as developer1:
   ```bash
   sudo -u developer1 touch /home/projects/webapp/test-dev1.txt
   sudo -u developer1 ls -la /home/projects/webapp/
   ```
   ✓ Should succeed

2. Test developer1 writing to scripts (should fail for write):
   ```bash
   sudo -u developer1 touch /home/projects/scripts/test-dev1.txt
   ```
   ✗ Should fail (Permission denied)

3. Test teamlead writing to scripts (should succeed):
   ```bash
   sudo -u teamlead touch /home/projects/scripts/test-lead.txt
   ```
   ✓ Should succeed

4. Clean up test files:
   ```bash
   sudo rm /home/projects/webapp/test-dev1.txt
   sudo rm /home/projects/scripts/test-lead.txt
   ```

**Document the results** in your system documentation.

**Why This Matters:**  
*Always test permissions after setting them! What you think you configured and what you actually configured can be different. Testing prevents security gaps and access issues.*

---

### ✅ Phase 2 Checkpoint

Before moving on, verify:
- [ ] Groups `developers` and `leads` exist
- [ ] All three users exist with correct group memberships
- [ ] Home directories have correct ownership
- [ ] Project directories have correct permissions
- [ ] Setgid bit is set on shared directories
- [ ] Permission tests pass

---

# Phase 3: System Configuration
**Modules Used:** 4, 7  
**Estimated Time:** 30-45 minutes

## Context
The team needs development tools installed, and the server should be properly configured with company branding.

---

### Task 3.1: Update System Packages

**Objective:** Ensure the system is up to date.

**Steps:**

1. Update package lists:
   ```bash
   sudo apt update
   ```

2. Check for upgradable packages:
   ```bash
   apt list --upgradable
   ```

3. (Optional) Upgrade packages:
   ```bash
   sudo apt upgrade -y
   ```

**Document:** How many packages needed updates? Any notable packages?

**Why This Matters:**  
*Keeping systems updated is crucial for security. Many breaches occur through known vulnerabilities that have patches available but weren't applied.*

---

### Task 3.2: Install Development Tools

**Objective:** Install tools the development team will need.

**Required Packages:**
- `git` - Version control
- `vim` - Text editor
- `curl` - Data transfer
- `wget` - File downloads
- `htop` - Process viewer
- `tree` - Directory visualization
- `ufw` - Firewall management
- `net-tools` - Network utilities (ifconfig, etc.)

**Steps:**

1. Install all packages:
   ```bash
   sudo apt install -y git vim curl wget htop tree ufw net-tools
   ```

2. Verify installations:
   ```bash
   git --version
   vim --version | head -1
   curl --version | head -1
   wget --version | head -1
   htop --version
   tree --version
   ufw version
   ```

3. Document installed versions in your system documentation file.

**Why This Matters:**  
*Developers need tools to work. Having a standard set of tools installed on all servers ensures consistency and allows team members to work efficiently.*

---

### Task 3.3: Create Package Documentation

**Objective:** Document what packages are installed for future reference.

**Steps:**

1. Create a package list file:
   ```bash
   dpkg --get-selections > /root/documentation/installed-packages.txt
   ```

2. Count total installed packages:
   ```bash
   dpkg --get-selections | wc -l
   ```

3. Create a custom list of manually installed packages:
   ```bash
   echo "=== Manually Installed Development Tools ===" > /root/documentation/dev-tools.txt
   echo "Date: $(date)" >> /root/documentation/dev-tools.txt
   echo "" >> /root/documentation/dev-tools.txt
   echo "git - Version Control" >> /root/documentation/dev-tools.txt
   echo "vim - Text Editor" >> /root/documentation/dev-tools.txt
   echo "curl - Data Transfer" >> /root/documentation/dev-tools.txt
   echo "wget - File Downloads" >> /root/documentation/dev-tools.txt
   echo "htop - Process Viewer" >> /root/documentation/dev-tools.txt
   echo "tree - Directory Visualization" >> /root/documentation/dev-tools.txt
   echo "ufw - Firewall" >> /root/documentation/dev-tools.txt
   echo "net-tools - Network Utilities" >> /root/documentation/dev-tools.txt
   ```

**Why This Matters:**  
*Documentation of installed software helps with troubleshooting, security audits, and recreating the environment if needed. When you leave the company, your successor will thank you.*

---

### Task 3.4: Configure Message of the Day (MOTD)

**Objective:** Create a custom login message for users.

**Steps:**

1. Edit the MOTD file:
   ```bash
   sudo nano /etc/motd
   ```

2. Add a professional message:
   ```
   ╔════════════════════════════════════════════════════════════╗
   ║           Welcome to TechStartup GmbH Dev Server           ║
   ╠════════════════════════════════════════════════════════════╣
   ║  This is a company server. All activity is monitored.      ║
   ║  Unauthorized access is prohibited.                        ║
   ║                                                            ║
   ║  For support: admin@techstartup.de                         ║
   ║  Documentation: /root/documentation/                       ║
   ╚════════════════════════════════════════════════════════════╝
   ```

3. Test by logging in as a user:
   ```bash
   sudo -u developer1 bash -l
   ```
   Then type `exit` to return.

**Why This Matters:**  
*MOTD serves both as a welcome message and a legal notice. Warning users that activity is monitored is a legal requirement in many jurisdictions and helps set expectations.*

---

### ✅ Phase 3 Checkpoint

Before moving on, verify:
- [ ] System packages are updated
- [ ] All development tools are installed
- [ ] Package documentation exists
- [ ] MOTD is configured

---

# Phase 4: Services and Automation
**Modules Used:** 8  
**Estimated Time:** 45-60 minutes

## Context
A production server needs properly configured services and automated maintenance tasks.

---

### Task 4.1: Check and Document Running Services

**Objective:** Understand what services are running on the system.

**Steps:**

1. List all running services:
   ```bash
   systemctl list-units --type=service --state=running
   ```

2. Create a services documentation file:
   ```bash
   echo "=== Running Services Documentation ===" > /root/documentation/services.txt
   echo "Date: $(date)" >> /root/documentation/services.txt
   echo "" >> /root/documentation/services.txt
   systemctl list-units --type=service --state=running >> /root/documentation/services.txt
   ```

3. Check for enabled services (start at boot):
   ```bash
   systemctl list-unit-files --type=service --state=enabled
   ```

**Why This Matters:**  
*Knowing what's running helps with security (disable unnecessary services), troubleshooting (what might be causing issues), and capacity planning (what's using resources).*

---

### Task 4.2: Configure SSH Service

**Objective:** Ensure SSH is properly configured for remote access.

**Steps:**

1. Check SSH service status:
   ```bash
   sudo systemctl status ssh
   ```

2. If not running, start and enable it:
   ```bash
   sudo systemctl start ssh
   sudo systemctl enable ssh
   ```

3. Verify SSH is listening:
   ```bash
   sudo ss -tlnp | grep ssh
   ```

4. Document SSH configuration:
   ```bash
   echo "=== SSH Configuration ===" >> /root/documentation/services.txt
   echo "Status: $(systemctl is-active ssh)" >> /root/documentation/services.txt
   echo "Enabled: $(systemctl is-enabled ssh)" >> /root/documentation/services.txt
   echo "Port: $(grep "^Port" /etc/ssh/sshd_config || echo "22 (default)")" >> /root/documentation/services.txt
   ```

**Why This Matters:**  
*SSH is the primary way sysadmins access Linux servers remotely. It must be running and properly secured for remote administration.*

---

### Task 4.3: Create Daily Backup Report Cron Job

**Objective:** Set up automated daily system status reports.

**Steps:**

1. Create the report script:
   ```bash
   sudo nano /home/projects/scripts/daily-report.sh
   ```

2. Add the following content:
   ```bash
   #!/bin/bash
   # Daily System Report Script
   # TechStartup GmbH
   # Created: $(date)
   
   REPORT_DIR="/var/log/company"
   REPORT_FILE="$REPORT_DIR/daily-report-$(date +%Y%m%d).txt"
   
   echo "=== TechStartup GmbH Daily System Report ===" > "$REPORT_FILE"
   echo "Generated: $(date)" >> "$REPORT_FILE"
   echo "" >> "$REPORT_FILE"
   
   echo "=== Disk Usage ===" >> "$REPORT_FILE"
   df -h >> "$REPORT_FILE"
   echo "" >> "$REPORT_FILE"
   
   echo "=== Memory Usage ===" >> "$REPORT_FILE"
   free -h >> "$REPORT_FILE"
   echo "" >> "$REPORT_FILE"
   
   echo "=== Top 5 Processes by Memory ===" >> "$REPORT_FILE"
   ps aux --sort=-%mem | head -6 >> "$REPORT_FILE"
   echo "" >> "$REPORT_FILE"
   
   echo "=== Service Status ===" >> "$REPORT_FILE"
   systemctl list-units --type=service --state=running | head -20 >> "$REPORT_FILE"
   echo "" >> "$REPORT_FILE"
   
   echo "Report saved to $REPORT_FILE"
   ```

3. Make the script executable:
   ```bash
   sudo chmod +x /home/projects/scripts/daily-report.sh
   ```

4. Test the script:
   ```bash
   sudo /home/projects/scripts/daily-report.sh
   cat /var/log/company/daily-report-$(date +%Y%m%d).txt
   ```

5. Set up the cron job:
   ```bash
   sudo crontab -e
   ```
   
   Add this line to run daily at 6 AM:
   ```
   0 6 * * * /home/projects/scripts/daily-report.sh
   ```

6. Verify the cron job:
   ```bash
   sudo crontab -l
   ```

**Why This Matters:**  
*Automated reports help sysadmins catch problems before they become critical. Daily checks of disk space, memory, and services can prevent outages.*

---

### Task 4.4: Log Rotation Awareness

**Objective:** Understand log rotation and document it.

**Steps:**

1. Check logrotate configuration:
   ```bash
   cat /etc/logrotate.conf
   ```

2. Check existing log rotation rules:
   ```bash
   ls /etc/logrotate.d/
   ```

3. Document log rotation in your system documentation:
   ```bash
   echo "=== Log Rotation Configuration ===" >> /root/documentation/services.txt
   echo "" >> /root/documentation/services.txt
   echo "Config file: /etc/logrotate.conf" >> /root/documentation/services.txt
   echo "Rule files: /etc/logrotate.d/" >> /root/documentation/services.txt
   echo "" >> /root/documentation/services.txt
   echo "Note: Company logs in /var/log/company/ may need custom rotation rules" >> /root/documentation/services.txt
   ```

**Why This Matters:**  
*Without log rotation, logs can fill up disk space and crash the server. Understanding the system helps you maintain it properly.*

---

### ✅ Phase 4 Checkpoint

Before moving on, verify:
- [ ] Running services are documented
- [ ] SSH service is running and enabled
- [ ] Daily report script exists and works
- [ ] Cron job is configured
- [ ] Log rotation is documented

---

# Phase 5: Network and Security
**Modules Used:** 9  
**Estimated Time:** 30-45 minutes

## Context
Security is critical. We need to document our network configuration and set up basic firewall rules.

---

### Task 5.1: Document Network Configuration

**Objective:** Create comprehensive network documentation.

**Steps:**

1. Create network documentation file:
   ```bash
   echo "=== Network Configuration Documentation ===" > /root/documentation/network.txt
   echo "Date: $(date)" >> /root/documentation/network.txt
   echo "" >> /root/documentation/network.txt
   ```

2. Document IP addresses:
   ```bash
   echo "=== IP Addresses ===" >> /root/documentation/network.txt
   ip addr >> /root/documentation/network.txt
   echo "" >> /root/documentation/network.txt
   ```

3. Document routing:
   ```bash
   echo "=== Routing Table ===" >> /root/documentation/network.txt
   ip route >> /root/documentation/network.txt
   echo "" >> /root/documentation/network.txt
   ```

4. Document DNS configuration:
   ```bash
   echo "=== DNS Configuration ===" >> /root/documentation/network.txt
   cat /etc/resolv.conf >> /root/documentation/network.txt
   echo "" >> /root/documentation/network.txt
   ```

5. Document listening ports:
   ```bash
   echo "=== Listening Ports ===" >> /root/documentation/network.txt
   sudo ss -tlnp >> /root/documentation/network.txt
   echo "" >> /root/documentation/network.txt
   ```

**Why This Matters:**  
*Network documentation is essential for troubleshooting connectivity issues and understanding the server's communication paths.*

---

### Task 5.2: Test Network Connectivity

**Objective:** Verify the server can communicate with the outside world.

**Steps:**

1. Test DNS resolution:
   ```bash
   ping -c 4 google.com
   ```

2. Test HTTP connectivity:
   ```bash
   curl -I https://www.google.com
   ```

3. Document results:
   ```bash
   echo "=== Connectivity Tests ===" >> /root/documentation/network.txt
   echo "Date: $(date)" >> /root/documentation/network.txt
   echo "DNS Test (google.com): SUCCESS" >> /root/documentation/network.txt
   echo "HTTP Test: SUCCESS" >> /root/documentation/network.txt
   ```

**Why This Matters:**  
*Verifying connectivity ensures the server can reach external resources (updates, APIs, etc.) and helps establish a baseline for troubleshooting.*

---

### Task 5.3: Configure UFW Firewall

**Objective:** Set up basic firewall rules to protect the server.

**Steps:**

1. Check UFW status:
   ```bash
   sudo ufw status
   ```

2. Set default policies (deny incoming, allow outgoing):
   ```bash
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   ```

3. Allow SSH (important - don't lock yourself out!):
   ```bash
   sudo ufw allow ssh
   ```
   or explicitly:
   ```bash
   sudo ufw allow 22/tcp
   ```

4. Allow HTTP and HTTPS (for web development):
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```

5. Enable the firewall:
   ```bash
   sudo ufw enable
   ```
   *Type 'y' to confirm*

6. Verify rules:
   ```bash
   sudo ufw status verbose
   ```

7. Document firewall configuration:
   ```bash
   echo "=== Firewall Configuration ===" >> /root/documentation/network.txt
   sudo ufw status verbose >> /root/documentation/network.txt
   ```

**⚠️ Warning:** If you're accessing the VM via SSH, make sure to allow SSH before enabling the firewall!

**Why This Matters:**  
*Firewalls are the first line of defense against unauthorized access. Only allowing necessary ports reduces the attack surface.*

---

### ✅ Phase 5 Checkpoint

Before moving on, verify:
- [ ] Network configuration is documented
- [ ] Connectivity tests pass
- [ ] UFW is enabled
- [ ] SSH, HTTP, and HTTPS are allowed
- [ ] Default policy is deny incoming

---

# Phase 6: Automation Script
**Modules Used:** 10  
**Estimated Time:** 60-90 minutes

## Context
Your final task is to create a comprehensive system health check script that demonstrates your scripting skills.

---

### Task 6.1: Create System Health Check Script

**Objective:** Create a comprehensive bash script that checks system health and generates a report.

**Steps:**

1. Create the script file:
   ```bash
   sudo nano /home/projects/scripts/health-check.sh
   ```

2. Add the following content:

```bash
#!/bin/bash
#================================================================
# TechStartup GmbH - System Health Check Script
#================================================================
# Description: Comprehensive system health monitoring script
# Author: Junior Sysadmin
# Created: $(date +%Y-%m-%d)
# Version: 1.0
#================================================================

# Configuration
REPORT_DIR="/var/log/company"
REPORT_FILE="$REPORT_DIR/health-check-$(date +%Y%m%d-%H%M%S).txt"
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
    echo "║  Date: $(date '+%Y-%m-%d %H:%M:%S')                            ║"
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
    if systemctl is-active --quiet ssh; then
        echo -e "${GREEN}[OK]${NC} SSH service is running"
    else
        echo -e "${RED}[CRITICAL]${NC} SSH service is NOT running!"
    fi
    
    # Check cron
    if systemctl is-active --quiet cron; then
        echo -e "${GREEN}[OK]${NC} Cron service is running"
    else
        echo -e "${YELLOW}[WARNING]${NC} Cron service is not running"
    fi
    
    echo ""
    echo "All running services:"
    systemctl list-units --type=service --state=running --no-pager | head -15
    echo ""
}

check_network_connectivity() {
    echo "=== Network Connectivity Check ==="
    echo ""
    
    # Check DNS resolution
    if ping -c 1 google.com &> /dev/null; then
        echo -e "${GREEN}[OK]${NC} DNS resolution working (google.com reachable)"
    else
        echo -e "${RED}[CRITICAL]${NC} DNS resolution failed!"
    fi
    
    # Check gateway
    GATEWAY=$(ip route | grep default | awk '{print $3}')
    if ping -c 1 "$GATEWAY" &> /dev/null; then
        echo -e "${GREEN}[OK]${NC} Gateway ($GATEWAY) is reachable"
    else
        echo -e "${RED}[CRITICAL]${NC} Gateway ($GATEWAY) is NOT reachable!"
    fi
    
    echo ""
    echo "Network interfaces:"
    ip -brief addr
    echo ""
}

check_last_logins() {
    echo "=== Last Login Information ==="
    echo ""
    
    echo "Last 5 logins:"
    last -n 5
    echo ""
    
    echo "Failed login attempts (last 10):"
    sudo lastb -n 10 2>/dev/null || echo "No failed login data available"
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
    echo "Disk Usage: ${DISK_USAGE}%"
    echo "Memory Usage: ${MEMORY_USAGE}%"
    echo "Report saved to: $REPORT_FILE"
    echo ""
    echo "Script completed at: $(date)"
}

#================================================================
# Main Script
#================================================================

# Ensure report directory exists
mkdir -p "$REPORT_DIR"

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
} | tee "$REPORT_FILE"

echo ""
echo "Health check complete. Report saved to: $REPORT_FILE"
```

3. Make the script executable:
   ```bash
   sudo chmod +x /home/projects/scripts/health-check.sh
   ```

4. Set proper ownership:
   ```bash
   sudo chown root:leads /home/projects/scripts/health-check.sh
   ```

5. Test the script:
   ```bash
   sudo /home/projects/scripts/health-check.sh
   ```

**Why This Matters:**  
*Automation is the sysadmin's best friend. A well-written health check script can be run manually for troubleshooting or scheduled via cron for proactive monitoring.*

---

### Task 6.2: Schedule Health Check Script

**Objective:** Add the health check script to cron for daily execution.

**Steps:**

1. Edit root's crontab:
   ```bash
   sudo crontab -e
   ```

2. Add a line to run the health check daily at 7 AM:
   ```
   0 7 * * * /home/projects/scripts/health-check.sh > /dev/null 2>&1
   ```

3. Verify all cron jobs:
   ```bash
   sudo crontab -l
   ```

   You should see both the daily report and health check scripts scheduled.

**Why This Matters:**  
*Scheduled health checks catch problems early. Running them in the morning means you can review the report when you start your day.*

---

### Task 6.3: Document Your Scripts

**Objective:** Create documentation for the scripts you've created.

**Steps:**

1. Update the scripts README:
   ```bash
   sudo nano /home/projects/scripts/README.md
   ```

2. Add comprehensive documentation:
   ```markdown
   # System Administration Scripts
   
   ## Overview
   This directory contains system administration scripts for TechStartup GmbH.
   
   ## Scripts
   
   ### daily-report.sh
   - **Purpose:** Generates daily system status report
   - **Schedule:** Daily at 6:00 AM via cron
   - **Output:** /var/log/company/daily-report-YYYYMMDD.txt
   - **Run manually:** `sudo /home/projects/scripts/daily-report.sh`
   
   ### health-check.sh
   - **Purpose:** Comprehensive system health monitoring
   - **Schedule:** Daily at 7:00 AM via cron
   - **Output:** /var/log/company/health-check-TIMESTAMP.txt
   - **Run manually:** `sudo /home/projects/scripts/health-check.sh`
   - **Checks performed:**
     - Disk usage (threshold: 80%)
     - Memory usage (threshold: 80%)
     - Critical services (SSH, cron)
     - Network connectivity
     - Login history
     - Available updates
     - System load
   
   ## Cron Schedule
   ```
   0 6 * * * /home/projects/scripts/daily-report.sh
   0 7 * * * /home/projects/scripts/health-check.sh
   ```
   
   ## Permissions
   - Owner: root
   - Group: leads
   - Mode: 755 (rwxr-xr-x)
   
   ## Maintenance
   - Review logs weekly
   - Update thresholds as needed
   - Test scripts after system updates
   ```

---

### ✅ Phase 6 Checkpoint

Before finishing, verify:
- [ ] Health check script exists and is executable
- [ ] Script runs without errors
- [ ] Report file is generated
- [ ] Script is scheduled in cron
- [ ] Documentation is complete

---

# 🎉 Bonus Tasks (Optional)

These tasks are optional but will give you extra practice:

---

## Bonus 1: User Onboarding Script

Create a script that automates adding new users to the system.

**Requirements:**
- Accept username as parameter
- Create user with proper groups
- Set up home directory
- Create welcome file
- Print summary

**Hints:**
```bash
#!/bin/bash
# Check if username provided
if [ -z "$1" ]; then
    echo "Usage: $0 <username>"
    exit 1
fi
# ... rest of script
```

---

## Bonus 2: SSH Key Authentication

Set up SSH key authentication for the teamlead user.

**Steps:**
1. Generate SSH key pair
2. Add public key to authorized_keys
3. Test login with key
4. Document the process

---

## Bonus 3: Backup Script

Create a script that backs up the `/home/projects` directory.

**Requirements:**
- Create compressed archive
- Add timestamp to filename
- Save to a backup location
- Keep only last 7 backups
- Log backup activity

---

# 📋 Final Documentation

Before declaring your project complete, ensure you have:

1. **System Documentation** (`/root/documentation/`)
   - system-info.txt
   - installed-packages.txt
   - dev-tools.txt
   - services.txt
   - network.txt

2. **Directory README Files**
   - /home/projects/README.md
   - /home/projects/webapp/README.md
   - /home/projects/scripts/README.md
   - /var/log/company/README.md

3. **Working Scripts**
   - daily-report.sh
   - health-check.sh

4. **Configured Cron Jobs**
   - Daily report at 6 AM
   - Health check at 7 AM

---

## 🏁 You're Done!

Congratulations on completing the TechStartup GmbH server setup! Now proceed to `submission-checklist.md` to verify everything is complete.

Remember: The best sysadmins are thorough, document everything, and think about security and automation from day one. You've just demonstrated all of these qualities!

---

*Proceed to `submission-checklist.md` →*