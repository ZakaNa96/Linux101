# Module 11: Exercises - System Monitoring and Log Analysis

## 🎯 Learning Objectives

These exercises will help you:
- Collect and interpret system performance data
- Diagnose service failures with logs
- Practice a structured troubleshooting process
- Build habits for daily Linux operations checks

---

## Exercise 1: Baseline Health Snapshot

### Goal
Create a point-in-time health report of your system.

### Tasks
1. Create a folder for this module:
   ```bash
   mkdir -p ~/linux101/module11
   cd ~/linux101/module11
   ```
2. Save outputs from the following commands to separate files:
   ```bash
   date > snapshot-time.txt
   uptime > uptime.txt
   free -h > memory.txt
   df -h > disk.txt
   ip a > network.txt
   ss -tulpen > sockets.txt
   ```
3. Review each file and write a short summary in `baseline-notes.txt`:
   - Is memory pressure low/medium/high?
   - Which filesystem is most full?
   - Which services are listening on network ports?

### Deliverable
A folder containing command outputs and your notes.

---

## Exercise 2: Process Investigation

### Goal
Identify top resource-consuming processes.

### Tasks
1. Run:
   ```bash
   ps aux --sort=-%cpu | head -n 10
   ps aux --sort=-%mem | head -n 10
   ```
2. Save both command outputs to files.
3. For the top 3 processes, document:
   - PID
   - user
   - command
   - reason you think it uses resources
4. Compare with `top` output and verify if your conclusions are consistent.

### Challenge
Use `watch -n 2 "ps aux --sort=-%cpu | head -n 6"` for 1 minute and observe process changes.

---

## Exercise 3: Log Hunting with `journalctl`

### Goal
Practice finding useful events in noisy logs.

### Tasks
1. View recent warning/error messages:
   ```bash
   journalctl -p warning..alert --since "2 hours ago"
   ```
2. Inspect SSH logs:
   ```bash
   journalctl -u ssh --since "today"
   ```
3. Record at least 5 notable entries in `log-findings.md` with:
   - timestamp
   - service
   - severity
   - what it likely means

### Bonus
Try filtering logs from the current boot only:
```bash
journalctl -b -p err..alert
```

---

## Exercise 4: Simulated Incident Response

### Goal
Follow a structured troubleshooting workflow.

### Scenario
"A service is down and users cannot access it."

### Tasks
1. Pick a service on your machine (for example `ssh`):
   ```bash
   systemctl status ssh
   ```
2. Gather diagnostics:
   ```bash
   journalctl -u ssh --since "30 minutes ago"
   ss -tulpen | grep ':22'
   ```
3. Create a short incident report (`incident-report.md`) with:
   - Problem statement
   - Evidence collected
   - Suspected root cause
   - Fix or mitigation
   - Verification steps

### Deliverable
An incident report with command evidence.

---

## Exercise 5: Build a Daily Check Script (Integration with Module 10)

### Goal
Create a script that performs basic daily monitoring checks.

### Tasks
1. Create `daily-check.sh` that outputs:
   - current date/time
   - uptime
   - memory summary (`free -h`)
   - disk summary (`df -h`)
   - failed services (`systemctl --failed`)
2. Save output to a timestamped log file:
   ```bash
   ~/linux101/module11/reports/daily-YYYYMMDD-HHMM.txt
   ```
3. Make script executable and run it.

### Stretch Goal
Add a warning message if any filesystem is above 85% usage.

---

## Self-Assessment Checklist

Before moving to Module 12, ensure you can:
- [ ] Quickly find high CPU or high memory processes
- [ ] Check disk usage and identify large directories
- [ ] Query logs by service, severity, and time range
- [ ] Follow a repeatable troubleshooting workflow
- [ ] Produce a short incident report with evidence
