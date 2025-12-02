# Module 1: Exercises - Introduction to Linux and the Terminal

🎯 **Exercise Goals:**
- Practice opening the terminal
- Use basic commands confidently
- Learn to get help when stuck
- Build muscle memory for common commands

---

## Before You Begin

> 💡 **Tip:** Open this file side-by-side with your terminal so you can follow along easily.

**How to approach these exercises:**
1. Read each task carefully
2. Try to complete it on your own first
3. If stuck, check the hints
4. Compare your output with the expected output
5. Solutions are at the bottom (but try first!)

---

## Exercise 1: Meet Your System 👋

🎯 **Goal:** Learn to open the terminal and discover basic information about your system.

### Tasks:

**1.1** Open the terminal using the keyboard shortcut.

<details>
<summary>💡 Hint</summary>

Press `Ctrl + Alt + T`

</details>

---

**1.2** Find out your username. What command do you use?

Write the command here: `________________`

**Expected Output:**
```
kali
```
(Your output might show a different username)

---

**1.3** Find out your computer's name (hostname).

Write the command here: `________________`

**Expected Output:**
```
kali
```

---

**1.4** Display the current date and time.

Write the command here: `________________`

**Expected Output (example):**
```
Mon Dec  2 10:15:30 CET 2024
```

---

**1.5** Clear the terminal screen.

Write the command here: `________________`

<details>
<summary>💡 Hint</summary>

You can use either the command `clear` or the keyboard shortcut `Ctrl + L`

</details>

---

### ✅ Exercise 1 Checklist:
- [ ] I opened the terminal with `Ctrl + Alt + T`
- [ ] I know my username
- [ ] I know my computer's hostname
- [ ] I can display the date and time
- [ ] I can clear the screen

---

## Exercise 2: Explore the Calendar 📅

🎯 **Goal:** Learn to use command options and arguments with the `cal` command.

### Tasks:

**2.1** Show the current month's calendar.

Write the command here: `________________`

**Expected Output (December 2024):**
```
   December 2024
Su Mo Tu We Th Fr Sa
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31
```

---

**2.2** Show the calendar for **December 2024** specifically (even if it's not the current month).

Write the command here: `________________`

<details>
<summary>💡 Hint</summary>

Use two arguments: month and year. Example: `cal 6 2024` shows June 2024.

</details>

---

**2.3** Show the calendar for the **entire year 2025**.

Write the command here: `________________`

<details>
<summary>💡 Hint</summary>

Use `cal --help` to find the option for showing a full year, OR just provide the year as an argument.

</details>

**Expected Output (partial):**
```
                            2025
      January               February               March
Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa  Su Mo Tu We Th Fr Sa
          1  2  3  4                     1                     1
 5  6  7  8  9 10 11   2  3  4  5  6  7  8   2  3  4  5  6  7  8
...
```

---

**2.4** **Bonus Challenge:** Show a calendar with the week starting on **Monday** instead of Sunday.

Write the command here: `________________`

<details>
<summary>💡 Hint</summary>

Check `cal --help` for an option related to Monday. Look for `-m` or `--monday`.

</details>

---

### ✅ Exercise 2 Checklist:
- [ ] I can show the current month's calendar
- [ ] I can show a specific month and year
- [ ] I can show an entire year
- [ ] I understand how to use options with commands

---

## Exercise 3: First Echo 🔊

🎯 **Goal:** Learn to print text to the terminal using the `echo` command.

### Tasks:

**3.1** Print your name to the terminal.

Example command: `echo "Max Mustermann"`

Write YOUR command here: `________________`

---

**3.2** Print the message: **I am learning Linux!**

Write the command here: `________________`

**Expected Output:**
```
I am learning Linux!
```

---

**3.3** Print today's date manually using echo (type the date yourself, don't use the `date` command).

Example: `echo "Today is December 2, 2024"`

Write YOUR command here: `________________`

---

**3.4** Print a message with multiple lines. Print:
```
Line 1: Hello
Line 2: World
```

<details>
<summary>💡 Hint</summary>

You can use `echo -e "Line 1\nLine 2"` where `\n` creates a new line. The `-e` option enables interpretation of backslash escapes.

</details>

Write the command here: `________________`

---

**3.5** **Bonus Challenge:** Print a message without the automatic new line at the end.

<details>
<summary>💡 Hint</summary>

Use `echo -n "Your message"` - the `-n` option prevents the trailing newline.

</details>

---

### ✅ Exercise 3 Checklist:
- [ ] I can print simple text with echo
- [ ] I understand that text goes inside quotes
- [ ] I tried the bonus challenges

---

## Exercise 4: Getting Help 📚

🎯 **Goal:** Learn to find help when you don't know how to use a command.

### Tasks:

**4.1** Display the help information for the `cal` command.

