# Module 2 Exercises: Navigating the File System

🎯 **Goal**: Practice navigating the Linux file system using the commands you learned in the lesson.

⏱️ **Estimated Time**: 45-60 minutes

---

## Before You Start

Make sure you:
1. Have your Kali Linux virtual machine running
2. Have a terminal open
3. Completed the Module 2 lesson

💡 **Tip**: Keep the lesson open in another window as a reference!

---

## Exercise 1: Explore the Root Directory

🎯 **Goal**: Get familiar with the Linux file system structure by exploring from the root.

### Tasks

1. **Find out where you are right now**
   ```bash
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /home/kali
   ```
   (or wherever you currently are)
   </details>

2. **Go to the root directory**
   ```bash
   cd /
   ```

3. **List all directories in root**
   ```bash
   ls
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
   boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
   ```
   (Your output may vary slightly depending on your Kali version)
   </details>

4. **List with more details**
   ```bash
   ls -l
   ```

5. **Identify 5 important directories and write down their purpose:**

   | Directory | Purpose |
   |-----------|---------|
   | `/home` | _________________________ |
   | `/etc` | _________________________ |
   | `/var` | _________________________ |
   | `/tmp` | _________________________ |
   | `/root` | _________________________ |

   <details>
   <summary>✅ Solution</summary>
   
   | Directory | Purpose |
   |-----------|---------|
   | `/home` | User home directories |
   | `/etc` | System configuration files |
   | `/var` | Variable data (logs, databases) |
   | `/tmp` | Temporary files |
   | `/root` | Root user's home directory |
   </details>

6. **Return to your home directory**
   ```bash
   cd ~
   ```

7. **Verify you're home**
   ```bash
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /home/kali
   ```
   </details>

### ✅ Exercise 1 Complete!
You explored the root directory and identified key system directories.

---

## Exercise 2: Path Practice

🎯 **Goal**: Master absolute and relative paths, and special path symbols.

### Tasks

1. **Navigate to `/etc` using an absolute path**
   ```bash
   cd /etc
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /etc
   ```
   </details>

2. **List the contents of `/etc`**
   ```bash
   ls
   ```
   
   💡 **Notice**: You'll see many configuration files here!

3. **Navigate to `/var/log` using an absolute path**
   ```bash
   cd /var/log
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /var/log
   ```
   </details>

4. **Go up one directory using `..`**
   ```bash
   cd ..
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /var
   ```
   </details>

5. **Go up two directories using `../..`**
   ```bash
   cd ../..
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /
   ```
   </details>

6. **Go back to your home directory using `~`**
   ```bash
   cd ~
   pwd
   ```

7. **Navigate to `/etc` again**
   ```bash
   cd /etc
   ```

