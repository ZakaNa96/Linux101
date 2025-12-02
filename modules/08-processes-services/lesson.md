# Module 8: Process and Service Management

## 🎯 Learning Goals

By the end of this module, you will be able to:
- Understand what processes are and how they work in Linux
- View and monitor running processes using ps, top, and htop
- Manage process priorities with nice and renice
- Control background and foreground processes
- Kill and terminate processes safely
- Understand Linux services (daemons) and systemd
- Manage services with systemctl
- Schedule tasks with cron and at
- Troubleshoot common process and service issues

---

## Prerequisites

Before starting this module, you should be comfortable with:
- ✅ Module 1: Basic terminal navigation
- ✅ Module 2: Navigating the filesystem
- ✅ Module 3: Working with files and directories
- ✅ Module 4: Viewing and editing text files
- ✅ Module 5: File permissions and ownership
- ✅ Module 6: User management and sudo
- ✅ Module 7: Package management with APT

---

## 1. Understanding Processes

### What is a Process?

A **process** is a running instance of a program. When you execute a command or open an application, the system creates a process to run that code.

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROGRAM vs PROCESS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Program                          Process                        │
│  ───────                          ───────                        │
│  • Static code on disk            • Running instance of program  │
│  • Stored in /usr/bin, etc.       • Exists in memory (RAM)       │
│  • One copy exists                • Multiple instances possible  │
│  • Example: /usr/bin/firefox      • Example: firefox running     │
│                                     with PID 1234                │
│                                                                  │
│  Think of it like:                                               │
│  • Program = Recipe book          • Process = Cooking the recipe │
│  • Program = Blueprint            • Process = Building the house │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Process ID (PID)

Every process gets a unique **Process ID (PID)** - a number that identifies it:

```bash
# Your current shell's PID
echo $$

# Example output: 1234
```

```
┌─────────────────────────────────────────────────────────────────┐
│                    IMPORTANT PIDs                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  PID 1     - The init/systemd process (first process started)   │
│              Parent of all other processes                       │
│                                                                  │
│  PID 0     - The kernel scheduler (not a real process)          │
│                                                                  │
│  Your PID  - Each shell session has its own PID                 │
│              Check with: echo $$                                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Parent and Child Processes

Processes have family relationships:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROCESS HIERARCHY                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│                      systemd (PID 1)                             │
│                           │                                      │
│            ┌──────────────┼──────────────┐                      │
│            │              │              │                      │
│            ▼              ▼              ▼                      │
│        sshd           cron          apache2                     │
│        (PID 500)      (PID 501)     (PID 502)                   │
│            │                            │                       │
│            ▼                            │                       │
│        sshd                       ┌─────┼─────┐                 │
│        (PID 1234)                 ▼     ▼     ▼                 │
│            │                   apache2 workers                  │
│            ▼                   (PIDs 503, 504, 505)             │
│        bash                                                      │
│        (PID 1235)                                                │
│            │                                                     │
│            ▼                                                     │
│        vim                                                       │
│        (PID 1236)                                                │
│                                                                  │
│  • PPID (Parent PID) - The PID of the process that created it   │
│  • Child processes inherit environment from parent               │
│  • When parent dies, children may become orphans (adopted by 1) │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Process States

Processes can be in different states:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROCESS STATES                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  State        Code    Description                                │
│  ─────        ────    ───────────                                │
│  Running      R       Currently executing on CPU                 │
│                       or waiting in run queue                    │
│                                                                  │
│  Sleeping     S       Waiting for an event (most common)         │
│               D       Uninterruptible sleep (waiting for I/O)    │
│                                                                  │
│  Stopped      T       Stopped by signal (Ctrl+Z) or debugger     │
│                                                                  │
│  Zombie       Z       Process finished but parent hasn't         │
│                       collected its exit status yet              │
│                                                                  │
│                                                                  │
│  Lifecycle:                                                      │
│                                                                  │
│  Created → Running ←→ Sleeping → Terminated → Zombie → Removed  │
│               ↓                                                  │
│            Stopped                                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

💡 **Tip**: A zombie process isn't harmful by itself, but many zombies indicate a problem with the parent process not cleaning up properly.

---

## 2. Viewing Processes

### `ps` - Process Snapshot

The `ps` command shows a snapshot of current processes:

```bash
# Basic: show processes in current terminal
ps

