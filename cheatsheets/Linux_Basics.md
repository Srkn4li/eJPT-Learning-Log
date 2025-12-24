# My Linux Cheatsheet

## Basics & Users
| Command | Description | Example |
| :--- | :--- | :--- |
| `echo` | Prints text to my terminal. I need quotes for sentences. | `echo "Hello World"` |
| `whoami` | Tells me which user I am currently logged in as. | `whoami` |

---

## Filesystem Navigation

| Command | Full Name | Description | Example |
| :--- | :--- | :--- | :--- |
| `pwd` | Print Working Directory | Shows me exactly where I am (absolute path). | `pwd` |
| `ls` | List | Lists files and folders for me. | `ls` |
| `cd` | Change Directory | Moves me into a different folder. | `cd Documents` |
| `ls -la` | List All | Lists **everything** (including hidden `.` files) + details (permissions, size, owner). | `ls -la` |

**Why is this important?**
It reveals `.secret` files and helps me spot permission errors. Crucial for Privilege Escalation & Forensics.

---

## Reading & Finding Files
| Command | Description | Example |
| :--- | :--- | :--- |
| `cat` | Dumps the entire content of a file for me to read. | `cat todo.txt` |
| `find -name <file>` | Searches for a file in my current folder and subfolders. | `find -name password.txt` |
| `find . -name flag.txt` | Explicitly searches in my current directory (`.`). | `find . -name flag.txt` |
| `find / -name flag.txt` | Searches the **entire** system (slow, produces many errors). | `find / -name flag.txt` |
| `find / -name flag.txt 2>/dev/null` | Searches everywhere but hides "Permission denied" errors. | `find / -name flag.txt 2>/dev/null` |
| `grep "<text>" <file>` | Searches for a specific string inside a file. | `grep "192.168.1.1" access.log` |
| `grep -r "<text>" <folder>` | Searches recursively through all files in a folder for the text. | `grep -r "password" /var/www/html` |

**Why is this important?**
`grep -r` is essential for CTFs, log analysis, and hunting for credentials in code.

---

## Operators & Redirections

| Operator | Function | Example |
| :--- | :--- | :--- |
| `&` | Runs the command in the background (so I can keep working). | `copy largefile.iso &` |
| `&&` | Only runs the second command if the first one was successful. | `cd Documents && ls` |
| `>` | Redirects output to a file (**overwrites** the file!). | `echo "New" > list.txt` |
| `>>` | Redirects output to a file (**appends** to the end). | `echo "More" >> list.txt` |