Write the command here: `________________`

**Question:** What option shows three months (previous, current, next)?
Answer: `________________`

---

**4.2** Open the manual page for the `echo` command.

Write the command here: `________________`

---

**4.3** While in the manual page, practice navigation:

| Task | Key to Press |
|------|--------------|
| Scroll down one line | |
| Scroll up one line | |
| Go to next page | |
| Search for "newline" | |
| Exit the manual | |

<details>
<summary>💡 Hint</summary>

- Scroll: Arrow keys or `j`/`k`
- Next page: `Space` or `f`
- Search: `/` followed by your search term
- Exit: `q`

</details>

---

**4.4** Use `whatis` to get a one-line description of these commands:

```bash
$ whatis date
$ whatis whoami
$ whatis hostname
```

Write down what each command does:
- `date`: ________________________________
- `whoami`: ________________________________
- `hostname`: ________________________________

---

**4.5** Use `--help` on the `date` command and find:

**Question:** What option shows only the year?

<details>
<summary>💡 Hint</summary>

Run `date --help` and look for options related to formatting. Try `date +%Y` for just the year.

</details>

---

### ✅ Exercise 4 Checklist:
- [ ] I can use `--help` to get quick help
- [ ] I can open and navigate manual pages with `man`
- [ ] I know how to exit the manual (`q`)
- [ ] I can use `whatis` for quick descriptions

---

## Exercise 5: Challenge - System Information Report 🏆

🎯 **Goal:** Combine everything you learned to create a mini system report.

### The Challenge:

Run commands to display the following information about your system, and write down the results:

| Information | Command | Your Result |
|-------------|---------|-------------|
| Username | | |
| Hostname | | |
| Current Date & Time | | |
| Current Month Calendar | | |

---

### Bonus: Create a "Report"

Run these commands one after another to create a formatted report:

```bash
echo "====== SYSTEM REPORT ======"
echo ""
echo "User Information:"
whoami
echo ""
echo "Computer Name:"
hostname
echo ""
echo "Current Date and Time:"
date
echo ""
echo "====== END OF REPORT ======"
```

Copy and paste this into your terminal line by line (or all at once) and observe the output!

---

### Reflection Questions:

1. Which command did you find most useful? Why?
   
   _Answer:_ ________________________________

2. What was the most challenging part of these exercises?
   
   _Answer:_ ________________________________

3. What keyboard shortcut will you remember for opening the terminal?
   
   _Answer:_ ________________________________

---

## ✅ Exercise 5 Checklist:
- [ ] I completed the system information table
- [ ] I ran the bonus report commands
- [ ] I answered the reflection questions

---

# 🎉 Congratulations!

You've completed all the exercises for Module 1!

---

## 📝 Solutions

<details>
<summary>Click to reveal solutions (try the exercises first!)</summary>

### Exercise 1 Solutions:

**1.1** Keyboard shortcut: `Ctrl + Alt + T`

**1.2** `whoami`

**1.3** `hostname`

**1.4** `date`

**1.5** `clear` (or `Ctrl + L`)

---

### Exercise 2 Solutions:

**2.1** `cal`

**2.2** `cal 12 2024`

**2.3** `cal 2025` or `cal -y` (for current year)

**2.4** `cal -m` or `cal --monday`

---

### Exercise 3 Solutions:

**3.1** `echo "Your Name"` (replace with your actual name)

**3.2** `echo "I am learning Linux!"`

**3.3** `echo "Today is December 2, 2024"` (use today's actual date)

**3.4** `echo -e "Line 1: Hello\nLine 2: World"`

**3.5** `echo -n "Your message"`

---

### Exercise 4 Solutions:

**4.1** `cal --help`
- Three months option: `-3` or `--three`

**4.2** `man echo`

**4.3** Navigation keys:
| Task | Key |
|------|-----|
| Scroll down | `↓` or `j` |
| Scroll up | `↑` or `k` |
| Next page | `Space` or `f` |
| Search | `/newline` then Enter |
| Exit | `q` |

**4.4** 
- `date`: display or set the system date and time
- `whoami`: print effective userid
- `hostname`: show or set the system's host name

**4.5** `date +%Y` shows only the year

---

### Exercise 5 Solution:

| Information | Command | Example Result |
|-------------|---------|----------------|
| Username | `whoami` | kali |
| Hostname | `hostname` | kali |
| Current Date & Time | `date` | Mon Dec 2 10:30:00 CET 2024 |
| Current Month Calendar | `cal` | (calendar output) |

</details>

---

## 🚀 What's Next?

Great job completing Module 1! In Module 2, you'll learn:
- How the Linux file system is organized
- How to navigate directories with `cd`
- How to list files with `ls`
- How to find where you are with `pwd`

Keep practicing these commands until they become second nature! 💪