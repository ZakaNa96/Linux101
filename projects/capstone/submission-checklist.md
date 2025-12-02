# ✅ Capstone Project Submission Checklist

Use this checklist to verify you have completed all required tasks before considering your project finished.

---

## How to Use This Checklist

1. Go through each item carefully
2. Verify each item by running the suggested verification command
3. Mark items as complete when verified
4. If any item fails, go back to the relevant phase in `tasks.md`

---

## 📋 Phase 1: Initial System Setup

### System Documentation
- [ ] Documentation directory exists at `/root/documentation/`
  ```bash
  ls -la /root/documentation/
  ```

- [ ] System information file exists and contains required data
  ```bash
  cat /root/documentation/system-info.txt
  ```
  Should include: hostname, OS version, kernel, CPU info, memory, disk space

### Directory Structure
- [ ] `/home/projects/` directory exists
  ```bash
  ls -la /home/projects/
  ```

- [ ] `/home/projects/webapp/` directory exists
  ```bash
  ls -la /home/projects/webapp/
  ```

- [ ] `/home/projects/scripts/` directory exists
  ```bash
  ls -la /home/projects/scripts/
  ```

- [ ] `/var/log/company/` directory exists
  ```bash
  ls -la /var/log/company/
  ```

### README Files
- [ ] README exists in `/home/projects/`
  ```bash
  cat /home/projects/README.md
  ```

- [ ] README exists in `/home/projects/webapp/`
  ```bash
  cat /home/projects/webapp/README.md
  ```

- [ ] README exists in `/home/projects/scripts/`
  ```bash
  cat /home/projects/scripts/README.md
  ```

- [ ] README exists in `/var/log/company/`
  ```bash
  cat /var/log/company/README.md
  ```

---

## 📋 Phase 2: User and Access Management

### Groups
- [ ] Group `developers` exists
  ```bash
  getent group developers
  ```

- [ ] Group `leads` exists
  ```bash
  getent group leads
  ```

### Users
- [ ] User `developer1` exists with correct groups
  ```bash
  id developer1
  # Should show: groups=...(developers)
  ```

- [ ] User `developer2` exists with correct groups
  ```bash
  id developer2
  # Should show: groups=...(developers)
  ```

- [ ] User `teamlead` exists with correct groups
  ```bash
  id teamlead
  # Should show: groups=...(developers),...(leads)
  ```

### Home Directories
- [ ] `/home/developer1/` exists and has correct ownership
  ```bash
  ls -la /home/ | grep developer1
  ```

- [ ] `/home/developer2/` exists and has correct ownership
  ```bash
  ls -la /home/ | grep developer2
  ```

- [ ] `/home/teamlead/` exists and has correct ownership
  ```bash
  ls -la /home/ | grep teamlead
  ```

- [ ] Welcome files exist in each home directory
  ```bash
  cat /home/developer1/welcome.txt
  cat /home/developer2/welcome.txt
  cat /home/teamlead/welcome.txt
  ```

### Permissions
- [ ] `/home/projects/` has correct permissions (2775 or similar, group=developers)
  ```bash
  ls -la /home/ | grep projects
  stat /home/projects/
  ```

- [ ] `/home/projects/webapp/` has correct permissions (2775, group=developers)
  ```bash
  stat /home/projects/webapp/
  ```

- [ ] `/home/projects/scripts/` has correct permissions (2755, group=leads)
  ```bash
  stat /home/projects/scripts/
  ```

- [ ] Setgid bit is set on shared directories
  ```bash
  ls -la /home/projects/
  # Should show 's' in group permissions (drwxrwsr-x)
  ```

### Permission Tests
- [ ] Developer can write to webapp directory
  ```bash
  sudo -u developer1 touch /home/projects/webapp/test.txt && echo "PASS" && sudo rm /home/projects/webapp/test.txt
  ```

- [ ] Developer CANNOT write to scripts directory
  ```bash
  sudo -u developer1 touch /home/projects/scripts/test.txt 2>&1 | grep -q "Permission denied" && echo "PASS (correctly denied)"
  ```

- [ ] Teamlead CAN write to scripts directory
  ```bash
  sudo -u teamlead touch /home/projects/scripts/test.txt && echo "PASS" && sudo rm /home/projects/scripts/test.txt
  ```

---

## 📋 Phase 3: System Configuration

### Package Updates
- [ ] System packages are updated (or documented)
  ```bash
  apt list --upgradable 2>/dev/null | wc -l
  ```

### Development Tools Installed
- [ ] git is installed
  ```bash
  git --version
  ```

- [ ] vim is installed
  ```bash
  vim --version | head -1
  ```

- [ ] curl is installed
  ```bash
  curl --version | head -1
  ```

- [ ] wget is installed
  ```bash
  wget --version | head -1
  ```

- [ ] htop is installed
  ```bash
  htop --version
  ```

- [ ] tree is installed
  ```bash
  tree --version
  ```

- [ ] ufw is installed
  ```bash
  ufw version
  ```

- [ ] net-tools is installed
  ```bash
  ifconfig --version 2>&1 | head -1
  ```