8. **Use `cd -` to go back to your previous location (home)**
   ```bash
   cd -
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /home/kali
   /home/kali
   ```
   (The first line shows where you're going, the second confirms it)
   </details>

9. **Use `cd -` again to switch back to `/etc`**
   ```bash
   cd -
   pwd
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   /etc
   /etc
   ```
   </details>

10. **Return home using just `cd` (no arguments)**
    ```bash
    cd
    pwd
    ```
    
    <details>
    <summary>📋 Expected Output</summary>
    
    ```
    /home/kali
    ```
    </details>

### ✅ Exercise 2 Complete!
You practiced using absolute paths, relative paths, and special path symbols (`.`, `..`, `~`, `-`).

---

## Exercise 3: Master the `ls` Command

🎯 **Goal**: Learn different ways to list and examine directory contents.

### Tasks

1. **Go to your home directory and list contents**
   ```bash
   cd ~
   ls
   ```
   
   <details>
   <summary>📋 Expected Output (example)</summary>
   
   ```
   Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
   ```
   </details>

2. **List with hidden files (files starting with `.`)**
   ```bash
   ls -a
   ```
   
   <details>
   <summary>📋 Expected Output (example)</summary>
   
   ```
   .  ..  .bashrc  .config  .local  Desktop  Documents  Downloads  ...
   ```
   
   💡 **Notice**: Files starting with `.` are hidden files. They include:
   - `.` (current directory)
   - `..` (parent directory)
   - `.bashrc` (bash configuration)
   - `.config` (configuration directory)
   </details>

3. **List in long format**
   ```bash
   ls -l
   ```
   
   **Look at the output and identify:**
   - Which items are directories? (start with `d`)
   - Which items are files? (start with `-`)
   - What is the size of each item?
   - When was each item last modified?

4. **Combine flags: long format + hidden files**
   ```bash
   ls -la
   ```

5. **Add human-readable sizes**
   ```bash
   ls -lah
   ```
   
   💡 **Notice**: Sizes now show in KB, MB, GB instead of bytes!

6. **List the `/etc` directory without going there**
   ```bash
   ls /etc
   ```

7. **List `/etc` with full details**
   ```bash
   ls -la /etc
   ```

8. **Find large files in `/var/log`**
   ```bash
   ls -lhS /var/log
   ```
   
   💡 **Note**: The `-S` flag sorts by size (largest first)
   
   **Question**: What is the largest log file you can see?
   
   <details>
   <summary>💡 Hint</summary>
   
   The largest files are usually `syslog`, `auth.log`, or similar system logs. Your results will vary based on your system's activity.
   </details>

9. **List only directories in your home**
   ```bash
   cd ~
   ls -d */
   ```
   
   <details>
   <summary>📋 Expected Output (example)</summary>
   
   ```
   Desktop/  Documents/  Downloads/  Music/  Pictures/  Public/  Templates/  Videos/
   ```
   </details>

10. **List files sorted by modification time (newest first)**
    ```bash
    ls -lt /var/log
    ```

### ✅ Exercise 3 Complete!
You mastered different ways to list directory contents with `ls`.

---

## Exercise 4: Tab Completion Challenge

🎯 **Goal**: Use Tab completion to navigate faster and more accurately.

### Tasks

1. **Navigate to `/etc/` using Tab completion**
   
   Type this, then press Tab:
   ```bash
   cd /et
   ```
   
   <details>
   <summary>📋 Expected Result</summary>
   
   It should complete to: `cd /etc/`
   </details>

2. **Find files in `/etc` that start with "pass"**
   
   Type this, then press Tab twice:
   ```bash
   ls /etc/pass
   ```
   
   <details>
   <summary>📋 Expected Output</summary>
   
   ```
   passwd   passwd-
   ```
   (Press Tab twice to see all options)
   </details>

3. **Navigate to your Documents folder using Tab**
   
   Type this, then press Tab:
   ```bash
   cd ~/Doc
   ```
   
   <details>
   <summary>📋 Expected Result</summary>
   
   It should complete to: `cd ~/Documents/`
   </details>

4. **Explore `/var/log` using Tab**
   
   Type this, then press Tab twice:
   ```bash
   ls /var/log/
   ```
   
   <details>
   <summary>📋 Expected Result</summary>
   
   You should see a list of all log files and directories in `/var/log/`
   </details>

5. **Tab complete a long path step by step**
   
   Try to navigate to `/usr/share/doc` using Tab at each step:
   ```bash
   cd /us<Tab>/sh<Tab>/do<Tab>
   ```
   
   <details>
   <summary>📋 Expected Result</summary>
   
   It should expand to: `cd /usr/share/doc/`
   </details>

6. **What happens when Tab doesn't complete?**
   
   Type this, then press Tab:
   ```bash
   cd /var/l
   ```
   
   <details>
   <summary>📋 Expected Result</summary>
   
   It might complete to `cd /var/log/` or you might hear a beep/see no change if there are multiple matches. Press Tab again to see all options (like `lib`, `local`, `lock`, `log`).
   </details>

### ✅ Exercise 4 Complete!
You learned to use Tab completion for faster navigation!

---

## Exercise 5: History and Efficiency

🎯 **Goal**: Use command history to work more efficiently.

### Tasks

1. **View your command history**
   ```bash
   history
   ```
   
   💡 **Notice**: Each command has a number next to it.

2. **View only the last 10 commands**
   ```bash
   history 10
   ```

3. **Run a command from history by number**
   
   Find a `cd` command in your history and run it using `!number`:
   ```bash
   !5
   ```
   (Replace `5` with the actual number from your history)

4. **Run the last command again**
   ```bash
   !!
   ```
   
   <details>
   <summary>💡 Explanation</summary>
   
   `!!` runs whatever your last command was. This is useful when you forgot `sudo`:
   ```bash
   cat /etc/shadow      # Permission denied
   sudo !!              # Runs: sudo cat /etc/shadow
   ```
   </details>

5. **Use the Up arrow to navigate history**
   
   Press the `↑` (Up arrow) key several times to see previous commands. Press `↓` (Down arrow) to go forward. Press Enter to execute the selected command.

6. **Search history with Ctrl+R**
   
   a. Press `Ctrl + R`
   
   b. Type `ls` (or part of a command you used)
   
   c. You should see a matching command appear
   
   d. Press `Ctrl + R` again to find older matches
   
   e. Press `Enter` to execute, or `Ctrl + C` to cancel

7. **Run the last command that started with 'cd'**
   ```bash
   !cd
   ```
   
   <details>
   <summary>📋 Expected Result</summary>
   
   This runs the most recent command that started with "cd"
   </details>

8. **Clear your terminal (from Module 1)**
   ```bash
   clear
   ```
   
   Or press `Ctrl + L`

### ✅ Exercise 5 Complete!
You learned to use history to work faster and repeat commands!

---

## Exercise 6: Scavenger Hunt 🏆

🎯 **Goal**: Use everything you learned to find specific files and directories.

This exercise tests all your navigation skills. Find each item and write down its full path.

### The Hunt

1. **Find the hostname file**
   
   💡 **Hint**: Configuration files are in `/etc`
   
   ```bash
   # Try these commands to find it:
   ls /etc/host*
   ```
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /etc/hostname
   ```
   
   You can view it with:
   ```bash
   cat /etc/hostname
   ```
   </details>

2. **Find the system's password file**
   
   💡 **Hint**: It's in `/etc` and starts with "pass"
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /etc/passwd
   ```
   
   View the first few lines:
   ```bash
   head /etc/passwd
   ```
   </details>

3. **Find where Apache web server configuration would be stored**
   
   💡 **Hint**: Look in `/etc` for a directory named "apache2"
   
   ```bash
   ls -d /etc/apache*
   ```
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /etc/apache2/
   ```
   
   (This directory exists if Apache is installed. On Kali, it might not be there by default.)
   </details>

4. **Find the main system log directory**
   
   💡 **Hint**: Variable data is in `/var`
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /var/log/
   ```
   
   List the logs:
   ```bash
   ls -la /var/log
   ```
   </details>

5. **Find your user's Desktop folder**
   
   💡 **Hint**: It's in your home directory
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /home/kali/Desktop
   ```
   
   Or using ~:
   ```bash
   ~/Desktop
   ```
   </details>

6. **Find the system's DNS configuration**
   
   💡 **Hint**: It's in `/etc` and the filename contains "resolv"
   
   ```bash
   ls /etc/resolv*
   ```
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /etc/resolv.conf
   ```
   
   View DNS servers:
   ```bash
   cat /etc/resolv.conf
   ```
   </details>

7. **Find where temporary files are stored**
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /tmp/
   ```
   
   List contents:
   ```bash
   ls -la /tmp
   ```
   </details>

8. **Find the kernel's boot files**
   
   💡 **Hint**: Look for a "boot" directory in root
   
   **Full path**: _______________________
   
   <details>
   <summary>✅ Solution</summary>
   
   ```bash
   /boot/
   ```
   
   List kernel files:
   ```bash
   ls -la /boot
   ```
   
   You should see files like `vmlinuz-*` (the Linux kernel)
   </details>

### 🏆 Bonus Challenge

Find a file called `sources.list` that contains package repository information.

💡 **Hint**: Look in `/etc/apt/`

<details>
<summary>✅ Solution</summary>

```bash
/etc/apt/sources.list
```

View it:
```bash
cat /etc/apt/sources.list
```
</details>

### ✅ Exercise 6 Complete!
Congratulations! You've completed the Scavenger Hunt!

---

## 🎉 Module 2 Exercises Complete!

### What You Practiced

| Exercise | Skills |
|----------|--------|
| Exercise 1 | Exploring root, identifying important directories |
| Exercise 2 | Absolute paths, relative paths, special symbols |
| Exercise 3 | Using `ls` with different options |
| Exercise 4 | Tab completion for efficiency |
| Exercise 5 | Command history and shortcuts |
| Exercise 6 | Real-world navigation and file finding |

### Self-Assessment Questions

Answer these questions to check your understanding:

1. What command shows your current directory?
   <details><summary>Answer</summary>`pwd`</details>

2. How do you go to your home directory? (3 ways)
   <details><summary>Answer</summary>`cd ~`, `cd`, or `cd /home/kali`</details>

3. What does `cd -` do?
   <details><summary>Answer</summary>Goes to the previous directory you were in</details>

4. How do you see hidden files with `ls`?
   <details><summary>Answer</summary>`ls -a` or `ls -la`</details>

5. What does pressing Tab twice do?
   <details><summary>Answer</summary>Shows all possible completions</details>

6. How do you search through command history?
   <details><summary>Answer</summary>`Ctrl + R`, then type part of the command</details>

---

## Next Steps

Excellent work! You're now comfortable navigating the Linux file system. 

In **Module 3**, you'll learn to:
- Create files and directories
- Copy, move, and rename files
- Delete files safely
- View and edit file contents

⚠️ **Warning**: In Module 3, you'll learn about `rm` (remove). NEVER run `rm -rf /` or similar commands that delete system files. Always double-check before deleting!

---

## Additional Practice (Optional)

If you want more practice, try these additional tasks:

1. **Create a mental map**: From memory, draw the Linux file system hierarchy and label the purpose of each main directory.

2. **Speed challenge**: How fast can you navigate from `/var/log` to `/home/kali/Documents` and back? (Use Tab completion!)

3. **History challenge**: Find all commands in your history that contain "etc" using `Ctrl+R`.

4. **Exploration**: Navigate to `/proc` and list its contents. This special directory shows running processes!
   ```bash
   cd /proc
   ls
   ```
   Each number is a process ID (PID)!