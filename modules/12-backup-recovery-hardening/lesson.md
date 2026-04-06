# Module 12: Backup, Recovery, and Basic Hardening

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Design a practical backup strategy for Linux systems
- Perform file and configuration backups with standard tools
- Verify and restore backups safely
- Apply basic Linux hardening techniques
- Build a simple recovery and security checklist for small servers

---

## Introduction

If monitoring tells you **something is wrong**, backups and hardening decide whether the outcome is a minor incident or a major disaster.

This module closes Linux 101 with two core sysadmin responsibilities:
1. **Recoverability** — Can we restore data and service quickly?
2. **Security baseline** — Have we reduced obvious attack paths?

---

## 1. Backup Fundamentals

### Why backups fail in real life

Many teams claim they have backups, but fail during recovery because:
- backups were never tested
- critical files were excluded
- permissions/ownership were lost
- restore procedures were undocumented

### The 3-2-1 principle

A practical rule:
- **3 copies** of data
- **2 different media types** (e.g., disk + cloud)
- **1 offsite** copy

For beginners, start simple and consistent.

---

## 2. What to Back Up on Linux

Typical priorities:
- User data (`/home`)
- System configuration (`/etc`)
- Application data directories (service-specific)
- Databases (with proper dump tools)
- Custom scripts and automation

Usually **not** needed:
- temporary files (`/tmp`)
- caches (`/var/cache`)
- reinstallable packages

---

## 3. Backup Tools and Commands

### `tar` archives

Create compressed backup:

```bash
sudo tar -czvf /backup/etc-$(date +%F).tar.gz /etc
```

List archive contents:

```bash
tar -tzf /backup/etc-2026-04-06.tar.gz | head
```

Restore to alternate location for safe testing:

```bash
mkdir -p ~/restore-test
tar -xzvf /backup/etc-2026-04-06.tar.gz -C ~/restore-test
```

### `rsync` for incremental sync

```bash
rsync -avh --delete /home/ /mnt/backup/home/
```

Useful options:
- `-a` archive mode (permissions, timestamps, recursion)
- `-v` verbose
- `-h` human-readable sizes
- `--delete` remove files on destination no longer in source

⚠️ Use `--delete` carefully.

---

## 4. Backup Verification and Restore Drills

A backup is only trustworthy if restoration works.

Minimum verification routine:
1. Confirm backup file exists and has expected size.
2. Validate archive/listing (`tar -tzf`, checksums).
3. Restore a sample file to a test directory.
4. Open and verify data integrity.
5. Record outcome in a backup log.

Create checksums:

```bash
sha256sum /backup/etc-2026-04-06.tar.gz > /backup/etc-2026-04-06.tar.gz.sha256
sha256sum -c /backup/etc-2026-04-06.tar.gz.sha256
```

---

## 5. Automation with Cron

Schedule daily backup at 2:30 AM:

```bash
crontab -e
```

Example entry:

```cron
30 2 * * * /home/user/scripts/backup-home.sh >> /home/user/logs/backup.log 2>&1
```

Best practice:
- include logging
- alert on failure
- rotate old backups

---

## 6. Basic Linux Hardening Checklist

### Account and authentication

- Use strong unique passwords
- Disable direct root SSH login
- Use SSH keys where possible
- Remove unused user accounts
- Review `sudo` privileges carefully

### System updates

- Install security updates regularly
- Remove unnecessary packages/services

### Network surface reduction

- Close unused ports
- Use host firewall (`ufw`/`nftables`/`iptables` depending on distro)
- Bind services only to required interfaces

### File and permission hygiene

- Restrict sensitive files (`/etc/shadow`, keys, secrets)
- Avoid world-writable files in sensitive paths
- Review script permissions and ownership

### Auditing and logging

- Ensure logs are retained and monitored
- Watch authentication logs for brute-force attempts

---

## 7. Recovery Planning Basics

A recovery plan should answer:
1. Which services are critical?
2. Where are backups stored?
3. Who can restore?
4. What is the target recovery time?
5. How do we verify success?

Create a one-page runbook including:
- restore commands
- service restart steps
- validation checks
- escalation contacts

---

## 8. Final Capstone Connection

Module 12 connects directly to the capstone project:
- Use scripting (Module 10) to automate backup + verification
- Use monitoring (Module 11) to detect failures quickly
- Apply permissions, users/groups, packages, and services from Modules 1-9

This is the complete Linux 101 operational cycle:
**configure → monitor → protect → recover**.

---

## Summary

In this module you learned how to:
- Design and automate practical Linux backups
- Validate and test restores
- Apply a baseline hardening approach
- Document a basic recovery workflow

You now have a solid beginner-to-junior foundation for Linux administration work.
