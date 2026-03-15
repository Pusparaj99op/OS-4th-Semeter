# Experiment 03 — FORK System Call

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To understand and execute the `fork()` system call in Linux to create child processes and observe parent-child process behavior.

---

## Theory

### Process

A **process** is a program in execution. Every process has a unique **Process ID (PID)**. The OS maintains a **Process Control Block (PCB)** for each process containing its state, PID, program counter, registers, and memory information.

### System Call

A **system call** is a programmatic mechanism through which a user-level program requests services from the operating system kernel. System calls provide the interface between user space and kernel space.

### The `fork()` System Call

`fork()` is a POSIX system call used to create a **new process** (child process) that is a near-exact copy of the calling process (parent process).

**Header file:** `<unistd.h>`

**Syntax:**
```c
pid_t fork(void);
```

**Return Values:**

| Returned To   | Value         | Meaning                          |
|---------------|---------------|----------------------------------|
| Parent process | Child's PID  | Positive integer (child's PID)   |
| Child process  | 0            | Zero — indicates it is the child |
| Error          | -1           | Fork failed; no child created    |

### How `fork()` Works

```
fork() called
        |
        v
Parent Process (PID = P)
        |
   +---------+
   |         |
   v         v
Parent     Child (new PID = C, PPID = P)
(gets C)   (gets 0)
```

After `fork()`:
- Both parent and child execute the **same code** from the point after `fork()`.
- They have **separate memory spaces** (copy-on-write in Linux).
- The child inherits the parent's file descriptors, variables, etc.

### Related Functions

| Function    | Purpose                                    |
|-------------|---------------------------------------------|
| `getpid()`  | Returns the PID of the calling process      |
| `getppid()` | Returns the PID of the parent process       |
| `wait()`    | Parent waits for child to finish            |
| `exit()`    | Terminates the calling process              |
| `exec()`    | Replaces process image with a new program   |

---

## Apparatus / Requirements

| Component       | Specification                        |
|-----------------|--------------------------------------|
| Computer System | Any PC with minimum 2 GB RAM        |
| Operating System| Linux (Ubuntu 20.04 or later)        |
| Compiler        | GCC (GNU C Compiler)                 |
| Editor          | Vi / Vim / Nano / any text editor    |

---

## Procedure

1. Open the terminal.
2. Create a new C source file using Vi editor: `vi fork_demo.c`
3. Write the C program using `fork()` system call.
4. Save and exit the editor.
5. Compile the program: `gcc fork_demo.c -o fork_demo`
6. Run the program: `./fork_demo`
7. Observe the output and note the parent and child PIDs.

---

## Program

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid;

    printf("Before fork: PID = %d\n", getpid());

    pid = fork();

    if (pid < 0) {
        /* Fork failed */
        fprintf(stderr, "Fork failed!\n");
        exit(1);
    } else if (pid == 0) {
        /* Child process */
        printf("\n[CHILD]  PID  = %d\n", getpid());
        printf("[CHILD]  PPID = %d (Parent's PID)\n", getppid());
        printf("[CHILD]  Executing child task...\n");
        exit(0);
    } else {
        /* Parent process */
        printf("\n[PARENT] PID  = %d\n", getpid());
        printf("[PARENT] Child PID created = %d\n", pid);
        printf("[PARENT] Waiting for child to finish...\n");
        wait(NULL);
        printf("[PARENT] Child has finished. Parent exiting.\n");
    }

    return 0;
}
```

---

## Compilation and Execution

```bash
$ gcc fork_demo.c -o fork_demo
$ ./fork_demo
```

---

## Output

```
Before fork: PID = 4521

[PARENT] PID  = 4521
[PARENT] Child PID created = 4522
[PARENT] Waiting for child to finish...

[CHILD]  PID  = 4522
[CHILD]  PPID = 4521 (Parent's PID)
[CHILD]  Executing child task...
[PARENT] Child has finished. Parent exiting.
```

> **Note:** Actual PID values will vary on each execution.

---

## Program 2 — Multiple fork() calls

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid1, pid2;

    pid1 = fork();   /* First fork */

    if (pid1 == 0) {
        printf("[Child 1] PID=%d, PPID=%d\n", getpid(), getppid());
        exit(0);
    }

    pid2 = fork();   /* Second fork */

    if (pid2 == 0) {
        printf("[Child 2] PID=%d, PPID=%d\n", getpid(), getppid());
        exit(0);
    }

    wait(NULL);
    wait(NULL);
    printf("[Parent]  PID=%d, created Child1=%d, Child2=%d\n",
           getpid(), pid1, pid2);

    return 0;
}
```

**Output:**
```
[Child 1] PID=5001, PPID=5000
[Child 2] PID=5002, PPID=5000
[Parent]  PID=5000, created Child1=5001, Child2=5002
```

---

## Result

The `fork()` system call was successfully executed. A child process was created as a copy of the parent process. The parent and child were identified using their PID and PPID, and the parent waited for the child to terminate using `wait()`.
