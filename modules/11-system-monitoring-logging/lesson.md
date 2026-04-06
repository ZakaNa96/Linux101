# Module 11: System Monitoring and Log Analysis

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Monitor CPU, memory, disk, and network usage on a Linux system
- Investigate system issues using logs and journald
- Identify performance bottlenecks and suspicious behavior
- Use command-line tools to troubleshoot common production problems
- Create a repeatable monitoring checklist for daily admin work

---

## Introduction

Now that you can script administrative tasks, the next step is learning how to **observe** and **diagnose** systems in real time.

Good administrators do not only configure systems — they constantly monitor health, investigate warnings, and respond before small issues become outages.

This module teaches practical monitoring and log analysis skills used every day in operations and security-focused environments.

---

## 1. Why Monitoring Matters

A Linux server can fail for many reasons:
- CPU overloaded by runaway processes
- Memory exhaustion causing swapping or crashes
- Full disks preventing writes
- Failed services after updates
- Network congestion or interface misconfiguration

Monitoring helps you answer these questions quickly:
1. What changed?
2. What is failing?
3. When did it start?
4. Which component is affected?

---

## 2. Real-Time System Monitoring Tools

### `top` and `htop`

Use `top` to see live process and resource usage:

```bash
top
```

Key fields to watch:
- **load average** (overall system pressure)
- **%CPU** and **%MEM** per process
- **Tasks** (running/sleeping/zombie)
- **Swap usage**

If available, `htop` is easier to read:

```bash
htop
```

### `free` for memory

```bash
free -h
```

Focus on:
- available RAM
- swap usage trends

### `vmstat` for quick performance snapshots

```bash
vmstat 1 5
```

This shows 5 samples at 1-second intervals. Helpful for spotting CPU wait (`wa`) or swapping activity.

### `iostat` for disk I/O

```bash
iostat -xz 1 3
```

(Install via `sysstat` if needed.)

Look for:
- high `%util`
- long `await` values

### `df` and `du` for storage

```bash
df -h
du -sh /var/log/* 2>/dev/null | sort -h
```

Use `df` for filesystem capacity and `du` for identifying large directories.

---

## 3. Network Visibility Basics

### Interface and address checks

```bash
ip a
ip route
```

### Socket and service checks

```bash
ss -tulpen
```

Use this to verify listening ports and owning processes.

### Connectivity tests

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

- IP ping works but DNS ping fails → likely DNS issue
- both fail → network routing/firewall issue

---

## 4. Log Files and journald

Logs are your primary evidence during troubleshooting.

### Traditional log files

Common locations under `/var/log`:
- `/var/log/syslog` or `/var/log/messages`
- `/var/log/auth.log`
- `/var/log/kern.log`
- service-specific logs (e.g., nginx, apache2)

Useful commands:

```bash
tail -f /var/log/syslog
less /var/log/auth.log
grep -i "error" /var/log/syslog
```

### `journalctl` with systemd

```bash
journalctl -xe
journalctl -u ssh
journalctl --since "1 hour ago"
journalctl -p err..alert
```

Examples:
- See all SSH service logs
- Filter by severity and time range
- Inspect boot-time failures with `journalctl -b`

---

## 5. Practical Troubleshooting Workflow

When something breaks:

1. **Confirm the symptom**
   - What exactly is failing? (web app, SSH, package install)
2. **Check service status**
   - `systemctl status <service>`
3. **Check recent logs**
   - `journalctl -u <service> --since "30 minutes ago"`
4. **Check system resources**
   - `top`, `free -h`, `df -h`
5. **Validate dependencies**
   - network, DNS, file permissions, ports
6. **Document findings and fix**
   - include root cause and prevention actions

---

## 6. Building a Daily Monitoring Checklist

A junior sysadmin checklist might include:
- Disk usage under 80%
- No failed critical services
- Backup job success from last run
- No repeated authentication failures
- No unusual CPU/memory spikes
- No runaway logs filling storage

You can automate this checklist later using scripts from Module 10.

---

## 7. Mini Case Study

**Scenario:** Users report a web dashboard is "very slow".

**Investigation:**
1. `top` shows CPU near 100% by a Python process.
2. `df -h` shows `/var` at 98%.
3. `du -sh /var/log/*` reveals one giant app log.
4. `journalctl -u app-service` shows repeated errors causing log spam.

**Root cause:** Service entered error loop, generating massive logs and consuming resources.

**Fix:** Restart service, fix misconfiguration, rotate logs properly, and add alerting thresholds.

---

## Summary

In this module you learned how to:
- Observe Linux system health in real time
- Investigate logs and service-level failures
- Follow a repeatable troubleshooting workflow

These skills are essential for both system administration and security incident response.