# Example output:
#   PID TTY          TIME CMD
#  1234 pts/0    00:00:00 bash
#  5678 pts/0    00:00:00 ps
```

### `ps aux` - All Processes (BSD Style)

The most commonly used format:

```bash
# Show ALL processes with details
ps aux
```

**Understanding the output:**

```
USER       PID  %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1   0.0  0.1 169584 13256 ?        Ss   Dec01   0:02 /sbin/init
root         2   0.0  0.0      0     0 ?        S    Dec01   0:00 [kthreadd]
kali      1234   0.1  0.5 234567 45678 pts/0    Ss   10:00   0:01 bash
kali      5678   2.5  1.2 456789 98765 pts/0    S+   10:05   0:30 vim file.txt
```

| Column | Description |
|--------|-------------|
| USER | Owner of the process |
| PID | Process ID |
| %CPU | CPU usage percentage |
| %MEM | Memory usage percentage |
| VSZ | Virtual memory size (KB) |
| RSS | Resident Set Size - actual RAM used (KB) |
| TTY | Terminal (? means no terminal) |
| STAT | Process state (R, S, T, Z, etc.) |
| START | When the process started |
| TIME | Total CPU time used |
| COMMAND | The command that started the process |

### `ps -ef` - All Processes (UNIX Style)

An alternative format showing parent processes:

```bash
# Show all processes with parent PID
ps -ef
```

**Output:**
```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Dec01 ?        00:00:02 /sbin/init
root         2     0  0 Dec01 ?        00:00:00 [kthreadd]
kali      1234  1200  0 10:00 pts/0    00:00:01 bash
kali      5678  1234  2 10:05 pts/0    00:00:30 vim file.txt
```

| Column | Description |
|--------|-------------|
| UID | User ID (username) |
| PID | Process ID |
| PPID | Parent Process ID |
| C | CPU utilization |
| STIME | Start time |
| TTY | Terminal |
| TIME | CPU time |
| CMD | Command |

### Useful `ps` Options

```bash
# Show processes as a tree (shows hierarchy)
ps auxf

# Show processes for a specific user
ps -u kali

# Show specific columns only
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu

# Show threads
ps -eLf

# Find a specific process
ps aux | grep firefox
```

### `pgrep` - Find Processes by Name

Find PIDs of processes matching a pattern:

```bash
# Find PID of process by name
pgrep firefox

# Show process names too
pgrep -l firefox

# Show full command line
pgrep -a firefox

# Find processes by user
pgrep -u kali

# Count matching processes
pgrep -c bash
```

### `pidof` - Find PID of Running Program

Get the PID of an exact program name:

```bash
# Find PID of a program
pidof bash
pidof firefox

# Returns multiple PIDs if multiple instances
pidof python3
```

💡 **Tip**: Use `pgrep` for pattern matching and `pidof` for exact program names.

---

## 3. Real-time Process Monitoring

### `top` - Interactive Process Viewer

The `top` command shows live, updating process information:

```bash
# Launch top
top
```

**Understanding the `top` display:**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ top - 10:30:00 up 2 days,  3:45,  2 users,  load average: 0.15, 0.20, 0.18 │
│ Tasks: 234 total,   1 running, 232 sleeping,   0 stopped,   1 zombie       │
│ %Cpu(s):  5.2 us,  2.1 sy,  0.0 ni, 92.0 id,  0.5 wa,  0.0 hi,  0.2 si     │
│ MiB Mem :   7976.3 total,   2345.2 free,   3456.7 used,   2174.4 buff/cache│
│ MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   4123.5 avail Mem │
├─────────────────────────────────────────────────────────────────────────────┤
│   PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND│
│  1234 kali      20   0  456789  98765  34567 S   5.0   1.2   0:30.50 firefox│
│  5678 root      20   0   12345   2345   1234 R   2.0   0.0   0:05.20 top    │
│  ...                                                                        │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Header sections:**

| Line | Information |
|------|-------------|
| Line 1 | System time, uptime, users, load average (1, 5, 15 min) |
| Line 2 | Total tasks and their states |
| Line 3 | CPU usage breakdown |
| Line 4 | Memory usage |
| Line 5 | Swap usage |

**CPU breakdown:**
- `us` - User space (your programs)
- `sy` - System/kernel
- `ni` - Nice (low priority user)
- `id` - Idle
- `wa` - Waiting for I/O
- `hi` - Hardware interrupts
- `si` - Software interrupts

### `top` Keyboard Commands

While `top` is running:

| Key | Action |
|-----|--------|
| `q` | Quit top |
| `h` | Help |
| `k` | Kill a process (asks for PID) |
| `r` | Renice a process (change priority) |
| `M` | Sort by memory usage |
| `P` | Sort by CPU usage (default) |
| `T` | Sort by time |
| `N` | Sort by PID |
| `u` | Filter by user |
| `c` | Toggle full command path |
| `1` | Toggle individual CPU cores |
| `Space` | Refresh immediately |

### `htop` - Enhanced Process Viewer

`htop` is a more user-friendly alternative to `top`:

```bash
# Install htop if not present
sudo apt install htop

