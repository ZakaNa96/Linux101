# Module 4: Exercises - Viewing and Editing Text

Practice these exercises to master text viewing and editing in Linux. Each exercise builds on skills from the lesson.

---

## Exercise 1: View System Files

**Objective:** Practice viewing file contents with `cat` and related commands.

### Tasks:

1. **View the hostname file:**
   ```bash
   cat /etc/hostname
   ```
   What is your system's hostname?

2. **View the passwd file with line numbers:**
   ```bash
   cat -n /etc/passwd
   ```
   How many lines are in the file?

3. **View the hosts file:**
   ```bash
   cat /etc/hosts
   ```
   What IP address is associated with "localhost"?

4. **Try viewing a large file with cat:**
   ```bash
   cat /var/log/syslog
   ```
   What happens? (The terminal floods with text!)

5. **View a file in reverse order:**
   ```bash
   tac /etc/passwd
   ```
   Which user appears first now?

### Expected Results:

- `/etc/hostname` shows your computer's name (e.g., "kali")
- `/etc/passwd` typically has 20-50+ lines
- localhost is mapped to `127.0.0.1`
- Large files flood the terminal when using `cat`
- `tac` shows the file from last line to first

💡 **Tip:** `cat` is great for small files, but use `less` for larger ones!

---

## Exercise 2: Navigate Large Files

**Objective:** Master navigation in large files using `less`, `head`, and `tail`.

### Tasks:

1. **Open a log file with less:**
   ```bash
   less /var/log/syslog
   ```
   Or if that doesn't exist:
   ```bash
   less /var/log/messages
   ```
   Or:
   ```bash
   less /var/log/dpkg.log
   ```

2. **Practice navigation in less:**
   - Press `Space` to go forward one page
   - Press `b` to go backward one page
   - Press `g` to go to the beginning
   - Press `G` to go to the end
   - Press `q` to quit

3. **Search within less:**
   - Open the file again with `less`
   - Press `/` and type `error` then `Enter`
   - Press `n` to find the next occurrence
   - Press `N` to find the previous occurrence

4. **View the first 20 lines of passwd:**
   ```bash
   head -n 20 /etc/passwd
   ```

5. **View the last 50 lines of a log file:**
   ```bash
   tail -n 50 /var/log/syslog
   ```
   Or:
   ```bash
   tail -n 50 /var/log/dpkg.log
   ```

6. **Watch a log file in real-time:**
   ```bash
   tail -f /var/log/syslog
   ```
   Open another terminal and do something (like run `ls`). Do you see new entries appear?
   
   Press `Ctrl+C` to stop watching.

### Expected Results:

- `less` allows smooth navigation through large files
- Searching finds highlighted matches
- `head` shows only the beginning
- `tail` shows only the end
- `tail -f` updates in real-time as new log entries appear

💡 **Tip:** `tail -f` is invaluable for monitoring logs while troubleshooting!

---

## Exercise 3: Search with grep

**Objective:** Learn to search for patterns in files using `grep`.

### Tasks:

1. **Find your username in passwd:**
   ```bash
   grep $USER /etc/passwd
   ```
   Or replace `$USER` with your actual username:
   ```bash
   grep kali /etc/passwd
   ```

2. **Find all lines containing "root":**
   ```bash
   grep root /etc/passwd
   ```
   How many lines contain "root"?

3. **Case-insensitive search:**
   ```bash
   grep -i ROOT /etc/passwd
   ```
   Does it still find matches?

4. **Show line numbers:**
   ```bash
   grep -n bash /etc/passwd
   ```
   Which line numbers have "bash"?

5. **Count users in passwd:**
   ```bash
   wc -l /etc/passwd
   ```
   How many users (lines) are there?

6. **Search recursively in /etc:**
   ```bash
   grep -r "nameserver" /etc/ 2>/dev/null
   ```
   Which file contains nameserver entries?

   ⚠️ Note: `2>/dev/null` hides error messages for files you can't read.

