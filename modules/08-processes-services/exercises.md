# Module 8 Exercises: Process and Service Management

## 🎯 Exercise Goals

These exercises will help you:
- View and understand running processes
- Use real-time process monitors (top and htop)
- Manage background and foreground processes
- Control and terminate processes safely
- Manage system services with systemctl
- Create and manage scheduled tasks with cron

---

## Exercise 1: Explore Running Processes

### Objective
Learn to view and understand the processes running on your system.

### Tasks

**1.1 View processes in your current terminal**

```bash
# Basic process list for your terminal session
ps
```

**Questions to answer:**
- How many processes are shown?
- What is the PID of your bash shell?
- What is the TTY column showing?

**1.2 View all processes on the system**

```bash
# List ALL processes with detailed information
ps aux

# Count total number of processes
ps aux | wc -l
```

Write down:
- Total number of processes: ______
- Approximate number of root processes (check USER column)

**1.3 Find your current shell process**

```bash
# Show your shell's PID
echo "My shell PID is: $$"

# Find your bash process in the process list
ps aux | grep "$$"

# Alternative: show specific info about your shell
ps -p $$ -o pid,ppid,user,cmd
```

**1.4 Identify system processes**

```bash
# Find processes owned by root
ps aux | grep "^root" | head -20

# Find the init system (PID 1)
ps -p 1 -o pid,cmd

# Find kernel threads (shown in brackets)
ps aux | grep "\[.*\]" | head -10
```

What is the command running as PID 1?

**1.5 Find processes by name**

```bash
# Find all bash processes
pgrep -l bash

# Find all processes containing "ssh"
pgrep -la ssh

# Count how many bash shells are running
pgrep -c bash

# Find the PID of systemd
pidof systemd
```

**1.6 View process hierarchy**

```bash
# Show processes as a tree
ps auxf | head -40

# Alternative: use pstree (may need to install)
pstree

# Show tree for a specific process
pstree -p $$
```

<details>
<summary>💡 Hints</summary>

- `ps aux` uses BSD-style options (no dash)
- `ps -ef` uses UNIX-style options (with dash)
- `$$` is a special variable containing your shell's PID
- `pgrep` is great for finding processes by name
- Kernel threads appear in square brackets [like-this]

</details>

<details>
<summary>✅ Expected Results</summary>

**1.1 Basic ps:**
```bash
$ ps
    PID TTY          TIME CMD
   1234 pts/0    00:00:00 bash
   5678 pts/0    00:00:00 ps
```
You should see at least bash and ps.

**1.2 Process count:**
```bash
$ ps aux | wc -l
# Typically 150-400 processes depending on your system
```

**1.3 Shell process:**
```bash
$ echo "My shell PID is: $$"
My shell PID is: 1234

$ ps -p $$ -o pid,ppid,user,cmd
    PID    PPID USER     CMD
   1234    1200 kali     bash
```

**1.4 PID 1:**
```bash
$ ps -p 1 -o pid,cmd
    PID CMD
      1 /sbin/init
# Or /lib/systemd/systemd on some systems
```

**1.5 Process finding:**
```bash
$ pgrep -l bash
1234 bash
5678 bash

$ pgrep -c bash
2
```

</details>

---

## Exercise 2: Use top and htop

### Objective
Master real-time process monitoring tools.

### Tasks

**2.1 Launch and explore top**

```bash
# Start top
top
```

While in top, answer these questions:
- What is the system load average (1, 5, 15 minutes)?
- How many total tasks are running?
- What percentage of CPU is idle?
- How much memory is being used?

**2.2 Navigate top**

While top is running, try these keyboard commands:

| Key | Action | What happened? |
|-----|--------|----------------|
| `h` | Help | |
| `M` | Sort by memory | |
| `P` | Sort by CPU | |
| `N` | Sort by PID | |
| `T` | Sort by time | |
| `u` | Filter by user (type your username) | |
| `c` | Toggle full command | |
| `1` | Toggle per-CPU display | |
| `q` | Quit | |

**2.3 Find the most resource-intensive processes**

```bash
# Start top sorted by CPU usage
top

# Press 'P' to ensure CPU sorting
# Note the top 3 CPU-consuming processes:
# 1. _____________
# 2. _____________
# 3. _____________

# Press 'M' to sort by memory
# Note the top 3 memory-consuming processes:
# 1. _____________
# 2. _____________
# 3. _____________
```