# Launch htop
htop
```

```
┌─────────────────────────────────────────────────────────────────┐
│                    htop ADVANTAGES                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Visual Features:                                                │
│  • Color-coded CPU/memory meters                                 │
│  • Scroll through process list with arrow keys                   │
│  • Mouse support (click to select/sort)                          │
│  • Visual process tree (F5)                                      │
│                                                                  │
│  Functionality:                                                  │
│  • Kill multiple processes at once                               │
│  • Search processes (F3)                                         │
│  • Filter processes (F4)                                         │
│  • Sort columns easily (F6)                                      │
│  • Show/hide threads                                             │
│  • Better configuration (F2)                                     │
│                                                                  │
│  Keyboard shortcuts (in htop):                                   │
│  • F1 - Help        • F6 - Sort by                               │
│  • F2 - Setup       • F7 - Nice -                                │
│  • F3 - Search      • F8 - Nice +                                │
│  • F4 - Filter      • F9 - Kill                                  │
│  • F5 - Tree view   • F10 - Quit                                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

💡 **Tip**: `htop` is generally preferred for interactive use, while `top` is useful when `htop` isn't installed or for scripts.

---

## 4. Process Priority and Nice Values

### Understanding Priority

Linux processes have a **priority** that determines how much CPU time they get:

```
┌─────────────────────────────────────────────────────────────────┐
│                    NICE VALUES                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Nice Value Range: -20 to +19                                    │
│                                                                  │
│  -20 ◄──────────────── 0 ────────────────► +19                  │
│   │                    │                    │                    │
│   │                    │                    │                    │
│  HIGHEST            DEFAULT              LOWEST                  │
│  PRIORITY           PRIORITY             PRIORITY                │
│  (least nice)       (standard)           (most nice)             │
│                                                                  │
│  • Lower nice = Higher priority = More CPU time                  │
│  • Higher nice = Lower priority = Less CPU time                  │
│  • Default nice value is 0                                       │
│  • Only root can set negative nice values                        │
│                                                                  │
│  The term "nice" reflects how "nice" the process is to others:  │
│  • A nice process (high value) yields CPU to others              │
│  • An "un-nice" process (low value) demands more CPU             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### `nice` - Start Process with Priority

Start a new process with a specific nice value:

```bash
# Start a command with lower priority (nice value 10)
nice -n 10 long-running-command

# Start with higher priority (requires root)
sudo nice -n -10 important-command

# Default nice (nice value 10)
nice long-running-command

# Check nice value of current shell
nice
```

### `renice` - Change Running Process Priority

Change the priority of an already running process:

```bash
# Change nice value of a process by PID
renice 10 -p 1234

# Make process higher priority (requires root)
sudo renice -10 -p 1234

# Change nice value for all processes of a user
sudo renice 5 -u username

# Change nice value for a process group
sudo renice 15 -g groupid
```

**Example workflow:**

```bash
# Start a CPU-intensive task
./compile-project.sh &
[1] 1234

# Check its current priority
ps -o pid,ni,cmd -p 1234
#   PID  NI CMD
#  1234   0 ./compile-project.sh

# Lower its priority (be nice to other processes)
renice 10 -p 1234
# 1234 (process ID) old priority 0, new priority 10

# Verify the change
ps -o pid,ni,cmd -p 1234
#   PID  NI CMD
#  1234  10 ./compile-project.sh
```

⚠️ **Warning**: Only root can:
- Set negative nice values (higher priority)
- Decrease nice value (increase priority) of a process
- Change nice value of processes owned by other users

---

## 5. Background and Foreground Processes

### Foreground Processes

By default, when you run a command, it runs in the **foreground**:
- It occupies your terminal
- You must wait for it to finish
- You can interact with it (provide input)

```bash
# This runs in foreground - terminal is blocked
sleep 60
# You have to wait 60 seconds before you can type again
```

### Running Processes in Background

Use `&` to run a process in the background:

```bash
# Start in background
sleep 60 &
# [1] 1234

# Terminal is immediately available
echo "I can type immediately!"
```

### Controlling Process Execution

```
┌─────────────────────────────────────────────────────────────────┐
│                    PROCESS CONTROL                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Key/Command      Action                                         │
│  ───────────      ──────                                         │
│  Ctrl+C           Terminate (kill) foreground process            │
│  Ctrl+Z           Suspend (pause) foreground process             │
│  command &        Start command in background                    │
│  jobs             List background jobs                           │
│  fg               Bring most recent job to foreground            │
│  fg %n            Bring job n to foreground                      │
│  bg               Resume paused job in background                │
│  bg %n            Resume job n in background                     │
│                                                                  │
│  Flow:                                                           │
│                                                                  │
│  ┌──────────┐    Ctrl+Z    ┌───────────┐    bg     ┌──────────┐ │
│  │Foreground│ ───────────► │  Stopped  │ ────────► │Background│ │
│  └──────────┘              └───────────┘           └──────────┘ │
│       ▲                          │                      │       │
│       │          fg              │         fg           │       │
│       └──────────────────────────┴──────────────────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### `jobs` - List Background Jobs