7. **Find lines NOT containing a pattern:**
   ```bash
   grep -v nologin /etc/passwd
   ```
   These are users who CAN log in (don't have "nologin" shell).

8. **Combine options:**
   ```bash
   grep -in "error" /var/log/syslog 2>/dev/null | head -20
   ```
   This searches case-insensitively, shows line numbers, and displays only the first 20 matches.

### Expected Results:

- Your user entry shows your home directory and shell
- Multiple lines may contain "root"
- Case-insensitive search finds the same results
- Line numbers help locate matches
- `/etc/passwd` typically has 20-50 users
- `/etc/resolv.conf` contains nameserver entries
- `-v` inverts the search (shows non-matches)

💡 **Tip:** `grep` is one of the most useful Linux commands. Master it!

---

## Exercise 4: Pipes and Redirection

**Objective:** Practice combining commands with pipes and saving output to files.

### Tasks:

1. **Save a file listing to a text file:**
   ```bash
   ls -la > filelist.txt
   cat filelist.txt
   ```

2. **Append to a file:**
   ```bash
   echo "--- Generated on $(date) ---" >> filelist.txt
   tail -3 filelist.txt
   ```

3. **Count users with bash shell:**
   ```bash
   grep bash /etc/passwd | wc -l
   ```
   How many users have bash as their shell?

4. **Sort and count unique shells:**
   ```bash
   cat /etc/passwd | cut -d: -f7 | sort | uniq -c
   ```
   This extracts the shell field (7th), sorts it, and counts each unique value.

5. **Find and sort users:**
   ```bash
   cat /etc/passwd | cut -d: -f1 | sort
   ```
   This lists all usernames in alphabetical order.

6. **Create a file with echo:**
   ```bash
   echo "Line 1" > myfile.txt
   echo "Line 2" >> myfile.txt
   echo "Line 3" >> myfile.txt
   cat myfile.txt
   ```

7. **Chain multiple commands:**
   ```bash
   cat /etc/passwd | grep -v nologin | cut -d: -f1 | sort > real_users.txt
   cat real_users.txt
   ```
   This finds users who can log in and saves the sorted list.

8. **View long output with less:**
   ```bash
   ls -la /etc | less
   ```
   Navigate through the listing, then quit with `q`.

### Expected Results:

- `filelist.txt` contains your directory listing
- Date is appended to the file
- Typically 1-5 users have bash shell
- You'll see counts for different shells like `/bin/bash`, `/usr/sbin/nologin`, etc.
- Files are created/appended correctly

⚠️ **Warning:** Remember: `>` overwrites, `>>` appends!

---

## Exercise 5: Edit with nano

**Objective:** Create and edit files using the nano text editor.

### Tasks:

1. **Create a new file:**
   ```bash
   nano my-notes.txt
   ```

2. **Add some content:**
   Type the following:
   ```
   My Linux Learning Notes
   =======================
   
   Today I learned:
   - How to view files with cat and less
   - How to search with grep
   - How to use pipes and redirection
   
   Commands to remember:
   - cat, less, head, tail
   - grep, wc, sort, uniq
   - nano for editing
   ```

3. **Save the file:**
   - Press `Ctrl+O` (Write Out)
   - Press `Enter` to confirm the filename
   - You should see "Wrote X lines" at the bottom

4. **Exit nano:**
   - Press `Ctrl+X`

5. **Verify the file was created:**
   ```bash
   cat my-notes.txt
   ```

6. **Re-open and add more content:**
   ```bash
   nano my-notes.txt
   ```
   
   Add at the bottom:
   ```
   
   Additional notes:
   - Practice makes perfect!
   - Don't be afraid to experiment
   ```
   
   Save (`Ctrl+O`, `Enter`) and exit (`Ctrl+X`).

7. **Practice cut and paste:**
   ```bash
   nano my-notes.txt
   ```
   - Move to a line you want to cut
   - Press `Ctrl+K` to cut the line
   - Move to where you want to paste
   - Press `Ctrl+U` to paste
   - Save and exit

8. **Practice search:**
   ```bash
   nano my-notes.txt
   ```
   - Press `Ctrl+W`
   - Type "grep" and press `Enter`
   - The cursor moves to "grep"
   - Exit without saving: `Ctrl+X`, then `N`

### Expected Results:

- File `my-notes.txt` is created with your content
- You can add, edit, cut, paste, and search text
- The file saves correctly each time
- You can exit with or without saving changes

💡 **Tip:** Look at the bottom of nano for available commands. `^` means Ctrl!

---

## Exercise 6: Survive vim

**Objective:** Learn basic vim commands to edit files and - most importantly - exit vim!

### Tasks:

1. **Open vim with a new file:**
   ```bash
   vim vim-test.txt
   ```
   You're now in Normal mode. Notice you can't type text yet!

2. **Enter Insert mode:**
   - Press `i`
   - Notice "-- INSERT --" appears at the bottom
   - Now type: `Hello from vim!`

3. **Add more lines:**
   - Press `Enter` for a new line
   - Type: `This is my first vim file.`
   - Press `Enter`
   - Type: `I can now exit vim properly!`

4. **Return to Normal mode:**
   - Press `Esc`
   - Notice "-- INSERT --" disappears

5. **Save the file:**
   - Type `:w` (you'll see it appear at the bottom)
   - Press `Enter`
   - You should see "vim-test.txt" [New] 3L, 69C written" or similar

6. **Quit vim:**
   - Type `:q`
   - Press `Enter`
   - You're back at the terminal!

7. **Verify the file:**
   ```bash
   cat vim-test.txt
   ```

8. **Edit the file again:**
   ```bash
   vim vim-test.txt
   ```
   - Press `i` for Insert mode
   - Add a new line: `Adding more content...`
   - Press `Esc`
   - Type `:wq` to save and quit in one command
   - Press `Enter`

9. **Practice quitting without saving:**
   ```bash
   vim vim-test.txt
   ```
   - Press `i` for Insert mode
   - Type: `This line will NOT be saved!`
   - Press `Esc`
   - Type `:q!` to force quit without saving
   - Press `Enter`
   
   ```bash
   cat vim-test.txt
   ```
   The "will NOT be saved" line shouldn't be there!

### Expected Results:

- You can create files in vim
- You understand Normal vs Insert mode
- You can save with `:w`
- You can quit with `:q`
- You can save and quit with `:wq`
- You can quit without saving with `:q!`
- The file contains only saved content

⚠️ **If you get stuck:** Press `Esc` multiple times, then type `:q!` and press `Enter`

💡 **Tip:** Run `vimtutor` for a comprehensive vim tutorial!

---

## Exercise 7: Challenge - Log Analysis

**Objective:** Apply all your skills to analyze a mock log file.

### Setup: Create a Mock Log File

First, create a sample log file to analyze:

```bash
cat > sample.log << 'EOF'
2024-01-15 08:00:01 INFO Server started successfully
2024-01-15 08:00:02 INFO Listening on port 8080
2024-01-15 08:01:15 INFO User login: john from 192.168.1.100
2024-01-15 08:02:30 WARNING High memory usage detected
2024-01-15 08:03:45 INFO User login: jane from 192.168.1.101
2024-01-15 08:05:00 ERROR Database connection failed
2024-01-15 08:05:01 INFO Retrying database connection...
2024-01-15 08:05:05 INFO Database connection restored
2024-01-15 08:10:00 INFO User login: john from 192.168.1.100
2024-01-15 08:15:30 WARNING Disk space low on /var
2024-01-15 08:20:00 ERROR Authentication failed for user admin from 10.0.0.50
2024-01-15 08:20:01 WARNING Multiple failed login attempts
2024-01-15 08:25:00 INFO User logout: john
2024-01-15 08:30:00 INFO User login: mary from 192.168.1.102
2024-01-15 08:35:00 ERROR Connection timeout from 10.0.0.50
2024-01-15 08:40:00 INFO Scheduled backup started
2024-01-15 08:45:00 INFO Backup completed successfully
2024-01-15 08:50:00 WARNING CPU usage above 80%
2024-01-15 08:55:00 INFO User logout: jane
2024-01-15 09:00:00 INFO Server health check: OK
EOF
```

### Tasks:

1. **View the entire log:**
   ```bash
   cat sample.log
   ```

2. **Find all ERROR entries:**
   ```bash
   grep ERROR sample.log
   ```
   
   **Expected output:**
   ```
   2024-01-15 08:05:00 ERROR Database connection failed
   2024-01-15 08:20:00 ERROR Authentication failed for user admin from 10.0.0.50
   2024-01-15 08:35:00 ERROR Connection timeout from 10.0.0.50
   ```

3. **Count WARNING entries:**
   ```bash
   grep -c WARNING sample.log
   ```
   
   **Expected output:** `4`

4. **Find all entries from IP 10.0.0.50:**
   ```bash
   grep "10.0.0.50" sample.log
   ```
   
   How many entries involve this IP? (Answer: 2)

5. **Extract all unique IP addresses:**
   ```bash
   grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" sample.log | sort | uniq
   ```
   
   **Expected output:**
   ```
   10.0.0.50
   192.168.1.100
   192.168.1.101
   192.168.1.102
   ```

6. **Count logins per user:**
   ```bash
   grep "User login" sample.log | grep -oE "login: [a-z]+" | sort | uniq -c
   ```
   
   **Expected output:**
   ```
         2 login: john
         1 login: jane
         1 login: mary
   ```

7. **Save all errors and warnings to a new file:**
   ```bash
   grep -E "ERROR|WARNING" sample.log > issues.log
   cat issues.log
   ```

8. **Find entries between 08:00 and 08:30:**
   ```bash
   grep "08:[0-2][0-9]:" sample.log
   ```

9. **Create a summary report:**
   ```bash
   echo "=== Log Analysis Report ===" > report.txt
   echo "" >> report.txt
   echo "Total entries: $(wc -l < sample.log)" >> report.txt
   echo "INFO entries: $(grep -c INFO sample.log)" >> report.txt
   echo "WARNING entries: $(grep -c WARNING sample.log)" >> report.txt
   echo "ERROR entries: $(grep -c ERROR sample.log)" >> report.txt
   echo "" >> report.txt
   echo "=== ERRORS ===" >> report.txt
   grep ERROR sample.log >> report.txt
   echo "" >> report.txt
   echo "=== WARNINGS ===" >> report.txt
   grep WARNING sample.log >> report.txt
   
   cat report.txt
   ```

### Challenge Questions:

1. What time did the first error occur?
   <details>
   <summary>Answer</summary>
   08:05:00 - Database connection failed
   </details>

2. How many total log entries are there?
   <details>
   <summary>Answer</summary>
   20 entries
   </details>

3. Which IP address appears most in the logs?
   <details>
   <summary>Answer</summary>
   192.168.1.100 (john's IP) - appears twice for login
   </details>

4. What was the last action recorded?
   <details>
   <summary>Answer</summary>
   09:00:00 - Server health check: OK
   </details>

### Clean Up:

```bash
rm sample.log issues.log report.txt
```

💡 **Tip:** These log analysis skills are essential for system administration and troubleshooting!

---

## 🎯 Skills Checklist

After completing these exercises, you should be able to:

- [ ] View files with `cat`, `tac`, `less`, `more`
- [ ] View specific portions with `head` and `tail`
- [ ] Monitor files in real-time with `tail -f`
- [ ] Search files with `grep` and its options
- [ ] Count lines, words, and characters with `wc`
- [ ] Sort and find unique lines with `sort` and `uniq`
- [ ] Redirect output with `>` and `>>`
- [ ] Chain commands with pipes (`|`)
- [ ] Create and edit files with `nano`
- [ ] Navigate nano and use its keyboard shortcuts
- [ ] Enter and exit vim properly
- [ ] Use vim's Insert and Normal modes
- [ ] Save and quit in vim (`:w`, `:q`, `:wq`, `:q!`)
- [ ] Analyze log files using combinations of commands

---

## 🏆 Bonus Challenges

1. **Find the 5 largest files in /etc:**
   ```bash
   ls -laS /etc | head -6
   ```

2. **Count how many configuration files end in .conf:**
   ```bash
   find /etc -name "*.conf" 2>/dev/null | wc -l
   ```

3. **Find all files modified today:**
   ```bash
   find /var/log -mtime 0 2>/dev/null
   ```

4. **Create a command alias in nano:**
   ```bash
   nano ~/.bashrc
   ```
   Add: `alias ll='ls -la'`
   Save, exit, then: `source ~/.bashrc`
   Now try: `ll`

---

Congratulations on completing Module 4! You now have essential skills for viewing and editing text files in Linux. These skills are fundamental for system administration, development, and everyday Linux use.