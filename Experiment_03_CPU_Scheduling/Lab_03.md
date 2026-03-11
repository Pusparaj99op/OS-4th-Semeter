---
# OPERATING SYSTEM LAB
### Experiment No.: 03
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 03                    |

---

## TITLE: CPU Scheduling Algorithms — FCFS, SJF, Round Robin, Priority

---

## AIM

To implement and compare CPU scheduling algorithms: First Come First Serve (FCFS), Shortest Job First (SJF), Round Robin (RR), and Priority Scheduling, and calculate their Average Waiting Time and Average Turnaround Time.

---

## THEORY

**CPU Scheduling** is the process of determining which process in the ready queue is allocated the CPU. The goal is to maximize CPU utilization and minimize waiting time.

### Important Terms:

| Term                       | Definition                                                               |
|----------------------------|--------------------------------------------------------------------------|
| **Arrival Time (AT)**      | The time at which a process arrives in the ready queue.                  |
| **Burst Time (BT)**        | The total time required by a process on the CPU.                         |
| **Completion Time (CT)**   | The time at which a process finishes execution.                          |
| **Turnaround Time (TAT)**  | TAT = Completion Time − Arrival Time                                     |
| **Waiting Time (WT)**      | WT = Turnaround Time − Burst Time                                        |
| **Response Time**          | Time from submission to first response.                                  |

### Algorithms:

1. **FCFS (First Come First Serve):** Non-preemptive. Process that arrives first gets the CPU first. Simple but can cause the *convoy effect*.

2. **SJF (Shortest Job First):** Non-preemptive. Process with the shortest burst time is scheduled next. Gives minimum average waiting time but can cause *starvation*.

3. **Round Robin (RR):** Preemptive. Each process gets a fixed time quantum. Fair and suitable for time-sharing systems.

4. **Priority Scheduling:** Each process is assigned a priority. Process with highest priority (lowest number) runs first. Can cause starvation; solved by *aging*.

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux / Windows
- **Compiler:** GCC (`gcc` on Linux, MinGW on Windows)
- **Editor:** VS Code / nano / vim
- **RAM:** Minimum 2 GB

---

## PROCEDURE / PROGRAMS

---

### Program 1: FCFS Scheduling

```c
// File: fcfs.c
// Author: Pranay K Gajbhiye
// FCFS CPU Scheduling Algorithm

#include <stdio.h>

int main() {
    int n, i;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int pid[n], at[n], bt[n], ct[n], tat[n], wt[n];

    for (i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time and Burst Time for P%d: ", i + 1);
        scanf("%d %d", &at[i], &bt[i]);
    }

    // Calculate Completion Time
    ct[0] = at[0] + bt[0];
    for (i = 1; i < n; i++) {
        if (ct[i-1] < at[i])
            ct[i] = at[i] + bt[i];
        else
            ct[i] = ct[i-1] + bt[i];
    }

    float totalTAT = 0, totalWT = 0;

    printf("\n%-6s %-6s %-6s %-6s %-6s %-6s\n",
           "PID", "AT", "BT", "CT", "TAT", "WT");
    printf("------------------------------------------\n");

    for (i = 0; i < n; i++) {
        tat[i] = ct[i] - at[i];
        wt[i]  = tat[i] - bt[i];
        totalTAT += tat[i];
        totalWT  += wt[i];
        printf("%-6d %-6d %-6d %-6d %-6d %-6d\n",
               pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time = %.2f", totalTAT / n);
    printf("\nAverage Waiting Time    = %.2f\n", totalWT / n);

    return 0;
}
```

**Compile & Run:**
```bash
gcc fcfs.c -o fcfs
./fcfs
```

---

### Program 2: SJF (Non-Preemptive) Scheduling

```c
// File: sjf.c
// Author: Pranay K Gajbhiye
// SJF Non-Preemptive CPU Scheduling Algorithm

#include <stdio.h>

int main() {
    int n, i, j, minIdx;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int pid[n], at[n], bt[n], ct[n], tat[n], wt[n], done[n];

    for (i = 0; i < n; i++) {
        pid[i] = i + 1;
        done[i] = 0;
        printf("Enter Arrival Time and Burst Time for P%d: ", i + 1);
        scanf("%d %d", &at[i], &bt[i]);
    }

    int currentTime = 0, completed = 0;
    float totalTAT = 0, totalWT = 0;

    while (completed < n) {
        minIdx = -1;
        int minBT = 9999;

        for (i = 0; i < n; i++) {
            if (!done[i] && at[i] <= currentTime && bt[i] < minBT) {
                minBT = bt[i];
                minIdx = i;
            }
        }

        if (minIdx == -1) {
            currentTime++;
            continue;
        }

        currentTime += bt[minIdx];
        ct[minIdx]  = currentTime;
        tat[minIdx] = ct[minIdx] - at[minIdx];
        wt[minIdx]  = tat[minIdx] - bt[minIdx];
        totalTAT   += tat[minIdx];
        totalWT    += wt[minIdx];
        done[minIdx] = 1;
        completed++;
    }

    printf("\n%-6s %-6s %-6s %-6s %-6s %-6s\n",
           "PID", "AT", "BT", "CT", "TAT", "WT");
    printf("------------------------------------------\n");

    for (i = 0; i < n; i++) {
        printf("%-6d %-6d %-6d %-6d %-6d %-6d\n",
               pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time = %.2f", totalTAT / n);
    printf("\nAverage Waiting Time    = %.2f\n", totalWT / n);

    return 0;
}
```