```bash
# Start some background jobs
sleep 100 &
sleep 200 &
sleep 300 &

# List all jobs
jobs
# [1]   Running                 sleep 100 &
# [2]-  Running                 sleep 200 &
# [3]+  Running                 sleep 300 &

# The + indicates the current job (most recent)
# The - indicates the previous job
```

### `fg` - Bring to Foreground

```bash
# Bring most recent job to foreground
fg

# Bring specific job to foreground
fg %1    # Job 1
fg %2    # Job 2

# You can also use the command name
fg %sleep
```

### `bg` - Resume in Background

After suspending a process with `Ctrl+Z`:

```bash
# Start a process
sleep 100
# Press Ctrl+Z
# [1]+  Stopped                 sleep 100

# Resume it in background
bg
# [1]+ sleep 100 &

# Or specify which job
bg %1
```

### `nohup` - Keep Running After Logout

Normally, background processes are killed when you log out. Use `nohup` to prevent this:

```bash
# Run a command that survives logout
nohup long-running-script.sh &

# Output goes to nohup.out by default
# Or redirect output
nohup long-running-script.sh > output.log 2>&1 &
```

**Practical example:**

```bash
# Start a backup that will continue even if you disconnect
nohup rsync -av /home/user/ /backup/user/ > backup.log 2>&1 &

# You can safely close your terminal now
```

💡 **Tip**: For long-running tasks over SSH, consider using `screen` or `tmux` instead of `nohup` for better control.

---

## 6. Killing/Terminating Processes

### Understanding Signals

Signals are how you communicate with processes:

```
┌─────────────────────────────────────────────────────────────────┐
│                    COMMON SIGNALS                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Number  Name      Description                                   │
│  ──────  ────      ───────────                                   │
│  1       SIGHUP    Hangup - terminal closed, reload config       │
│  2       SIGINT    Interrupt - Ctrl+C                            │
│  9       SIGKILL   Kill immediately - cannot be caught/ignored   │
│  15      SIGTERM   Terminate gracefully - default kill signal    │
│  18      SIGCONT   Continue - resume stopped process             │
│  19      SIGSTOP   Stop - pause process (like Ctrl+Z)            │
│  20      SIGTSTP   Terminal stop - Ctrl+Z                        │
│                                                                  │
│  Best Practice Order:                                            │
│  1. SIGTERM (15) - Ask nicely to terminate                       │
│  2. Wait a few seconds                                           │
│  3. SIGKILL (9) - Force kill (last resort)                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### `kill` - Send Signal to Process

```bash
# Send SIGTERM (default, graceful termination)
kill 1234

# Explicitly send SIGTERM
kill -15 1234
kill -TERM 1234
kill -SIGTERM 1234

# Force kill (use as last resort!)
kill -9 1234
kill -KILL 1234

# Reload configuration (common for daemons)
kill -1 1234
kill -HUP 1234

# Stop/pause a process
kill -STOP 1234

# Continue a stopped process
kill -CONT 1234

# List all available signals
kill -l
```

⚠️ **Warning about `kill -9`:**
```
┌─────────────────────────────────────────────────────────────────┐
│                    ⚠️ DANGER: kill -9                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  kill -9 (SIGKILL) should be a LAST RESORT because:             │
│                                                                  │
│  ✗ Process cannot clean up (temp files, locks, etc.)            │
│  ✗ Process cannot save data                                      │
│  ✗ Process cannot notify children                                │
│  ✗ May leave system in inconsistent state                        │
│  ✗ May cause data corruption                                     │
│                                                                  │
│  Always try in this order:                                       │
│  1. kill PID           (SIGTERM - graceful)                      │
│  2. Wait 5-10 seconds                                            │
│  3. kill -9 PID        (SIGKILL - if still running)              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### `killall` - Kill Processes by Name

Kill all processes with a given name:

```bash
# Kill all firefox processes
killall firefox

# Force kill all processes named python3
killall -9 python3

# Interactive mode - ask before each kill
killall -i firefox

# Only kill processes older than 1 hour
killall -o 1h process-name

# Only kill processes younger than 10 minutes
killall -y 10m process-name

# Kill processes owned by a specific user
killall -u username process-name
```

### `pkill` - Kill Processes by Pattern

Kill processes matching a pattern:

```bash
# Kill processes matching pattern
pkill firefox

# Kill with signal
pkill -9 firefox

# Kill by user
pkill -u kali

# Kill by terminal
pkill -t pts/1

# Kill the oldest matching process
pkill -o pattern

# Kill the newest matching process
pkill -n pattern

# Preview what would be killed (don't actually kill)
pkill -0 pattern && echo "Found processes"
```

### Practical Examples