**2.4 Try htop (if available)**

```bash
# Check if htop is installed
which htop

# If not installed:
sudo apt install htop

# Launch htop
htop
```

Explore htop features:
- Use arrow keys to scroll through processes
- Press F5 to toggle tree view
- Press F6 to choose sort column
- Press F4 to filter processes
- Press F9 to see kill signals
- Press F10 or q to quit

**2.5 Compare top and htop**

| Feature | top | htop |
|---------|-----|------|
| Color display | | |
| Mouse support | | |
| Scrollable process list | | |
| Easy sorting | | |
| Process search/filter | | |
| Tree view | | |

**2.6 Monitor a specific user's processes**

```bash
# In top: press 'u' then type username
top -u kali

# In htop: press 'u' and select user
htop -u kali
```

<details>
<summary>💡 Hints</summary>

- Load average shows system load (1.0 = 100% of one CPU)
- Values in top update every 3 seconds by default
- htop is generally more user-friendly for interactive use
- Use `top -b` for batch mode (useful in scripts)

</details>

<details>
<summary>✅ Expected Results</summary>

**2.1 Top display:**
```
top - 10:30:00 up 2 days, 3:45, 2 users, load average: 0.15, 0.20, 0.18
Tasks: 234 total,   1 running, 232 sleeping,   0 stopped,   1 zombie
%Cpu(s):  5.2 us,  2.1 sy,  0.0 ni, 92.0 id,  0.5 wa,  0.0 hi,  0.2 si
MiB Mem :   7976.3 total,   2345.2 free,   3456.7 used,   2174.4 buff/cache
```

- Load average: 0.15, 0.20, 0.18 (low load)
- Tasks: 234 total
- CPU idle: 92.0%
- Memory: varies by system

**2.5 Comparison:**

| Feature | top | htop |
|---------|-----|------|
| Color display | Limited | ✅ Full color |
| Mouse support | No | ✅ Yes |
| Scrollable process list | Limited | ✅ Yes |
| Easy sorting | Keys only | ✅ Menu + keys |
| Process search/filter | Limited | ✅ Good |
| Tree view | Yes (F key) | ✅ Yes (F5) |

</details>

---

## Exercise 3: Background Processing

### Objective
Learn to manage foreground and background processes.

### Tasks

**3.1 Start a long-running process in foreground**

```bash
# Start a process that runs for 5 minutes
sleep 300

# Notice you can't type anything - the terminal is blocked
# Press Ctrl+C to stop it
```

**3.2 Start a process in background**

```bash
# Start in background using &
sleep 300 &

# Note the job number and PID shown
# [1] 12345

# Verify you can still use the terminal
echo "I can type!"
```

**3.3 Suspend and resume a process**

```bash
# Start a foreground process
sleep 300

# Press Ctrl+Z to suspend it
# You should see: [1]+  Stopped                 sleep 300

# Check the job status
jobs

# Resume it in the background
bg

# Verify it's running
jobs
```

**3.4 Work with multiple background jobs**

```bash
# Start several background jobs
sleep 100 &
sleep 200 &
sleep 300 &

# List all jobs
jobs
# [1]   Running                 sleep 100 &
# [2]-  Running                 sleep 200 &
# [3]+  Running                 sleep 300 &

# Bring job 2 to foreground
fg %2

# Press Ctrl+Z to suspend it
# Send it back to background
bg %2

# Kill job 1
kill %1

# Check jobs again
jobs
```

**3.5 Understand job specifiers**

```bash
# Start some jobs
sleep 100 &
sleep 200 &

# Different ways to reference jobs:
# %1      - Job number 1
# %+      - Current job (most recent)
# %-      - Previous job
# %sleep  - Job starting with "sleep"

# Try these
fg %+
# Ctrl+Z
bg %-
jobs
```

**3.6 Keep a process running after logout (nohup)**

```bash
# Create a test script
cat > /tmp/long-task.sh << 'EOF'
#!/bin/bash
for i in {1..60}; do
    echo "$(date): Running iteration $i" >> /tmp/long-task.log
    sleep 5
done
echo "$(date): Task completed" >> /tmp/long-task.log
EOF

chmod +x /tmp/long-task.sh

# Run with nohup
nohup /tmp/long-task.sh &

# Check it's running
ps aux | grep long-task

# View the output
tail -f /tmp/long-task.log
# Press Ctrl+C to stop watching

# The script will continue even if you log out
```

