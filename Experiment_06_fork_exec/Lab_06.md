---
# OPERATING SYSTEM LAB
### Experiment No.: 06
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 06                    |

---

## TITLE: Process Creation using fork() and exec() System Calls

---

## AIM

To understand and implement process creation using `fork()`, `exec()`, `wait()`, and `getpid()` system calls in Linux/Unix, and observe parent-child process behavior.

---

## THEORY

### Process:
A **process** is a program in execution. Each process has a unique **Process ID (PID)** and runs in its own memory space.

### System Calls for Process Management:

| System Call    | Description                                                         |
|----------------|---------------------------------------------------------------------|
| `fork()`       | Creates a copy of the current process (child process)              |
| `exec()`       | Replaces the current process image with a new program              |
| `wait()`       | Makes parent wait until child process finishes                     |
| `exit()`       | Terminates a process                                               |
| `getpid()`     | Returns the PID of the calling process                             |
| `getppid()`    | Returns the PID of the parent process                              |

### fork() Return Values:
| Return Value | Meaning                         |
|--------------|---------------------------------|
| `< 0`        | Fork failed (error)             |
| `= 0`        | In child process                |
| `> 0`        | In parent process (value = child PID) |

### Process Tree after fork():
```
         Parent Process (PID = 100)
                   |
              fork() called
             /           \
   Parent (PID=100)   Child (PID=101)
   Returns 101        Returns 0
```

### exec() Family:
The `exec()` family replaces the current process's memory space with a new program. The original process is gone after `exec()` succeeds.
- `execl()`, `execv()`, `execlp()`, `execvp()` are common variants.

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux (Required — fork/exec are POSIX/Unix system calls)
- **Compiler:** GCC
- **Header Files:** `<unistd.h>`, `<sys/types.h>`, `<sys/wait.h>`, `<stdio.h>`
- **Editor:** VS Code / nano / vim

---

## PROCEDURE / PROGRAMS

---

### Program 1: Basic fork() — Parent and Child Process

```c
// File: fork_basic.c
// Author: Pranay K Gajbhiye
// Basic fork() demonstration

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid;

    printf("Before fork:\n");
    printf("  Current PID  = %d\n", getpid());

    pid = fork();

    if (pid < 0) {
        printf("Fork failed!\n");
        return 1;
    }
    else if (pid == 0) {
        // Child process
        printf("\n[CHILD PROCESS]\n");
        printf("  Child  PID  = %d\n", getpid());
        printf("  Parent PID  = %d\n", getppid());
        printf("  Child is running...\n");
    }
    else {
        // Parent process
        printf("\n[PARENT PROCESS]\n");
        printf("  Parent PID  = %d\n", getpid());
        printf("  Child  PID  = %d (returned by fork)\n", pid);
        printf("  Parent is running...\n");
    }

    printf("Process %d finished.\n", getpid());
    return 0;
}
```

---

### Program 2: fork() with wait() — Parent Waits for Child

```c
// File: fork_wait.c
// Author: Pranay K Gajbhiye
// fork() with wait() - parent waits for child to complete

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid;
    int status;

    pid = fork();

    if (pid < 0) {
        printf("Fork failed!\n");
        return 1;
    }
    else if (pid == 0) {
        // Child Process
        printf("[CHILD]  PID=%d started.\n", getpid());
        sleep(2);   // Simulate work
        printf("[CHILD]  PID=%d completed work.\n", getpid());
    }
    else {
        // Parent Process
        printf("[PARENT] PID=%d waiting for child (PID=%d)...\n",
               getpid(), pid);
        wait(&status);  // Wait for child to finish
        printf("[PARENT] Child has finished. Parent continues.\n");
        printf("[PARENT] PID=%d — done.\n", getpid());
    }

    return 0;
}
```

---

### Program 3: exec() — Replace Process Image

```c
// File: fork_exec.c
// Author: Pranay K Gajbhiye
// fork() + execl() demonstration

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid;

    pid = fork();

    if (pid < 0) {
        printf("Fork failed!\n");
        return 1;
    }
    else if (pid == 0) {
        // Child: replace with /bin/ls command
        printf("[CHILD] PID=%d — About to exec 'ls -l'\n", getpid());
        execl("/bin/ls", "ls", "-l", NULL);

        // This line only runs if execl() fails
        printf("[CHILD] exec failed!\n");
    }
    else {
        // Parent
        printf("[PARENT] PID=%d — Child PID=%d is executing 'ls -l'\n",
               getpid(), pid);
        wait(NULL);
        printf("[PARENT] Child finished. Parent done.\n");
    }

    return 0;
}
```

---

### Program 4: Multiple Children using Loops

```c
// File: multi_fork.c
// Author: Pranay K Gajbhiye
// Creating multiple child processes with fork()

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    int n = 3;   // Number of child processes
    pid_t pid;
    int i;

    printf("[PARENT] PID=%d will create %d children.\n", getpid(), n);

    for (i = 0; i < n; i++) {
        pid = fork();

        if (pid == 0) {
            // Child process
            printf("[CHILD %d] PID=%d, Parent PID=%d — working...\n",
                   i+1, getpid(), getppid());
            sleep(1);
            printf("[CHILD %d] PID=%d — done.\n", i+1, getpid());
            return 0;   // Exit child
        }
    }

    // Parent waits for all children
    for (i = 0; i < n; i++) {
        wait(NULL);
    }

    printf("[PARENT] All children finished. Parent exiting.\n");
    return 0;
}
```

**Compile & Run:**
```bash
gcc fork_basic.c -o fork_basic    && ./fork_basic
gcc fork_wait.c  -o fork_wait     && ./fork_wait
gcc fork_exec.c  -o fork_exec     && ./fork_exec
gcc multi_fork.c -o multi_fork    && ./multi_fork
```

---

## OUTPUT / OBSERVATIONS

**Program 1 — Basic fork():**
```
Before fork:
  Current PID  = 1500

[PARENT PROCESS]
  Parent PID  = 1500
  Child  PID  = 1501 (returned by fork)
  Parent is running...
Process 1500 finished.

[CHILD PROCESS]
  Child  PID  = 1501
  Parent PID  = 1500
  Child is running...
Process 1501 finished.
```

**Program 2 — fork() with wait():**
```
[PARENT] PID=1502 waiting for child (PID=1503)...
[CHILD]  PID=1503 started.
[CHILD]  PID=1503 completed work.
[PARENT] Child has finished. Parent continues.
[PARENT] PID=1502 — done.
```

**Program 3 — exec():**
```
[PARENT] PID=1504 — Child PID=1505 is executing 'ls -l'
[CHILD]  PID=1505 — About to exec 'ls -l'
total 48
-rwxr-xr-x 1 pranay pranay 8752 Mar 11 10:00 fork_exec
-rw-r--r-- 1 pranay pranay  620 Mar 11 10:00 fork_exec.c
...
[PARENT] Child finished. Parent done.
```

---

## RESULT

Process creation using `fork()`, `exec()`, `wait()`, and `getpid()` system calls was successfully demonstrated in C on Linux. The programs verified that:
1. `fork()` creates an exact copy of the parent process as a child.
2. `wait()` makes the parent pause until the child finishes.
3. `exec()` replaces the child's memory space with a new program.
4. Multiple children can be created in a loop, each running concurrently.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 06*