```bash
# Scenario 1: Kill a hung browser
pgrep -l firefox          # Find the process
kill 1234                  # Try graceful kill
sleep 5                    # Wait
pgrep -l firefox          # Check if still running
kill -9 1234              # Force kill if necessary

# Scenario 2: Kill all processes of a crashed application
killall -9 application-name

# Scenario 3: Kill a process using a specific port (need lsof)
sudo lsof -i :8080        # Find what's using port 8080
kill $(sudo lsof -t -i :8080)  # Kill it

# Scenario 4: Kill all processes of a user
pkill -u problematic-user
```

---

## 7. Understanding Services (Daemons)

### What is a Service/Daemon?

A **daemon** (or service) is a background process that runs continuously, providing system functionality:

```
┌─────────────────────────────────────────────────────────────────┐
│                    SERVICES vs PROCESSES                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Regular Process           Service (Daemon)                      │
│  ───────────────           ────────────────                      │
│  • Started by user         • Started at boot or by systemd       │
│  • Runs in foreground      • Runs in background                  │
│  • Has a terminal (tty)    • Detached from terminal              │
│  • Exits when done         • Runs continuously                   │
│  • Interactive             • Non-interactive                     │
│                                                                  │
│  Examples of Daemons:                                            │
│  • sshd    - SSH server                                          │
│  • apache2 - Web server                                          │
│  • cron    - Task scheduler                                      │
│  • systemd - Init system and service manager                     │
│  • networkd - Network manager                                    │
│  • cups    - Printing service                                    │
│                                                                  │
│  Naming Convention:                                              │
│  Many daemons end with 'd': sshd, httpd, crond, systemd          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### systemd - The Modern Init System

Modern Linux systems (including Kali) use **systemd** to manage services:

```
┌─────────────────────────────────────────────────────────────────┐
│                    systemd OVERVIEW                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  systemd is:                                                     │
│  • PID 1 - The first process started                             │
│  • Init system - Boots the system                                │
│  • Service manager - Starts/stops services                       │
│  • Much more - logging, networking, timers, etc.                 │
│                                                                  │
│  Key Concepts:                                                   │
│  • Unit - Any resource systemd can manage                        │
│  • Service unit - A daemon/service (.service files)              │
│  • Target - A group of units (like runlevels)                    │
│                                                                  │
│  Unit Files Location:                                            │
│  /lib/systemd/system/     - System default units                 │
│  /etc/systemd/system/     - Admin customizations (higher priority) │
│  /run/systemd/system/     - Runtime units                        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 8. Managing Services with systemctl

### `systemctl status` - Check Service Status

```bash
# Check status of SSH service
sudo systemctl status ssh

# Check status of Apache
sudo systemctl status apache2

# Check status of cron
systemctl status cron
```

**Understanding status output:**

```
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: disabled)
     Active: active (running) since Mon 2024-12-02 10:00:00 UTC; 2h ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 1234 (sshd)
      Tasks: 1 (limit: 4678)
     Memory: 2.3M
        CPU: 45ms
     CGroup: /system.slice/ssh.service
             └─1234 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups

Dec 02 10:00:00 kali systemd[1]: Started OpenBSD Secure Shell server.
```

| Field | Meaning |
|-------|---------|
| Loaded | Unit file loaded, enabled/disabled at boot |
| Active | Current state (running, stopped, failed) |
| Main PID | Process ID of the main service process |
| Tasks | Number of tasks/threads |
| Memory | Memory usage |
| CGroup | Control group hierarchy |

### `systemctl start/stop/restart` - Control Services

```bash
# Start a service
sudo systemctl start ssh

# Stop a service
sudo systemctl stop ssh

# Restart a service (stop then start)
sudo systemctl restart ssh

# Reload configuration without restart
sudo systemctl reload ssh
# (not all services support reload)

# Reload or restart (reload if supported, otherwise restart)
sudo systemctl reload-or-restart ssh
```

### `systemctl enable/disable` - Boot Configuration

```bash
# Enable service to start at boot
sudo systemctl enable ssh

# Disable service from starting at boot
sudo systemctl disable ssh

# Enable AND start immediately
sudo systemctl enable --now ssh

# Disable AND stop immediately
sudo systemctl disable --now ssh
```

### Quick Status Checks

```bash
# Is the service running?
systemctl is-active ssh
# Returns: active or inactive

# Is the service enabled at boot?
systemctl is-enabled ssh
# Returns: enabled, disabled, static, masked

# Is the service failed?
systemctl is-failed ssh
# Returns: failed or active
```

### Service Management Quick Reference