<details>
<summary>💡 Hints</summary>

- `&` at the end starts a process in background
- `Ctrl+Z` suspends (pauses) the foreground process
- `jobs` lists all jobs in the current shell
- `fg` brings a job to foreground
- `bg` resumes a suspended job in background
- `nohup` prevents the process from receiving SIGHUP when you log out

</details>

<details>
<summary>✅ Expected Results</summary>

**3.2 Background process:**
```bash
$ sleep 300 &
[1] 12345

$ echo "I can type!"
I can type!
```

**3.3 Suspend and resume:**
```bash
$ sleep 300
^Z
[1]+  Stopped                 sleep 300

$ jobs
[1]+  Stopped                 sleep 300

$ bg
[1]+ sleep 300 &

$ jobs
[1]+  Running                 sleep 300 &
```

**3.4 Multiple jobs:**
```bash
$ jobs
[1]   Running                 sleep 100 &
[2]-  Running                 sleep 200 &
[3]+  Running                 sleep 300 &

$ kill %1

$ jobs
[1]   Terminated              sleep 100
[2]-  Running                 sleep 200 &
[3]+  Running                 sleep 300 &
```

</details>

---

## Exercise 4: Process Control

### Objective
Practice finding, monitoring, and terminating processes.

### Tasks

**4.1 Start test processes**

```bash
# Start some test processes
sleep 1000 &
sleep 2000 &
sleep 3000 &

# Note their PIDs
jobs -l
```

**4.2 Find processes by various methods**

```bash
# Find by name using pgrep
pgrep -l sleep

# Find by name using ps and grep
ps aux | grep sleep

# Get just the PIDs
pgrep sleep

# Find by exact name using pidof
pidof sleep
```

**4.3 Monitor a specific process**

```bash
# Get one of your sleep PIDs
PID=$(pgrep -n sleep)  # -n = newest
echo "Monitoring PID: $PID"

# View process details
ps -p $PID -o pid,ppid,state,cmd,%cpu,%mem

# Watch the process in top
top -p $PID
# Press q to exit
```

**4.4 Send signals to processes**

```bash
# Start a test process
sleep 500 &
PID=$!
echo "Started process with PID: $PID"

# Check it's running
ps -p $PID

# Stop (pause) the process
kill -STOP $PID
ps -p $PID -o pid,state,cmd
# State should be T (stopped)

# Continue the process
kill -CONT $PID
ps -p $PID -o pid,state,cmd
# State should be S (sleeping)

# Send SIGTERM (graceful termination)
kill $PID
# or: kill -15 $PID
# or: kill -TERM $PID

# Verify it's gone
ps -p $PID
```

**4.5 Force kill a process**

```bash
# Start another test process
sleep 600 &
PID=$!

# Force kill (use only when necessary!)
kill -9 $PID
# or: kill -KILL $PID

# Verify
ps -p $PID
```

**4.6 Kill processes by name**

```bash
# Start multiple sleep processes
sleep 100 &
sleep 100 &
sleep 100 &

# Kill all sleep processes using killall
killall sleep

# Verify
pgrep sleep

# Start more
sleep 100 &
sleep 100 &

# Kill using pkill (pattern matching)
pkill sleep

# Verify
pgrep sleep
```

**4.7 Practice the proper kill sequence**

```bash
# Start a process
sleep 1000 &
PID=$!

# Step 1: Try graceful termination
kill $PID

# Step 2: Wait a moment
sleep 2

# Step 3: Check if still running
if ps -p $PID > /dev/null 2>&1; then
    echo "Process still running, force killing..."
    kill -9 $PID
else
    echo "Process terminated gracefully"
fi
```

<details>
<summary>💡 Hints</summary>

- Always try `kill` (SIGTERM) before `kill -9` (SIGKILL)
- SIGKILL cannot be caught or ignored - use as last resort
- `$!` contains the PID of the last background process
- Use `killall -i` for interactive confirmation before killing

</details>

<details>
<summary>✅ Expected Results</summary>

**4.2 Finding processes:**
```bash
$ pgrep -l sleep
1234 sleep
1235 sleep
1236 sleep

$ pidof sleep
1236 1235 1234
```

