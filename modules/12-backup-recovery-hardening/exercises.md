# Module 12: Exercises - Backup, Recovery, and Basic Hardening

## 🎯 Learning Objectives

These exercises will help you:
- Create repeatable backups of important Linux data
- Verify and test restores rather than assuming success
- Automate backups with logging
- Apply and validate practical hardening controls

---

## Exercise 1: Identify Backup Scope

### Goal
Define what should and should not be backed up on your system.

### Tasks
1. Create `backup-scope.md` in `~/linux101/module12/`.
2. Include sections:
   - Critical data (must back up)
   - Important but non-critical data
   - Data to exclude
3. Add your reasons for each decision.

### Hint
Focus on `/home`, `/etc`, and service/application data.

---

## Exercise 2: Create and Verify a `tar` Backup

### Goal
Create a backup archive and verify its integrity.

### Tasks
1. Create a test source directory with sample files.
2. Build a compressed archive:
   ```bash
   tar -czvf backup-test.tar.gz <source_dir>
   ```
3. Generate checksum:
   ```bash
   sha256sum backup-test.tar.gz > backup-test.tar.gz.sha256
   ```
4. Verify checksum:
   ```bash
   sha256sum -c backup-test.tar.gz.sha256
   ```
5. List archive contents without extracting:
   ```bash
   tar -tzf backup-test.tar.gz
   ```

### Deliverable
Archive, checksum file, and a short notes file documenting results.

---

## Exercise 3: Restore Drill

### Goal
Prove that a restore works.

### Tasks
1. Create `restore-target/` directory.
2. Extract your archive into it.
3. Compare source and restore:
   ```bash
   diff -r <source_dir> restore-target/<source_dir_name>
   ```
4. Document whether permissions and content are correct.

### Success Criteria
`diff` returns no differences (or only expected metadata differences).

---

## Exercise 4: Rsync Backup Script

### Goal
Automate directory sync with logging.

### Tasks
1. Create `backup-home.sh` script that:
   - uses `rsync -avh`
   - writes logs to `~/linux101/module12/logs/`
   - exits with a clear success/failure message
2. Make script executable and run it.
3. Re-run after modifying a source file and verify destination updates.

### Stretch Goal
Add retention cleanup for old log files (e.g., older than 14 days).

---

## Exercise 5: Basic Hardening Audit

### Goal
Perform a mini hardening review on your Linux VM.

### Tasks
Check and document the following:
- Current users with shell access
- `sudo`-capable accounts
- SSH root login configuration
- Open listening ports (`ss -tulpen`)
- Pending updates

Record findings in `hardening-audit.md` with:
- current state
- risk level (low/medium/high)
- recommended action

---

## Exercise 6: Recovery Runbook

### Goal
Create a short operational recovery guide.

### Tasks
Create `recovery-runbook.md` that includes:
1. Backup locations
2. Restore commands
3. Service restart/validation commands
4. Who to notify in an incident
5. Post-recovery verification checklist

### Bonus
Time yourself while performing a test restore and record estimated recovery time.

---

## Final Reflection

Before finishing Linux 101, answer in `final-reflection.md`:
1. Which Linux area do you feel strongest in now?
2. Which area needs more practice?
3. What 30-day plan will you follow to keep improving?
4. Which tasks can you automate immediately at work/lab?