```
┌─────────────────────────────────────────────────────────────────┐
│               systemctl COMMAND REFERENCE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Status & Information                                            │
│  ────────────────────                                            │
│  systemctl status service    Check service status                │
│  systemctl is-active service Check if running                    │
│  systemctl is-enabled service Check if enabled at boot           │
│  systemctl show service      Show all properties                 │
│                                                                  │
│  Control                                                         │
│  ───────                                                         │
│  systemctl start service     Start the service                   │
│  systemctl stop service      Stop the service                    │
│  systemctl restart service   Restart the service                 │
│  systemctl reload service    Reload configuration                │
│                                                                  │
│  Boot Configuration                                              │
│  ──────────────────                                              │
│  systemctl enable service    Start at boot                       │
│  systemctl disable service   Don't start at boot                 │
│  systemctl enable --now svc  Enable and start immediately        │
│                                                                  │
│  Masking (prevent any start)                                     │
│  ───────                                                         │
│  systemctl mask service      Completely disable                  │
│  systemctl unmask service    Remove the mask                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

💡 **Tip**: You can often omit `.service` - systemctl assumes it: `systemctl status ssh` equals `systemctl status ssh.service`

---

## 9. Listing and Viewing Services

### `systemctl list-units` - List Active Units

```bash
# List all active units
systemctl list-units

# List only service units
systemctl list-units --type=service

# List all units (including inactive)
systemctl list-units --all

# List all services (including inactive)
systemctl list-units --type=service --all

# List failed units
systemctl list-units --failed
# Or shorter:
systemctl --failed
```

### `systemctl list-unit-files` - List All Unit Files

```bash
# List all unit files
systemctl list-unit-files

# List only service unit files
systemctl list-unit-files --type=service

# Filter by state
systemctl list-unit-files --state=enabled
systemctl list-unit-files --state=disabled
```

### Viewing Service Logs with `journalctl`

systemd keeps logs via `journald`. View them with `journalctl`:

```bash
# View all logs
sudo journalctl

# View logs for a specific service
sudo journalctl -u ssh

# Follow logs in real-time (like tail -f)
sudo journalctl -u ssh -f

# Show logs since boot
sudo journalctl -u ssh -b

# Show last 50 lines
sudo journalctl -u ssh -n 50

# Show logs from specific time
sudo journalctl -u ssh --since "1 hour ago"
sudo journalctl -u ssh --since "2024-12-01" --until "2024-12-02"

# Show kernel messages
sudo journalctl -k

# Show errors and above
sudo journalctl -p err
```

**Priority levels for `-p`:**
| Level | Name |
|-------|------|
| 0 | emerg |
| 1 | alert |
| 2 | crit |
| 3 | err |
| 4 | warning |
| 5 | notice |
| 6 | info |
| 7 | debug |

---

## 10. System Targets (Runlevels)

### What are Targets?

Targets are groups of units that represent system states:

```
┌─────────────────────────────────────────────────────────────────┐
│                    COMMON TARGETS                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Target                Description              Old Runlevel     │
│  ──────                ───────────              ────────────     │
│  poweroff.target       System shutdown          0                │
│  rescue.target         Single user mode         1                │
│  multi-user.target     Multi-user, no GUI       3                │
│  graphical.target      Multi-user, with GUI     5                │
│  reboot.target         System reboot            6                │
│                                                                  │
│  Default Targets:                                                │
│  • Servers typically use: multi-user.target                      │
│  • Desktops typically use: graphical.target                      │
│  • Kali Linux typically uses: graphical.target                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Managing Targets

```bash
# View current default target
systemctl get-default

# Set default target (permanent)
sudo systemctl set-default multi-user.target
sudo systemctl set-default graphical.target

# Switch to a target immediately (temporary)
sudo systemctl isolate multi-user.target
sudo systemctl isolate graphical.target

# Emergency and rescue modes
sudo systemctl rescue     # Single user mode
sudo systemctl emergency  # Minimal emergency shell
```

---

## 11. Scheduling Tasks

### cron - Scheduled Recurring Tasks

`cron` is a daemon that runs scheduled tasks (called cron jobs):

```bash
# View your cron jobs
crontab -l

# Edit your cron jobs
crontab -e

# Remove all your cron jobs
crontab -r

# Edit cron jobs for another user (requires root)
sudo crontab -u username -e
```

### Cron Syntax

```
┌─────────────────────────────────────────────────────────────────┐
│                    CRON SYNTAX                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌───────────── minute (0-59)                                    │
│  │ ┌───────────── hour (0-23)                                    │
│  │ │ ┌───────────── day of month (1-31)                          │
│  │ │ │ ┌───────────── month (1-12)                               │
│  │ │ │ │ ┌───────────── day of week (0-6, 0=Sunday)              │
│  │ │ │ │ │                                                       │
│  │ │ │ │ │                                                       │
│  * * * * * command to execute                                    │
│                                                                  │
│  Special Characters:                                             │
│  * = any value           (every)                                 │
│  , = value list          (1,3,5)                                 │
│  - = range               (1-5)                                   │
│  / = step values         (*/10 = every 10)                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Common Cron Examples

```bash
# Run at minute 0 of every hour (hourly)
0 * * * * /path/to/script.sh