**4.4 Signals:**
```bash
$ kill -STOP $PID
$ ps -p $PID -o pid,state,cmd
    PID S CMD
   1234 T sleep 500

$ kill -CONT $PID
$ ps -p $PID -o pid,state,cmd
    PID S CMD
   1234 S sleep 500
```

**4.6 Kill by name:**
```bash
$ sleep 100 &
[1] 1234
$ sleep 100 &
[2] 1235
$ sleep 100 &
[3] 1236

$ killall sleep
[1]   Terminated              sleep 100
[2]-  Terminated              sleep 100
[3]+  Terminated              sleep 100

$ pgrep sleep
# No output - all killed
```

</details>

---

## Exercise 5: Service Management

### Objective
Learn to manage system services using systemctl.

### Tasks

**5.1 Check SSH service status**

```bash
# Check the status of the SSH service
sudo systemctl status ssh

# Look for:
# - Is it active (running)?
# - Is it enabled (starts at boot)?
# - What is its PID?
# - Any recent log messages?
```

Record the information:
| Property | Value |
|----------|-------|
| Active | |
| Enabled | |
| Main PID | |

**5.2 Quick status checks**

```bash
# Is SSH running?
systemctl is-active ssh

# Is SSH enabled at boot?
systemctl is-enabled ssh

# Is SSH failed?
systemctl is-failed ssh
```

**5.3 Control a service (carefully!)**

⚠️ **Warning**: If you're connected via SSH, don't stop the SSH service!

```bash
# Use cron service for safe practice
# Check cron status
sudo systemctl status cron

# Stop cron
sudo systemctl stop cron

# Verify it stopped
systemctl is-active cron

# Start cron
sudo systemctl start cron

# Verify it started
systemctl is-active cron

# Restart cron
sudo systemctl restart cron
```

**5.4 Enable and disable services at boot**

```bash
# Check if cron is enabled at boot
systemctl is-enabled cron

# If not enabled, enable it
sudo systemctl enable cron

# Verify
systemctl is-enabled cron

# Disable it (it won't start on next boot)
sudo systemctl disable cron

# Verify
systemctl is-enabled cron

# Enable it again (we want cron to run!)
sudo systemctl enable cron
```

**5.5 View service logs**

```bash
# View recent logs for SSH
sudo journalctl -u ssh -n 20

# View logs since last boot
sudo journalctl -u ssh -b

# Follow logs in real-time
sudo journalctl -u ssh -f
# Press Ctrl+C to stop

# View logs for cron
sudo journalctl -u cron -n 20
```

**5.6 List running services**

```bash
# List all running services
systemctl list-units --type=service --state=running

# Count running services
systemctl list-units --type=service --state=running | grep -c "\.service"

# List all services (including stopped)
systemctl list-units --type=service --all

# List failed services
systemctl --failed
```

**5.7 Explore a service's configuration**

```bash
# Show the service unit file location
systemctl show ssh --property=FragmentPath

# View the service unit file
systemctl cat ssh

# Show all properties of a service
systemctl show ssh
```

<details>
<summary>💡 Hints</summary>

- Service names often don't need `.service` suffix
- Use `sudo` for start/stop/enable/disable operations
- `journalctl -f` is like `tail -f` for logs
- Check `--failed` regularly to catch problems

</details>

<details>
<summary>✅ Expected Results</summary>

**5.1 SSH status:**
```bash
$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: disabled)
     Active: active (running) since Mon 2024-12-02 10:00:00 UTC; 2h ago
   Main PID: 1234 (sshd)
      Tasks: 1 (limit: 4678)
     Memory: 2.3M
```

| Property | Value |
|----------|-------|
| Active | active (running) |
| Enabled | enabled |
| Main PID | 1234 |

**5.2 Quick checks:**
```bash
$ systemctl is-active ssh
active

$ systemctl is-enabled ssh
enabled

$ systemctl is-failed ssh
active
```

**5.6 Service listing:**
```bash
$ systemctl list-units --type=service --state=running | head -10
  UNIT                     LOAD   ACTIVE SUB     DESCRIPTION
  cron.service             loaded active running Regular background program processing daemon
  dbus.service             loaded active running D-Bus System Message Bus
  ssh.service              loaded active running OpenBSD Secure Shell server
  systemd-journald.service loaded active running Journal Service
  ...
```

</details>

---

## Exercise 6: Cron Jobs

### Objective
Learn to schedule recurring tasks with cron.

### Tasks

**6.1 View existing cron jobs**

