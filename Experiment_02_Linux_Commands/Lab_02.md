# Experiment 02 — Basic Linux Commands

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To execute and understand basic Linux commands for file management, directory navigation, process control, and system information.

---

## Theory

Linux is an open-source, Unix-like operating system kernel. It uses a **Command Line Interface (CLI)** called the **shell** (commonly Bash — Bourne Again Shell) for user interaction.

### Categories of Basic Linux Commands

| Category              | Purpose                                        |
|-----------------------|------------------------------------------------|
| File & Directory      | Create, list, move, copy, delete files/folders |
| Process Management    | View and manage running processes              |
| System Information    | Display system and hardware details            |
| Text Processing       | Read and manipulate file contents              |
| Permissions           | Manage file access permissions                 |

### File System Hierarchy (FHS)

```
/                    Root directory
├── bin/             Essential command binaries
├── etc/             Configuration files
├── home/            User home directories
├── tmp/             Temporary files
├── usr/             User programs & utilities
└── var/             Variable data (logs, etc.)
```

---

## Apparatus / Requirements

| Component       | Specification                         |
|-----------------|---------------------------------------|
| Computer System | Any PC with minimum 2 GB RAM         |
| Operating System| Linux (Ubuntu 20.04 or later)         |
| Software        | Terminal (Bash shell)                 |

---

## Procedure

1. Open the Linux Terminal.
2. Execute each command listed below.
3. Observe and note the output.
4. Understand the purpose and syntax of each command.

---

## Commands with Explanation and Output

### 1. `pwd` — Print Working Directory

Displays the absolute path of the current directory.

```bash
$ pwd
```

**Output:**
```
/home/pranay
```

---

### 2. `ls` — List Directory Contents

Lists files and directories in the current directory.

```bash
$ ls
$ ls -l        # long listing format
$ ls -la       # include hidden files
```

**Output:**
```
Desktop  Documents  Downloads  Music  Pictures  Videos

total 48
drwxr-xr-x  2 pranay pranay 4096 Mar 10 10:00 Desktop
drwxr-xr-x  5 pranay pranay 4096 Mar 10 10:00 Documents
drwxr-xr-x  2 pranay pranay 4096 Mar 10 10:00 Downloads
```

---

### 3. `cd` — Change Directory

Navigates between directories.

```bash
$ cd Documents
$ cd ..          # go one level up
$ cd ~           # go to home directory
$ cd /           # go to root directory
```

**Output:**
```
(directory changes; use pwd to confirm)
```

---

### 4. `mkdir` — Make Directory

Creates a new directory.

```bash
$ mkdir OS_Lab
$ mkdir -p OS_Lab/Experiment01    # create nested directories
```

**Output:**
```
(new directory OS_Lab created)
```

---

### 5. `rmdir` — Remove Empty Directory

Removes an empty directory.

```bash
$ rmdir OS_Lab
```

**Output:**
```
(directory removed if empty)
```

---

### 6. `touch` — Create Empty File

Creates an empty file or updates the timestamp of an existing file.

```bash
$ touch hello.txt
```

**Output:**
```
(file hello.txt created)
```

---

### 7. `cp` — Copy Files or Directories

Copies files or directories from source to destination.

```bash
$ cp hello.txt hello_backup.txt
$ cp -r OS_Lab/ OS_Lab_Backup/    # recursive copy
```

**Output:**
```
(file copied successfully)
```

---

### 8. `mv` — Move or Rename Files

Moves a file to another location or renames it.

```bash
$ mv hello.txt greet.txt          # rename
$ mv greet.txt Documents/         # move to Documents
```

**Output:**
```
(file renamed/moved successfully)
```

---

### 9. `rm` — Remove Files or Directories

Deletes files or directories.

```bash
$ rm hello_backup.txt
$ rm -r OS_Lab_Backup/            # recursive delete
$ rm -rf dirname/                 # force delete (use with caution)
```

**Output:**
```
(file/directory deleted)
```

---

### 10. `cat` — Concatenate and Display File Content

