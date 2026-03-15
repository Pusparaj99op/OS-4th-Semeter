# Experiment 05 — FCFS Process Scheduling Algorithm

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To implement the **First Come First Serve (FCFS)** process scheduling algorithm in C and calculate the waiting time, turnaround time, and average times.

---

## Theory

### CPU Scheduling

**CPU Scheduling** is the mechanism by which the operating system decides which process in the ready queue gets the CPU next. The goal is to maximize CPU utilization and ensure fair resource allocation.

### Key Terms

| Term                    | Definition                                                                     |
|-------------------------|--------------------------------------------------------------------------------|
| **Arrival Time (AT)**   | Time at which the process arrives in the ready queue                           |
| **Burst Time (BT)**     | CPU time required by the process to complete execution                         |
| **Completion Time (CT)**| Time at which the process finishes execution                                   |
| **Turnaround Time (TAT)**| Time from arrival to completion: `TAT = CT - AT`                             |
| **Waiting Time (WT)**   | Time spent waiting in the ready queue: `WT = TAT - BT`                        |
| **Response Time**       | Time from arrival until the process first gets the CPU                         |

### FCFS Algorithm

**First Come First Serve (FCFS)** is the simplest scheduling algorithm. Processes are executed in the order they arrive in the ready queue — whoever arrives first gets the CPU first.

**Characteristics:**
- **Non-preemptive**: Once a process starts, it runs to completion without interruption.
- **Simple** to implement using a FIFO queue.
- **No starvation**: Every process eventually gets the CPU.
- **Convoy Effect**: Short processes may wait a long time behind a long process.

**Example:**

| Process | AT | BT |
|---------|----|----|
| P1      |  0 | 5  |
| P2      |  1 | 3  |
| P3      |  2 | 8  |

**Gantt Chart:**
```
|  P1  |  P2  |      P3      |
0      5      8             16
```

Calculations:
- P1: CT=5,  TAT=5-0=5,  WT=5-5=0
- P2: CT=8,  TAT=8-1=7,  WT=7-3=4
- P3: CT=16, TAT=16-2=14, WT=14-8=6

Avg TAT = (5+7+14)/3 = 8.67
Avg WT  = (0+4+6)/3  = 3.33

---

## Apparatus / Requirements

| Component       | Specification                        |
|-----------------|--------------------------------------|
| Computer System | Any PC with minimum 2 GB RAM        |
| Operating System| Linux (Ubuntu 20.04 or later)        |
| Compiler        | GCC (GNU C Compiler)                 |
| Editor          | Vi / Vim / any text editor           |

---

## Procedure

1. Open the terminal and create the file: `vi fcfs.c`
2. Write the FCFS scheduling program.
3. Compile: `gcc fcfs.c -o fcfs`
4. Run: `./fcfs`
5. Enter the number of processes, arrival times, and burst times.
6. Observe: Gantt chart, completion times, turnaround times, waiting times, and averages.

---

## Program

```c
#include <stdio.h>

int main() {
    int n, i, j;
    int at[20], bt[20], ct[20], tat[20], wt[20];
    int temp_at[20], temp_bt[20], order[20];
    float avg_tat = 0, avg_wt = 0;

    printf("=== FCFS Scheduling Algorithm ===\n\n");
    printf("Enter number of processes: ");
    scanf("%d", &n);

    printf("\nEnter Arrival Time and Burst Time for each process:\n");
    for (i = 0; i < n; i++) {
        printf("Process P%d -> Arrival Time: ", i + 1);
        scanf("%d", &at[i]);
        printf("Process P%d -> Burst Time  : ", i + 1);
        scanf("%d", &bt[i]);
        temp_at[i] = at[i];
        temp_bt[i] = bt[i];
        order[i]   = i;
    }

    /* Sort processes by Arrival Time (Bubble Sort) */
    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - 1 - i; j++) {
            if (temp_at[j] > temp_at[j + 1]) {
                int tmp;
                tmp = temp_at[j]; temp_at[j] = temp_at[j+1]; temp_at[j+1] = tmp;
                tmp = temp_bt[j]; temp_bt[j] = temp_bt[j+1]; temp_bt[j+1] = tmp;
                tmp = order[j];   order[j]   = order[j+1];   order[j+1]   = tmp;
            }
        }
    }

    /* Calculate Completion Time */
    int current_time = 0;
    for (i = 0; i < n; i++) {
        if (current_time < temp_at[i])
            current_time = temp_at[i];   /* CPU idle until next process arrives */
        current_time += temp_bt[i];
        ct[i] = current_time;
    }

    /* Calculate TAT and WT */
    for (i = 0; i < n; i++) {
        tat[i] = ct[i] - temp_at[i];
        wt[i]  = tat[i] - temp_bt[i];
        avg_tat += tat[i];
        avg_wt  += wt[i];
    }
    avg_tat /= n;
    avg_wt  /= n;

    /* Display Results */
    printf("\n--- Gantt Chart ---\n|");
    int time = 0;
    for (i = 0; i < n; i++) {
        printf(" P%d |", order[i] + 1);
    }
    printf("\n0");
    time = 0;
    for (i = 0; i < n; i++) {
        if (time < temp_at[i]) time = temp_at[i];
        time += temp_bt[i];
        printf("    %d", time);
    }

    printf("\n\n%-10s %-14s %-12s %-18s %-18s %-14s\n",
           "Process", "Arrival Time", "Burst Time",
           "Completion Time", "Turnaround Time", "Waiting Time");
    printf("-----------------------------------------------------------------------\n");

    for (i = 0; i < n; i++) {
        printf("%-10s %-14d %-12d %-18d %-18d %-14d\n",
               i == 0 ? "P1" : i == 1 ? "P2" : i == 2 ? "P3" :
               i == 3 ? "P4" : "P5",
               temp_at[i], temp_bt[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time : %.2f\n", avg_tat);
    printf("Average Waiting Time    : %.2f\n",  avg_wt);

    return 0;
}
```

---

## Compilation and Execution

```bash
$ gcc fcfs.c -o fcfs
$ ./fcfs
```

---

## Output

```
=== FCFS Scheduling Algorithm ===

Enter number of processes: 4

Enter Arrival Time and Burst Time for each process:
Process P1 -> Arrival Time: 0
Process P1 -> Burst Time  : 5
Process P2 -> Arrival Time: 1
Process P2 -> Burst Time  : 3
Process P3 -> Arrival Time: 2
Process P3 -> Burst Time  : 8
Process P4 -> Arrival Time: 3
Process P4 -> Burst Time  : 2

--- Gantt Chart ---
| P1 | P2 | P3 | P4 |
0    5    8   16   18

Process    Arrival Time   Burst Time   Completion Time   Turnaround Time   Waiting Time
-----------------------------------------------------------------------
P1         0              5            5                 5                 0
P2         1              3            8                 7                 4
P3         2              8            16                14                6
P4         3              2            18                15                13

Average Turnaround Time : 10.25
Average Waiting Time    : 5.75
```

---

## Result

The FCFS (First Come First Serve) scheduling algorithm was successfully implemented in C. The arrival times, burst times, completion times, turnaround times, and waiting times for all processes were computed and displayed correctly.