```bash
# View your user's cron jobs
crontab -l

# If you see "no crontab for user", that's fine - you'll create one

# View system cron jobs
ls -la /etc/cron.d/
ls -la /etc/cron.daily/
cat /etc/crontab
```

**6.2 Understand cron syntax**

```
# Cron format:
# minute hour day-of-month month day-of-week command
# 0-59   0-23 1-31         1-12  0-6 (0=Sun)

# Match these expressions with their meanings:
# Expression        Meaning (write your answer)
# * * * * *        ___________________
# 0 * * * *        ___________________
# 0 0 * * *        ___________________
# 30 4 * * *       ___________________
# 0 9 * * 1-5      ___________________
# */15 * * * *     ___________________
```

**6.3 Create a simple cron job**

```bash
# Edit your crontab
crontab -e

# Add this line (logs the date every minute):
* * * * * echo "$(date): Cron is working" >> /tmp/cron-test.log

# Save and exit (in nano: Ctrl+X, Y, Enter)

# Verify the cron job was added
crontab -l
```

**6.4 Verify the cron job is running**

```bash
# Wait at least 1-2 minutes, then check the log
cat /tmp/cron-test.log

# Watch for new entries
tail -f /tmp/cron-test.log
# Press Ctrl+C after you see a few entries
```

**6.5 Modify the cron job**

```bash
# Edit crontab
crontab -e

# Change to run every 5 minutes instead
*/5 * * * * echo "$(date): Cron 5-min check" >> /tmp/cron-test.log

# Save and exit
```

**6.6 Create more practical cron jobs**

```bash
# Edit crontab
crontab -e

# Add these example jobs (for practice):

# Log system uptime every hour
0 * * * * uptime >> /tmp/uptime.log

# Log disk usage daily at 8 AM
0 8 * * * df -h >> /tmp/disk-usage.log

# Clean old log files every Sunday at midnight
0 0 * * 0 find /tmp -name "*.log" -mtime +7 -delete

# Save and exit
```

**6.7 Remove the test cron jobs**

```bash
# View current jobs
crontab -l

# Edit and remove the test entries
crontab -e
# Delete the test lines, save and exit

# Or remove all cron jobs (careful!)
# crontab -r

# Verify
crontab -l
```

**6.8 Clean up test files**

```bash
# Remove test log files
rm -f /tmp/cron-test.log /tmp/uptime.log /tmp/disk-usage.log
```

<details>
<summary>💡 Hints</summary>

- Cron uses the user's environment, not your login shell environment
- Use full paths in cron jobs (e.g., `/usr/bin/echo` instead of `echo`)
- Redirect output or cron will email you (if mail is configured)
- Check `/var/log/syslog` or `journalctl -u cron` for cron logs

</details>

<details>
<summary>✅ Expected Results</summary>

**6.2 Cron expression meanings:**

| Expression | Meaning |
|------------|---------|
| `* * * * *` | Every minute |
| `0 * * * *` | Every hour (at minute 0) |
| `0 0 * * *` | Every day at midnight |
| `30 4 * * *` | Every day at 4:30 AM |
| `0 9 * * 1-5` | 9 AM on weekdays (Mon-Fri) |
| `*/15 * * * *` | Every 15 minutes |

**6.3 Cron job added:**
```bash
$ crontab -l
* * * * * echo "$(date): Cron is working" >> /tmp/cron-test.log
```

**6.4 Cron output:**
```bash
$ cat /tmp/cron-test.log
Mon Dec  2 10:30:01 UTC 2024: Cron is working
Mon Dec  2 10:31:01 UTC 2024: Cron is working
Mon Dec  2 10:32:01 UTC 2024: Cron is working
```

</details>

---

## Challenge Exercise: System Health Monitor

### Objective
Apply everything you've learned to create a comprehensive system monitoring setup.

### Scenario
You're setting up a Kali Linux system and need to:
1. Monitor system processes
2. Manage essential services
3. Set up scheduled monitoring tasks
4. Document your work

### Tasks

**Step 1: Create a monitoring script**