Displays the content of a file.

```bash
$ cat /etc/os-release
```

**Output:**
```
NAME="Ubuntu"
VERSION="20.04.6 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.6 LTS"
```

---

### 11. `echo` — Print Text to Terminal

Prints a string or variable value to the terminal.

```bash
$ echo "Hello, World!"
$ echo $HOME
```

**Output:**
```
Hello, World!
/home/pranay
```

---

### 12. `whoami` — Current User Name

Displays the username of the currently logged-in user.

```bash
$ whoami
```

**Output:**
```
pranay
```

---

### 13. `date` — Display System Date and Time

Shows the current system date and time.

```bash
$ date
```

**Output:**
```
Sun Mar 15 10:30:00 IST 2026
```

---

### 14. `cal` — Display Calendar

Displays a calendar of the current or specified month/year.

```bash
$ cal
$ cal 2026
```

**Output:**
```
     March 2026
Su Mo Tu We Th Fr Sa
 1  2  3  4  5  6  7
 8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30 31
```

---

### 15. `man` — Manual Pages

Displays the manual/documentation for a command.

```bash
$ man ls
$ man cat
```

**Output:**
```
LS(1)                     User Commands                    LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...
...
```

---

### 16. `chmod` — Change File Permissions

Modifies the access permissions of a file.

```bash
$ chmod 755 hello.txt     # rwxr-xr-x
$ chmod +x script.sh      # add execute permission
```

**Output:**
```
(permissions updated; verify with ls -l)
```

---

### 17. `ps` — Process Status

Displays currently running processes.

```bash
$ ps
$ ps aux      # all processes with details
```

**Output:**
```
  PID TTY          TIME CMD
 1234 pts/0    00:00:00 bash
 5678 pts/0    00:00:00 ps
```

---

### 18. `top` — Real-Time Process Monitor

Shows a dynamic, real-time view of running processes and resource usage.

```bash
$ top
```

**Output:**
```
top - 10:31:00 up 2:10,  1 user,  load average: 0.10, 0.12, 0.08
Tasks: 180 total,   1 running, 179 sleeping,   0 stopped
%Cpu(s):  2.3 us,  0.7 sy,  0.0 ni, 96.5 id
MiB Mem :   7892.0 total,   3200.0 free
...
```

---

### 19. `clear` — Clear Terminal Screen

Clears all previous output from the terminal.

```bash
$ clear
```

---

### 20. `history` — Command History

Displays the list of previously entered commands.

```bash
$ history
$ history | tail -10    # last 10 commands
```

**Output:**
```
  1  pwd
  2  ls
  3  mkdir OS_Lab
  4  cd OS_Lab
  5  touch hello.txt
  ...
```

---

## Summary Table

| Command  | Purpose                             | Common Options         |
|----------|-------------------------------------|------------------------|
| `pwd`    | Print current directory             | —                      |
| `ls`     | List files                          | `-l`, `-a`, `-la`      |
| `cd`     | Change directory                    | `..`, `~`, `/`         |
| `mkdir`  | Create directory                    | `-p`                   |
| `rmdir`  | Remove empty directory              | —                      |
| `touch`  | Create empty file                   | —                      |
| `cp`     | Copy file/directory                 | `-r`                   |
| `mv`     | Move or rename                      | —                      |
| `rm`     | Delete file/directory               | `-r`, `-rf`            |
| `cat`    | Display file content                | —                      |
| `echo`   | Print text                          | —                      |
| `whoami` | Current username                    | —                      |
| `date`   | Current date and time               | —                      |
| `cal`    | Calendar                            | month, year            |
| `man`    | Manual pages                        | command name           |
| `chmod`  | Change permissions                  | `755`, `+x`            |
| `ps`     | Process status                      | `aux`                  |
| `top`    | Real-time process monitor           | —                      |
| `clear`  | Clear terminal                      | —                      |
| `history`| Command history                     | —                      |

---

## Result

Basic Linux commands for file management, directory navigation, process control, and system information were executed and understood successfully.
