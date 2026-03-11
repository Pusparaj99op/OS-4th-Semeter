---
# OPERATING SYSTEM LAB
### Experiment No.: 01
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 01                    |

---

## TITLE: Study and Use of Basic Linux / Unix Commands

---

## AIM

To study and practice the basic Linux/Unix commands for file management, process management, directory operations, and user-related operations.

---

## THEORY

Linux is a free, open-source operating system based on the Unix kernel, originally developed by **Linus Torvalds** in 1991. It is widely used in servers, embedded systems, and development environments.

### Key Concepts:

**Shell:** A command-line interface that accepts user commands and executes them. Common shells include `bash`, `sh`, `zsh`.

**File System Hierarchy:**
- `/` — Root directory
- `/home` — User home directories
- `/etc` — Configuration files
- `/bin` — Essential binary executables
- `/tmp` — Temporary files
- `/var` — Variable data (logs, spool)

### Categories of Linux Commands:

| Category               | Description                                  |
|------------------------|----------------------------------------------|
| File Commands          | Create, copy, move, delete files             |
| Directory Commands     | Navigate and manage directories              |
| Process Commands       | View and manage running processes            |
| User Commands          | Manage users and permissions                 |
| I/O Redirection        | Redirect input/output of commands            |

### Important Commands:

| Command        | Description                              | Example                      |
|----------------|------------------------------------------|------------------------------|
| `pwd`          | Print working directory                  | `pwd`                        |
| `ls`           | List files and directories               | `ls -la`                     |
| `cd`           | Change directory                         | `cd /home/pranay`            |
| `mkdir`        | Make a new directory                     | `mkdir Lab_Files`            |
| `rmdir`        | Remove empty directory                   | `rmdir Lab_Files`            |
| `touch`        | Create empty file / update timestamp     | `touch file.txt`             |
| `cat`          | Display file contents                    | `cat file.txt`               |
| `cp`           | Copy files/directories                   | `cp a.txt b.txt`             |
| `mv`           | Move or rename files                     | `mv old.txt new.txt`         |
| `rm`           | Remove files                             | `rm file.txt`                |
| `chmod`        | Change file permissions                  | `chmod 755 file.txt`         |
| `chown`        | Change file ownership                    | `chown user:group file.txt`  |
| `ps`           | Show running processes                   | `ps aux`                     |
| `kill`         | Terminate a process                      | `kill 1234`                  |
| `grep`         | Search text in files                     | `grep "hello" file.txt`      |
| `man`          | Display manual of a command              | `man ls`                     |
| `echo`         | Display a message                        | `echo "Hello, World!"`       |
| `date`         | Show current date and time               | `date`                       |
| `whoami`       | Show current logged-in user              | `whoami`                     |
| `clear`        | Clear the terminal screen                | `clear`                      |

### File Permission Structure:
```
-rwxr-xr-x  1  pranay  group  4096  Mar 11 10:00  file.txt
|__|__|__|
 |  |  |
 |  |  └── Others: r-x (5)
 |  └───── Group:  r-x (5)
 └──────── Owner:  rwx (7)
```
- `r` = read (4), `w` = write (2), `x` = execute (1)

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux / Any Linux Distribution
- **Software:** Terminal (Bash Shell)
- **RAM:** Minimum 2 GB
- **Storage:** At least 10 GB free disk space

---

## PROCEDURE

### Step-by-Step Execution:

**Step 1:** Open the Terminal (Ctrl + Alt + T in Ubuntu).

**Step 2:** Check the current working directory.
```bash
pwd
```

**Step 3:** List files in the current directory.
```bash
ls
ls -la
```

**Step 4:** Create a new directory.
```bash
mkdir OS_Lab
cd OS_Lab
```

**Step 5:** Create files using `touch` and `cat`.
```bash
touch file1.txt
cat > file2.txt
This is file2 content.
(Press Ctrl+D to save)
```

**Step 6:** Display file contents.
```bash
cat file1.txt
cat file2.txt
```

**Step 7:** Copy and move files.
```bash
cp file1.txt file1_copy.txt
mv file2.txt renamed_file.txt
```

**Step 8:** Check file permissions.
```bash
ls -l
```

**Step 9:** Change file permissions.
```bash
chmod 755 file1.txt
ls -l file1.txt
```

**Step 10:** View running processes.
```bash
ps
ps aux
```

**Step 11:** Use grep to search.
```bash
echo "Hello Pranay" > greet.txt
grep "Pranay" greet.txt
```

**Step 12:** Remove files and directory.
```bash
rm file1_copy.txt
cd ..
rmdir OS_Lab
```

---

## OUTPUT / OBSERVATIONS

```
$ pwd
/home/pranay

$ ls -la
total 32
drwxr-xr-x  5 pranay pranay 4096 Mar 11 10:00 .
drwxr-xr-x 20 root   root   4096 Mar 11 09:00 ..
-rw-r--r--  1 pranay pranay  220 Mar 11 09:00 .bash_logout

$ mkdir OS_Lab
$ cd OS_Lab
$ touch file1.txt
$ ls
file1.txt

$ ls -l file1.txt
-rw-r--r-- 1 pranay pranay 0 Mar 11 10:05 file1.txt

$ chmod 755 file1.txt
$ ls -l file1.txt
-rwxr-xr-x 1 pranay pranay 0 Mar 11 10:05 file1.txt

$ whoami
pranay

$ date
Tue Mar 11 10:06:00 IST 2026

$ ps
  PID TTY          TIME CMD
 1234 pts/0    00:00:00 bash
 1256 pts/0    00:00:00 ps
```

---

## RESULT

The basic Linux/Unix commands were successfully studied and executed. The commands for file creation, directory management, permission setting, process viewing, and text searching were all carried out as expected. The output matches the standard Linux command behavior, confirming successful completion of the experiment.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 01*