```bash
# Create a system health check script
cat > ~/system-health.sh << 'EOF'
#!/bin/bash
# System Health Check Script
# Run: ./system-health.sh

LOG_FILE="/tmp/system-health.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')

echo "========================================" >> $LOG_FILE
echo "System Health Report - $DATE" >> $LOG_FILE
echo "========================================" >> $LOG_FILE

echo "" >> $LOG_FILE
echo "=== System Uptime ===" >> $LOG_FILE
uptime >> $LOG_FILE

echo "" >> $LOG_FILE
echo "=== Memory Usage ===" >> $LOG_FILE
free -h >> $LOG_FILE

echo "" >> $LOG_FILE
echo "=== Disk Usage ===" >> $LOG_FILE
df -h / >> $LOG_FILE

echo "" >> $LOG_FILE
echo "=== Top 5 CPU Processes ===" >> $LOG_FILE
ps aux --sort=-%cpu | head -6 >> $LOG_FILE

echo "" >> $LOG_FILE
echo "=== Top 5 Memory Processes ===" >> $LOG_FILE
ps aux --sort=-%mem | head -6 >> $LOG_FILE

echo "" >> $LOG_FILE
echo "=== Failed Services ===" >> $LOG_FILE
systemctl --failed >> $LOG_FILE

echo "" >> $LOG_FILE
echo "Health check completed at $DATE" >> $LOG_FILE
echo "" >> $LOG_FILE

# Print to screen as well
cat $LOG_FILE
EOF

chmod +x ~/system-health.sh
```

**Step 2: Test the monitoring script**

```bash
# Run the script
./system-health.sh

# Check the log
cat /tmp/system-health.log
```

**Step 3: Start a background process and monitor it**

```bash
# Start a background process (simulated workload)
(while true; do echo "working" > /dev/null; sleep 1; done) &
WORKER_PID=$!
echo "Started worker process with PID: $WORKER_PID"

# Find it using ps
ps aux | grep $WORKER_PID

# Monitor it in top
top -p $WORKER_PID
# Press q to exit

# Check its priority
ps -p $WORKER_PID -o pid,ni,cmd

# Lower its priority (be nice)
renice 10 -p $WORKER_PID

# Verify priority changed
ps -p $WORKER_PID -o pid,ni,cmd

# Kill it when done
kill $WORKER_PID
```

**Step 4: Manage SSH service**

```bash
# Check SSH status
sudo systemctl status ssh

# View recent SSH logs
sudo journalctl -u ssh -n 10

# Ensure SSH is enabled at boot
sudo systemctl is-enabled ssh
# If not: sudo systemctl enable ssh

# Restart SSH to ensure clean state
sudo systemctl restart ssh

# Verify it's running
systemctl is-active ssh
```

**Step 5: Set up scheduled monitoring**

```bash
# Edit crontab
crontab -e

# Add these entries:

# Run health check every hour
0 * * * * /home/kali/system-health.sh

# Log system uptime every 30 minutes
*/30 * * * * echo "$(date): $(uptime)" >> /tmp/uptime-monitor.log

# Save and exit
crontab -l
```

**Step 6: Create a service status checker**

```bash
# Create a script to check essential services
cat > ~/check-services.sh << 'EOF'
#!/bin/bash
# Check Essential Services

SERVICES="ssh cron systemd-journald"

echo "Service Status Check - $(date)"
echo "=============================="

for service in $SERVICES; do
    status=$(systemctl is-active $service)
    enabled=$(systemctl is-enabled $service 2>/dev/null)
    
    if [ "$status" = "active" ]; then
        echo "✓ $service: $status (enabled: $enabled)"
    else
        echo "✗ $service: $status (enabled: $enabled)"
    fi
done
EOF

chmod +x ~/check-services.sh

# Test it
./check-services.sh
```

**Step 7: Document your setup**

```bash
# Create a summary document
cat > ~/monitoring-setup.md << 'EOF'
# System Monitoring Setup

## Date
[Add today's date]

## Scripts Created
1. `~/system-health.sh` - Comprehensive health check
2. `~/check-services.sh` - Essential service status

## Cron Jobs Configured
- Hourly: System health check
- Every 30 min: Uptime logging

## Services Managed
- SSH: [status]
- Cron: [status]

## Log Files
- `/tmp/system-health.log` - Health reports
- `/tmp/uptime-monitor.log` - Uptime history

## Commands Used
- `ps aux` - Process listing
- `top/htop` - Real-time monitoring
- `systemctl` - Service management
- `journalctl` - Log viewing
- `crontab` - Task scheduling
EOF

# Edit with your actual values
nano ~/monitoring-setup.md
```

**Step 8: Clean up**