# Run at 2:30 AM daily
30 2 * * * /path/to/backup.sh

# Run at 9 AM on weekdays (Mon-Fri)
0 9 * * 1-5 /path/to/work-script.sh

# Run every 15 minutes
*/15 * * * * /path/to/check.sh

# Run at midnight on the 1st of every month
0 0 1 * * /path/to/monthly.sh

# Run every Sunday at 4 AM
0 4 * * 0 /path/to/weekly.sh

# Run at 6 PM on the 15th and last day of month
0 18 15,L * * /path/to/script.sh

# Run twice a day at 6 AM and 6 PM
0 6,18 * * * /path/to/script.sh
```

### Special Cron Strings

```bash
@reboot     # Run once at startup
@yearly     # Run once a year (0 0 1 1 *)
@annually   # Same as @yearly
@monthly    # Run once a month (0 0 1 * *)
@weekly     # Run once a week (0 0 * * 0)
@daily      # Run once a day (0 0 * * *)
@midnight   # Same as @daily
@hourly     # Run once an hour (0 * * * *)

# Example
@reboot /path/to/startup-script.sh
@daily /path/to/daily-backup.sh
```

### System Cron Directories

```bash
/etc/cron.d/        # Additional cron files
/etc/cron.hourly/   # Scripts run hourly
/etc/cron.daily/    # Scripts run daily
/etc/cron.weekly/   # Scripts run weekly
/etc/cron.monthly/  # Scripts run monthly
```

💡 **Tip**: Put executable scripts in these directories to run them at those intervals.

### `at` - One-Time Scheduled Tasks

For tasks you want to run once at a specific time:

```bash
# Schedule a command for a specific time
at 10:30
# Type commands, then Ctrl+D to finish

# Schedule for a specific date
at 10:30 Dec 25

# Schedule relative time
at now + 1 hour
at now + 30 minutes
at noon tomorrow
at midnight

# List pending at jobs
atq

# Remove an at job
atrm job-number

# View what an at job will do
at -c job-number
```

**Example:**

```bash
# Schedule a reminder
echo "echo 'Meeting in 10 minutes!' | wall" | at now + 50 minutes

# Schedule a backup for tonight
at 23:00 << EOF
/home/user/backup.sh
EOF

# Check scheduled jobs
atq
# 1    Mon Dec  2 23:00:00 2024 a kali
```

---

## 12. Practical Scenarios

### Scenario 1: Web Server Management

```bash
# Check if Apache is running
systemctl status apache2

# If not running, start it
sudo systemctl start apache2

# Make sure it starts on boot
sudo systemctl enable apache2

# Check the ports it's using
ss -tlnp | grep apache

# View Apache logs
sudo journalctl -u apache2 -f
```

### Scenario 2: Troubleshooting a Failed Service

```bash
# Check what services have failed
systemctl --failed

# Get details about the failed service
systemctl status failed-service.service

# View detailed logs
sudo journalctl -u failed-service.service -n 100

# Try to restart
sudo systemctl restart failed-service.service

# If still failing, check the unit file
systemctl cat failed-service.service
```

### Scenario 3: Managing SSH Service

```bash
# Check SSH status
sudo systemctl status ssh

# Start SSH
sudo systemctl start ssh

# Enable SSH to start at boot
sudo systemctl enable ssh

# Restart SSH after config changes
sudo systemctl restart ssh

# View SSH connection logs
sudo journalctl -u ssh | grep "Accepted"
```

### Scenario 4: Finding Resource-Hungry Processes

```bash
# Find processes using most CPU
ps aux --sort=-%cpu | head -10

# Find processes using most memory
ps aux --sort=-%mem | head -10

# Use top/htop for real-time monitoring
htop

# Find a specific process consuming resources
ps aux | grep process-name
```

### Scenario 5: Killing a Hung Application

```bash
# Find the process
pgrep -l application-name
# or
ps aux | grep application-name

# Try graceful termination first
kill PID

# Wait a few seconds, check if still running
sleep 5
pgrep -l application-name

# If still running, force kill
kill -9 PID

# Alternative: kill by name
killall -9 application-name
```

### Scenario 6: Setting Up a Scheduled Backup

```bash
# Create a backup script
cat > ~/backup.sh << 'EOF'
#!/bin/bash
DATE=$(date +%Y%m%d)
tar -czf /backup/home-$DATE.tar.gz /home/kali/
EOF

chmod +x ~/backup.sh

# Schedule it to run daily at 2 AM
crontab -e
# Add: 0 2 * * * /home/kali/backup.sh