### Package Documentation
- [ ] Package list documented
  ```bash
  ls -la /root/documentation/installed-packages.txt
  ```

- [ ] Dev tools documented
  ```bash
  cat /root/documentation/dev-tools.txt
  ```

### MOTD
- [ ] MOTD is configured with company message
  ```bash
  cat /etc/motd
  ```

---

## 📋 Phase 4: Services and Automation

### Services Documentation
- [ ] Services documented in services.txt
  ```bash
  cat /root/documentation/services.txt
  ```

### SSH Service
- [ ] SSH service is running
  ```bash
  systemctl is-active ssh
  ```

- [ ] SSH service is enabled (starts at boot)
  ```bash
  systemctl is-enabled ssh
  ```

- [ ] SSH is listening on port 22
  ```bash
  ss -tlnp | grep :22
  ```

### Daily Report Script
- [ ] Daily report script exists
  ```bash
  ls -la /home/projects/scripts/daily-report.sh
  ```

- [ ] Daily report script is executable
  ```bash
  test -x /home/projects/scripts/daily-report.sh && echo "EXECUTABLE"
  ```

- [ ] Daily report script runs without errors
  ```bash
  sudo /home/projects/scripts/daily-report.sh
  ```

- [ ] Daily report generates output file
  ```bash
  ls -la /var/log/company/daily-report-*.txt | tail -1
  ```

### Cron Jobs
- [ ] Daily report cron job is scheduled
  ```bash
  sudo crontab -l | grep daily-report
  ```

- [ ] Cron syntax is correct (runs at 6 AM)
  ```bash
  sudo crontab -l | grep "0 6"
  ```

---

## 📋 Phase 5: Network and Security

### Network Documentation
- [ ] Network configuration documented
  ```bash
  cat /root/documentation/network.txt
  ```

- [ ] IP addresses documented
  ```bash
  grep -A 20 "IP Addresses" /root/documentation/network.txt
  ```

- [ ] Listening ports documented
  ```bash
  grep -A 10 "Listening Ports" /root/documentation/network.txt
  ```

### Connectivity
- [ ] DNS resolution works
  ```bash
  ping -c 1 google.com && echo "PASS"
  ```

- [ ] Gateway is reachable
  ```bash
  ping -c 1 $(ip route | grep default | awk '{print $3}') && echo "PASS"
  ```

### Firewall
- [ ] UFW is enabled
  ```bash
  sudo ufw status | grep -q "Status: active" && echo "ENABLED"
  ```

- [ ] SSH is allowed
  ```bash
  sudo ufw status | grep -E "22|ssh"
  ```

- [ ] HTTP (80) is allowed
  ```bash
  sudo ufw status | grep "80"
  ```

- [ ] HTTPS (443) is allowed
  ```bash
  sudo ufw status | grep "443"
  ```

- [ ] Default incoming policy is deny
  ```bash
  sudo ufw status verbose | grep "Default: deny"
  ```

---

## 📋 Phase 6: Automation Script

### Health Check Script
- [ ] Health check script exists
  ```bash
  ls -la /home/projects/scripts/health-check.sh
  ```

- [ ] Health check script is executable
  ```bash
  test -x /home/projects/scripts/health-check.sh && echo "EXECUTABLE"
  ```

- [ ] Script has proper shebang line
  ```bash
  head -1 /home/projects/scripts/health-check.sh
  # Should show: #!/bin/bash
  ```

- [ ] Script runs without errors
  ```bash
  sudo /home/projects/scripts/health-check.sh
  ```

### Script Functionality
- [ ] Script checks disk usage
  ```bash
  grep -q "disk" /home/projects/scripts/health-check.sh && echo "PASS"
  ```

- [ ] Script checks memory usage
  ```bash
  grep -q "memory\|Memory" /home/projects/scripts/health-check.sh && echo "PASS"
  ```

- [ ] Script checks running services
  ```bash
  grep -q "systemctl\|service" /home/projects/scripts/health-check.sh && echo "PASS"
  ```

- [ ] Script checks network connectivity
  ```bash
  grep -q "ping\|network" /home/projects/scripts/health-check.sh && echo "PASS"
  ```

- [ ] Script checks login information
  ```bash
  grep -q "last\|login" /home/projects/scripts/health-check.sh && echo "PASS"
  ```

- [ ] Script generates report file
  ```bash
  ls -la /var/log/company/health-check-*.txt | tail -1
  ```

### Health Check Cron Job
- [ ] Health check cron job is scheduled
  ```bash
  sudo crontab -l | grep health-check
  ```

- [ ] Cron syntax is correct (runs at 7 AM)
  ```bash
  sudo crontab -l | grep "0 7"
  ```

---

## 📋 Documentation Completeness

### /root/documentation/
- [ ] system-info.txt exists and has content
  ```bash
  wc -l /root/documentation/system-info.txt
  ```

- [ ] installed-packages.txt exists
  ```bash
  wc -l /root/documentation/installed-packages.txt
  ```

- [ ] dev-tools.txt exists
  ```bash
  wc -l /root/documentation/dev-tools.txt
  ```