```bash
# Remove cron jobs (keep or remove as desired)
crontab -e
# Remove the test entries

# Clean up test files
rm -f /tmp/system-health.log /tmp/uptime-monitor.log

# Keep the scripts if you find them useful!
ls -la ~/*.sh
```

<details>
<summary>💡 Hints</summary>

- Use `>>` to append to logs, `>` to overwrite
- `systemctl --failed` shows any services with problems
- cron jobs run with limited environment - use full paths
- Always test scripts manually before adding to cron

</details>

<details>
<summary>✅ Expected Results</summary>

**Step 2: Health check output:**
```
========================================
System Health Report - 2024-12-02 10:30:00
========================================

=== System Uptime ===
 10:30:00 up 2 days, 3:45, 2 users, load average: 0.15, 0.20, 0.18

=== Memory Usage ===
              total        used        free      shared  buff/cache   available
Mem:          7.8Gi       3.4Gi       2.3Gi       234Mi       2.1Gi       3.9Gi
Swap:         2.0Gi          0B       2.0Gi

=== Disk Usage ===
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   15G   33G  31% /

=== Top 5 CPU Processes ===
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...

=== Top 5 Memory Processes ===
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...

=== Failed Services ===
0 loaded units listed.
```

**Step 6: Service check output:**
```
Service Status Check - Mon Dec  2 10:30:00 UTC 2024
==============================
✓ ssh: active (enabled: enabled)
✓ cron: active (enabled: enabled)
✓ systemd-journald: active (enabled: static)
```

</details>

---

## Bonus Challenges

### Challenge A: Process Tree Investigation

```bash
# Find the full process tree for your current shell
pstree -p $$

# Create a child process and observe the tree
bash
echo "New shell PID: $$"
pstree -p $$
exit

# Find all processes started by systemd
pstree -p 1 | head -50
```

### Challenge B: Signal Handling

```bash
# Create a script that handles signals
cat > /tmp/signal-test.sh << 'EOF'
#!/bin/bash
trap 'echo "Received SIGTERM"; exit 0' TERM
trap 'echo "Received SIGINT (Ctrl+C)"; exit 0' INT
trap 'echo "Received SIGHUP"' HUP

echo "PID: $$"
echo "Send me signals! (TERM, INT, HUP)"
echo "Or press Ctrl+C"

while true; do
    sleep 1
done
EOF

chmod +x /tmp/signal-test.sh

# Run in background
/tmp/signal-test.sh &
PID=$!

# Test signals
kill -HUP $PID
sleep 1
kill -TERM $PID
```

### Challenge C: Resource Limits

```bash
# View current resource limits
ulimit -a

# Start a process with limited resources
# (Limit CPU time to 5 seconds)
bash -c 'ulimit -t 5; while true; do :; done'
# Process will be killed after 5 CPU seconds
```

### Challenge D: System Target Investigation

```bash
# View current target
systemctl get-default

# List all targets
systemctl list-units --type=target

# See what services are part of multi-user target
systemctl list-dependencies multi-user.target

# See what services are part of graphical target
systemctl list-dependencies graphical.target
```

---

## 📝 Summary

After completing these exercises, you should be able to:

- ✅ View and analyze running processes with `ps`, `pgrep`, and `pidof`
- ✅ Monitor processes in real-time with `top` and `htop`
- ✅ Manage background and foreground processes
- ✅ Suspend, resume, and control jobs
- ✅ Safely terminate processes using signals
- ✅ Manage services with `systemctl`
- ✅ View service logs with `journalctl`
- ✅ Create and manage cron jobs for scheduled tasks
- ✅ Create monitoring scripts for system health

## Cleanup

To remove all test files and cron jobs created during these exercises:

```bash
# Remove test files
rm -f /tmp/cron-test.log /tmp/uptime.log /tmp/disk-usage.log
rm -f /tmp/system-health.log /tmp/uptime-monitor.log
rm -f /tmp/long-task.sh /tmp/long-task.log
rm -f /tmp/signal-test.sh

# Remove test scripts (keep if useful)
# rm -f ~/system-health.sh ~/check-services.sh ~/monitoring-setup.md

# Clear cron jobs if added
crontab -e
# Remove test entries

# Kill any remaining test processes
killall sleep 2>/dev/null

echo "Cleanup complete!"
```

Note: You may want to keep `~/system-health.sh` and `~/check-services.sh` as they can be useful for ongoing system monitoring!