# Verify
crontab -l
```

---

## Quick Reference

### Process Commands Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│              PROCESS COMMANDS REFERENCE                       │
├──────────────────────────────────────────────────────────────┤
│  VIEWING PROCESSES                                            │
│  ─────────────────                                            │
│  ps aux                List all processes (detailed)          │
│  ps -ef                List all processes (with PPID)         │
│  ps auxf               Process tree                           │
│  pgrep name            Find PID by name                       │
│  pidof program         Get PID of program                     │
│  top                   Interactive process viewer             │
│  htop                  Enhanced process viewer                │
├──────────────────────────────────────────────────────────────┤
│  PROCESS CONTROL                                              │
│  ───────────────                                              │
│  command &             Run in background                      │
│  Ctrl+Z                Suspend foreground process             │
│  Ctrl+C                Terminate foreground process           │
│  jobs                  List background jobs                   │
│  fg %n                 Bring job n to foreground              │
│  bg %n                 Resume job n in background             │
│  nohup cmd &           Run immune to hangup                   │
├──────────────────────────────────────────────────────────────┤
│  KILLING PROCESSES                                            │
│  ─────────────────                                            │
│  kill PID              Send SIGTERM (graceful)                │
│  kill -9 PID           Send SIGKILL (force)                   │
│  killall name          Kill by name                           │
│  pkill pattern         Kill by pattern                        │
├──────────────────────────────────────────────────────────────┤
│  PRIORITY                                                     │
│  ────────                                                     │
│  nice -n 10 cmd        Start with nice value 10               │
│  renice 10 -p PID      Change nice value of process           │
└──────────────────────────────────────────────────────────────┘
```

### Service Commands Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│              SERVICE COMMANDS REFERENCE                       │
├──────────────────────────────────────────────────────────────┤
│  STATUS                                                       │
│  ──────                                                       │
│  systemctl status svc      Check service status               │
│  systemctl is-active svc   Is it running?                     │
│  systemctl is-enabled svc  Is it enabled at boot?             │
│  systemctl --failed        List failed services               │
├──────────────────────────────────────────────────────────────┤
│  CONTROL                                                      │
│  ───────                                                      │
│  systemctl start svc       Start service                      │
│  systemctl stop svc        Stop service                       │
│  systemctl restart svc     Restart service                    │
│  systemctl reload svc      Reload configuration               │
├──────────────────────────────────────────────────────────────┤
│  BOOT CONFIGURATION                                           │
│  ──────────────────                                           │
│  systemctl enable svc      Start at boot                      │
│  systemctl disable svc     Don't start at boot                │
│  systemctl enable --now    Enable and start                   │
├──────────────────────────────────────────────────────────────┤
│  LISTING                                                      │
│  ───────                                                      │
│  systemctl list-units --type=service    List services         │
│  systemctl list-unit-files              List all unit files   │
├──────────────────────────────────────────────────────────────┤
│  LOGS                                                         │
│  ────                                                         │
│  journalctl -u svc         View service logs                  │
│  journalctl -u svc -f      Follow logs                        │
│  journalctl -u svc -n 50   Last 50 lines                      │
└──────────────────────────────────────────────────────────────┘
```

### Cron Syntax Quick Reference

```
┌──────────────────────────────────────────────────────────────┐
│              CRON SCHEDULE EXAMPLES                           │
├──────────────────────────────────────────────────────────────┤
│  * * * * *        Every minute                                │
│  0 * * * *        Every hour (at minute 0)                    │
│  0 0 * * *        Every day at midnight                       │
│  0 0 * * 0        Every Sunday at midnight                    │
│  0 0 1 * *        First day of month at midnight              │
│  */15 * * * *     Every 15 minutes                            │
│  0 9-17 * * 1-5   9AM-5PM on weekdays (hourly)                │
│  30 4 * * *       4:30 AM daily                               │
│                                                               │
│  @reboot          Once at startup                             │
│  @hourly          Once an hour                                │
│  @daily           Once a day                                  │
│  @weekly          Once a week                                 │
│  @monthly         Once a month                                │
└──────────────────────────────────────────────────────────────┘
```

---

## ✅ What You Learned

In this module, you learned:

- [x] What processes are and how they differ from programs
- [x] How to view processes with `ps`, `top`, and `htop`
- [x] Understanding PIDs, PPIDs, and process states
- [x] How to manage process priorities with `nice` and `renice`
- [x] How to run processes in background and foreground
- [x] How to kill/terminate processes safely with signals
- [x] What services (daemons) are and how they work
- [x] How to manage services with `systemctl`
- [x] How to view service logs with `journalctl`
- [x] Understanding system targets (runlevels)
- [x] How to schedule tasks with `cron` and `at`
- [x] Practical system administration scenarios

---

## Next Steps

In the next module, we'll explore **shell scripting** - how to automate tasks by writing bash scripts, using variables, conditionals, loops, and more.

Practice the exercises to reinforce your understanding of process and service management!