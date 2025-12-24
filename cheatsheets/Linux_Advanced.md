# My Linux Cheatsheet

## Arguments & Help
Most commands change their behavior using **flags** (switches).
- Short form: `-a`
- Long form: `--all`

| Command | Description | Example |
| :--- | :--- | :--- |
| `ls -a` | Shows **all** files (including hidden ones). | `ls -a` |
| `ls --help` | Shows me the help menu and options. | `ls --help` |
| `man` | Opens the manual for a command. | `man ls` |
| `file` | Shows me the actual file type (header analysis). | `file unknown_file` |

> **Navigation in `man`:**
> `↓` Scroll • `h` Help • `q` Quit

---

## Creating & Editing Files/Folders

| Command | Full Name | Function | Example |
| :--- | :--- | :--- | :--- |
| `touch` | touch | Creates an empty file for me. | `touch note.txt` |
| `mkdir` | make directory | Creates a folder. | `mkdir vacation_pics` |
| `cp` | copy | Copies a file or folder. | `cp note.txt backup.txt` |
| `mv` | move | Moves **or** renames a file. | `mv old.txt new.txt` |
| `rm` | remove | Deletes a file. | `rm trash.txt` |
| `rm -R` | remove recursive | Deletes a folder + contents. | `rm -R my_folder` |

---

## Users & Permissions


Linux distinguishes between **Owner**, **Group**, and **Others**.

**Viewing Permissions:**
`ls -l` → `-rw-r--r-- 1 user group ...`

| Char | Meaning | Explanation |
| :--- | :--- | :--- |
| `r` | Read | Read file / View folder content |
| `w` | Write | Edit file / Create or delete files in folder |
| `x` | Execute | Run file as script / Enter directory |

**Switching Users:**

| Command | Description |
| :--- | :--- |
| `su [user]` | Switches to the user. |
| `su -l [user]` | Switches user + loads their full environment. |

---

## Important System Directories


| Path | Purpose / Content |
| :--- | :--- |
| `/etc` | Configurations, `passwd`, `shadow`. |
| `/var` | Variable data: Logs, databases, backups. |
| `/root` | Home directory of the root user. |
| `/tmp` | Temporary files, writable by everyone. |

---

# Permissions & Owners (Extended)

## `chmod` – Changing Permissions

| Command | Example | Explanation |
| :--- | :--- | :--- |
| `chmod +x script.sh` | Makes a file executable. | Necessary for my scripts, tools, and exploits. |
| `chmod 777 file` | Gives everyone every permission. | Unsafe, but often useful in CTFs. |
| `chmod 600 id_rsa` | Only owner can read/write. | Mandatory for SSH Private Keys. |

---

## `chown` – Changing Owners

| Command | Example | Explanation |
| :--- | :--- | :--- |
| `chown user:user file` | `chown serkan:serkan config.txt` | Changes owner and group. |
| `chown -R user:group folder/` | `chown -R www-data:www-data /var/www` | Recursive (for folders). |

---

# Network & Remote

## `ssh` – Remote Login

| Command | Example | What it does |
| :--- | :--- | :--- |
| `ssh user@IP` | `ssh serkan@10.10.10.10` | Establishes an SSH connection. |
| `ssh -i key.pem user@IP` | `ssh -i id_rsa root@192.168.1.10` | Login using a Private Key. |

---

## `yes` – Auto-Confirming

| Command | Example | Explanation |
| :--- | :--- | :--- |
| `yes` | `yes | apt install package` | Outputs "y" infinitely. |
| `yes` (with SSH) | Used to auto-accept fingerprints. | Great for my scripts. |

---

## Text Editors (Nano & VIM)
**Nano** is beginner-friendly. **VIM** is powerful but complex.

**Start Nano:** `nano filename`

| Shortcut (Nano) | Function |
| :--- | :--- |
| `Ctrl + X` | Exit (asks to save). |
| `Ctrl + O` | Save (Write Out). |
| `Ctrl + W` | Search (Where Is). |
| `Ctrl + K` | Cut entire line. |
| `Ctrl + U` | Paste text. |

---

## Transferring Files

### Download from Web
| Command | Description | Example |
| :--- | :--- | :--- |
| `wget URL` | Downloads files via HTTP/HTTPS. | `wget http://site.com/file.txt` |
| `curl -O URL` | Downloads and saves with original name. | `curl -O http://site.com/file` |

> **Note:**
> `curl` without `-O` only prints content to my terminal – it does NOT save it.

---

### My Webserver (Python)
Starts a temporary webserver in my current directory.

| Command | Port | Download from another PC |
| :--- | :--- | :--- |
| `python3 -m http.server` | 8000 (Default) | `wget http://[IP]:8000/file` |

---

### SCP (Secure Copy via SSH)
Copies files securely. Syntax is always `cp SOURCE DESTINATION`.

| Direction | Syntax | Example |
| :--- | :--- | :--- |
| **Upload** (Local → Remote) | `scp [File] [User]@[IP]:[Path]` | `scp file.txt root@10.10.10.10:/root/` |
| **Download** (Remote → Local) | `scp [User]@[IP]:[Path] [Target]` | `scp root@10.10.10.10:/root/file.txt .` |

---

## Process Management


Every program is a process with a **PID** (Process ID).

### Viewing & Killing
| Command | Description |
| :--- | :--- |
| `ps` | Shows processes in my current session. |
| `ps aux` | Shows **all** processes from all users. |
| `top` | Real-time process monitor. |
| `kill [PID]` | Stops a process (SIGTERM). |
| `kill -9 [PID]` | Forces a process to stop (SIGKILL). |

---

### Managing Services (systemctl)
`systemctl [Option] [ServiceName]`

* `start` / `stop` – Start/Stop service
* `enable` / `disable` – Turn autostart on/off
* `status` – Check if service is running

---

### Background & Foreground
| Command | Description |
| :--- | :--- |
| `&` | Starts command in the background. |
| `Ctrl + Z` | Pauses process → moves to background. |
| `fg` | Brings process back to foreground. |

---

## Automation (Cronjobs)


**Edit Cron:** `crontab -e`

**Syntax:**
`MIN HOUR DAY MONTH WEEKDAY COMMAND`
`*` = Wildcard

| Example | Meaning |
| :--- | :--- |
| `0 * * * *` | Every hour. |
| `0 12 * * *` | Every day at 12:00 PM. |
| `*/5 * * * *` | Every 5 minutes. |
| `@reboot` | On system startup. |

---

## Package Management (APT) & Logs

### APT
| Command | Description |
| :--- | :--- |
| `apt update` | Updates my package lists. |
| `apt install [Name]` | Installs a package. |
| `apt remove [Name]` | Removes a package. |
| `add-apt-repository` | Adds a new repository. |

---

### Important Log Files (`/var/log/`)
* `/var/log/apache2/access.log` – Webserver hits
* `/var/log/apache2/error.log` – Webserver errors
* `/var/log/syslog` – System messages
* `/var/log/auth.log` – Login attempts (SSH, su, sudo)

---

## History & Trace Analysis

| Command | Description | Why is this important? |
| :--- | :--- | :--- |
| `history` | Shows the last ~1000 commands. | Passwords are often visible here if admins typed them directly (e.g., `mysql -u root -pSecret123`). |
| `cat ~/.bash_history` | Shows the saved history file. | If I compromise a user, I can read their history to see what they did. |