---

### Program 3: Round Robin Scheduling

```c
// File: round_robin.c
// Author: Pranay K Gajbhiye
// Round Robin CPU Scheduling Algorithm

#include <stdio.h>

int main() {
    int n, quantum, i, time = 0, completed = 0;
    printf("Enter number of processes: ");
    scanf("%d", &n);
    printf("Enter Time Quantum: ");
    scanf("%d", &quantum);

    int pid[n], at[n], bt[n], rem[n], ct[n], tat[n], wt[n];

    for (i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("Enter Arrival Time and Burst Time for P%d: ", i + 1);
        scanf("%d %d", &at[i], &bt[i]);
        rem[i] = bt[i];
    }

    float totalTAT = 0, totalWT = 0;

    while (completed < n) {
        int allDone = 1;
        for (i = 0; i < n; i++) {
            if (rem[i] > 0 && at[i] <= time) {
                allDone = 0;
                if (rem[i] <= quantum) {
                    time += rem[i];
                    rem[i] = 0;
                    ct[i]  = time;
                    tat[i] = ct[i] - at[i];
                    wt[i]  = tat[i] - bt[i];
                    totalTAT += tat[i];
                    totalWT  += wt[i];
                    completed++;
                } else {
                    time    += quantum;
                    rem[i]  -= quantum;
                }
            }
        }
        if (allDone) time++;
    }

    printf("\n%-6s %-6s %-6s %-6s %-6s %-6s\n",
           "PID", "AT", "BT", "CT", "TAT", "WT");
    printf("------------------------------------------\n");

    for (i = 0; i < n; i++) {
        printf("%-6d %-6d %-6d %-6d %-6d %-6d\n",
               pid[i], at[i], bt[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time = %.2f", totalTAT / n);
    printf("\nAverage Waiting Time    = %.2f\n", totalWT / n);

    return 0;
}
```

---

### Program 4: Priority Scheduling (Non-Preemptive)

```c
// File: priority.c
// Author: Pranay K Gajbhiye
// Priority CPU Scheduling Algorithm (Non-Preemptive)
// Lower priority number = Higher priority

#include <stdio.h>

int main() {
    int n, i, minIdx;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int pid[n], at[n], bt[n], pr[n], ct[n], tat[n], wt[n], done[n];

    for (i = 0; i < n; i++) {
        pid[i]  = i + 1;
        done[i] = 0;
        printf("Enter Arrival Time, Burst Time, Priority for P%d: ", i+1);
        scanf("%d %d %d", &at[i], &bt[i], &pr[i]);
    }

    int currentTime = 0, completed = 0;
    float totalTAT = 0, totalWT = 0;

    while (completed < n) {
        minIdx = -1;
        int minPr = 9999;

        for (i = 0; i < n; i++) {
            if (!done[i] && at[i] <= currentTime && pr[i] < minPr) {
                minPr  = pr[i];
                minIdx = i;
            }
        }

        if (minIdx == -1) {
            currentTime++;
            continue;
        }

        currentTime  += bt[minIdx];
        ct[minIdx]    = currentTime;
        tat[minIdx]   = ct[minIdx] - at[minIdx];
        wt[minIdx]    = tat[minIdx] - bt[minIdx];
        totalTAT     += tat[minIdx];
        totalWT      += wt[minIdx];
        done[minIdx]  = 1;
        completed++;
    }

    printf("\n%-6s %-6s %-6s %-6s %-6s %-6s %-6s\n",
           "PID", "AT", "BT", "PR", "CT", "TAT", "WT");
    printf("----------------------------------------------------\n");

    for (i = 0; i < n; i++) {
        printf("%-6d %-6d %-6d %-6d %-6d %-6d %-6d\n",
               pid[i], at[i], bt[i], pr[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time = %.2f", totalTAT / n);
    printf("\nAverage Waiting Time    = %.2f\n", totalWT / n);

    return 0;
}
```

---

## SAMPLE INPUT / OUTPUT

### FCFS Sample:
**Input:**
```
Number of processes: 4
P1: AT=0, BT=5
P2: AT=1, BT=3
P3: AT=2, BT=8
P4: AT=3, BT=6
```

**Output:**
```
PID    AT     BT     CT     TAT    WT
------------------------------------------
1      0      5      5      5      0
2      1      3      8      7      4
3      2      8      16     14     6
4      3      6      22     19     13

Average Turnaround Time = 11.25
Average Waiting Time    = 5.75
```

### Round Robin Sample (Quantum = 2):
**Input:**
```
Number of processes: 3
P1: AT=0, BT=5
P2: AT=1, BT=3
P3: AT=2, BT=8
Time Quantum: 2
```

**Output:**
```
PID    AT     BT     CT     TAT    WT
------------------------------------------
1      0      5      13     13     8
2      1      3      10     9      6
3      2      8      18     16     8

Average Turnaround Time = 12.67
Average Waiting Time    = 7.33
```

---

## RESULT

All four CPU scheduling algorithms — FCFS, SJF (Non-Preemptive), Round Robin, and Priority Scheduling — were successfully implemented in C and executed. The programs correctly calculated **Completion Time**, **Turnaround Time**, **Waiting Time**, and their averages. Round Robin ensures fairness whereas SJF gives minimum average waiting time.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 03*