- [ ] services.txt exists
  ```bash
  wc -l /root/documentation/services.txt
  ```

- [ ] network.txt exists
  ```bash
  wc -l /root/documentation/network.txt
  ```

### Script Documentation
- [ ] Scripts README is comprehensive
  ```bash
  wc -l /home/projects/scripts/README.md
  # Should be substantial (20+ lines)
  ```

---

## 📋 Bonus Tasks (Optional)

### Bonus 1: User Onboarding Script
- [ ] Script exists at `/home/projects/scripts/onboard-user.sh`
- [ ] Script accepts username as parameter
- [ ] Script creates user with proper groups
- [ ] Script is documented

### Bonus 2: SSH Key Authentication
- [ ] SSH key pair generated
- [ ] Public key added to authorized_keys for teamlead
- [ ] Key-based login tested
- [ ] Process documented

### Bonus 3: Backup Script
- [ ] Backup script exists
- [ ] Creates compressed archive
- [ ] Includes timestamp in filename
- [ ] Manages backup rotation
- [ ] Logs activity

---

## 🎯 Final Verification

Run this comprehensive verification script to check everything at once:

```bash
#!/bin/bash
echo "=== Capstone Project Verification ==="
echo ""

PASS=0
FAIL=0

check() {
    if eval "$2" > /dev/null 2>&1; then
        echo "[✓] $1"
        ((PASS++))
    else
        echo "[✗] $1"
        ((FAIL++))
    fi
}

echo "--- Phase 1: System Setup ---"
check "Documentation directory exists" "test -d /root/documentation"
check "System info file exists" "test -f /root/documentation/system-info.txt"
check "/home/projects exists" "test -d /home/projects"
check "/home/projects/webapp exists" "test -d /home/projects/webapp"
check "/home/projects/scripts exists" "test -d /home/projects/scripts"
check "/var/log/company exists" "test -d /var/log/company"

echo ""
echo "--- Phase 2: Users and Groups ---"
check "Group developers exists" "getent group developers"
check "Group leads exists" "getent group leads"
check "User developer1 exists" "id developer1"
check "User developer2 exists" "id developer2"
check "User teamlead exists" "id teamlead"

echo ""
echo "--- Phase 3: Packages ---"
check "git installed" "which git"
check "vim installed" "which vim"
check "curl installed" "which curl"
check "ufw installed" "which ufw"

echo ""
echo "--- Phase 4: Services ---"
check "SSH running" "systemctl is-active ssh"
check "Daily report script exists" "test -x /home/projects/scripts/daily-report.sh"
check "Cron job configured" "sudo crontab -l | grep -q daily-report"

echo ""
echo "--- Phase 5: Network/Security ---"
check "UFW enabled" "sudo ufw status | grep -q 'Status: active'"
check "Network documentation exists" "test -f /root/documentation/network.txt"

echo ""
echo "--- Phase 6: Automation ---"
check "Health check script exists" "test -x /home/projects/scripts/health-check.sh"
check "Health check cron job" "sudo crontab -l | grep -q health-check"

echo ""
echo "================================"
echo "Results: $PASS passed, $FAIL failed"
echo "================================"

if [ $FAIL -eq 0 ]; then
    echo "🎉 Congratulations! All checks passed!"
else
    echo "⚠️  Some checks failed. Review tasks.md for missing items."
fi
```

Save this as `/tmp/verify-capstone.sh`, make it executable with `chmod +x /tmp/verify-capstone.sh`, and run it.

---

## 📊 Scoring Guide

| Category | Points | Your Score |
|----------|--------|------------|
| Phase 1: System Setup | 15 | ___ / 15 |
| Phase 2: Users & Groups | 25 | ___ / 25 |
| Phase 3: Configuration | 15 | ___ / 15 |
| Phase 4: Services | 15 | ___ / 15 |
| Phase 5: Network/Security | 15 | ___ / 15 |
| Phase 6: Automation | 15 | ___ / 15 |
| **Total** | **100** | ___ / 100 |

### Scoring Criteria

- **90-100**: Excellent - Ready for production sysadmin work
- **80-89**: Good - Minor improvements needed
- **70-79**: Satisfactory - Review weak areas
- **60-69**: Needs Improvement - Re-do failed sections
- **Below 60**: Incomplete - Start from beginning with solution guide

---

## 📝 Self-Assessment Questions

Before submitting, ask yourself:

1. **Could another sysadmin take over this server with only my documentation?**
2. **Are all passwords secure enough for a production environment?**
3. **Would the automation catch problems before they become critical?**
4. **Is the permission structure actually secure?**
5. **Did I test everything, not just assume it works?**

---

## 🏁 Ready to Submit?

If all mandatory checkboxes are marked, you have successfully completed the Capstone Project!

**Next steps:**
1. Take a final VM snapshot named "Capstone-Complete"
2. Review your documentation one more time
3. Compare your solution with `solution/solution-guide.md`
4. Celebrate your accomplishment! 🎉

---

*Congratulations on completing the Linux Fundamentals Capstone Project!*