# Experiment 06 — SJF Process Scheduling Algorithm

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To implement the **Shortest Job First (SJF)** (non-preemptive) process scheduling algorithm in C and calculate waiting time, turnaround time, and average times.

---

## Theory

### Shortest Job First (SJF) Scheduling

**SJF** (also called **Shortest Job Next — SJN**) is a CPU scheduling algorithm that selects the process with the **smallest burst time** from the ready queue for execution next.

**Characteristics:**
- **Non-preemptive** version (implemented here): Once a process starts, it runs to completion.
- **Optimal**: Gives the minimum average waiting time among all non-preemptive algorithms.
- **Starvation**: Long processes may starve if short processes keep arriving.
- **Requires knowing burst time in advance** (which is difficult in practice; usually estimated).

**Preemptive version** of SJF is called **Shortest Remaining Time First (SRTF)**.

### Comparison: FCFS vs SJF

| Feature             | FCFS             | SJF (Non-Preemptive)      |
|---------------------|------------------|---------------------------|
| Selection Criteria  | Arrival order    | Shortest burst time       |
| Optimal?            | No               | Yes (min avg wait time)   |
| Starvation          | No               | Yes (long jobs may starve)|
| Convoy Effect       | Yes              | No                        |

### Example

| Process | AT | BT |
|---------|----|----|
| P1      |  0 | 6  |
| P2      |  1 | 3  |
| P3      |  2 | 8  |
| P4      |  3 | 2  |

**Gantt Chart (SJF Non-Preemptive):**
```
| P1 | P4 | P2 |    P3    |
0    6    8   11         19
```

- At t=0: Only P1 is available → P1 runs (BT=6)
- At t=6: P2(BT=3), P3(BT=8), P4(BT=2) are ready → P4 has shortest → P4 runs
- At t=8: P2(BT=3), P3(BT=8) → P2 runs
- At t=11: Only P3 → P3 runs

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

1. Open terminal and create: `vi sjf.c`
2. Write the SJF scheduling program.
3. Compile: `gcc sjf.c -o sjf`
4. Run: `./sjf`
5. Enter process details.
6. Observe the Gantt chart and scheduling metrics.

---

## Program

```c
#include <stdio.h>

#define MAX 20

int main() {
    int n, i, j, min_idx;
    int at[MAX], bt[MAX], ct[MAX], tat[MAX], wt[MAX];
    int remaining_bt[MAX], done[MAX];
    float avg_tat = 0, avg_wt = 0;
    int gantt_proc[MAX * 10], gantt_time[MAX * 10 + 1];
    int g = 0;

    printf("=== SJF Scheduling (Non-Preemptive) ===\n\n");
    printf("Enter number of processes: ");
    scanf("%d", &n);

    printf("\nEnter Arrival Time and Burst Time:\n");
    for (i = 0; i < n; i++) {
        printf("Process P%d -> Arrival Time: ", i + 1);
        scanf("%d", &at[i]);
        printf("Process P%d -> Burst Time  : ", i + 1);
        scanf("%d", &bt[i]);
        remaining_bt[i] = bt[i];
        done[i] = 0;
    }

    int current_time = 0, completed = 0;

    gantt_time[0] = 0;

    while (completed < n) {
        /* Find process with shortest BT that has arrived and not yet done */
        min_idx = -1;
        for (i = 0; i < n; i++) {
            if (!done[i] && at[i] <= current_time) {
                if (min_idx == -1 || bt[i] < bt[min_idx])
                    min_idx = i;
            }
        }

        if (min_idx == -1) {
            current_time++;   /* CPU idle */
            continue;
        }

        /* Execute selected process */
        gantt_proc[g] = min_idx + 1;
        current_time += bt[min_idx];
        ct[min_idx]  = current_time;
        gantt_time[++g] = current_time;
        done[min_idx] = 1;
        completed++;
    }

    /* Calculate TAT and WT */
    for (i = 0; i < n; i++) {
        tat[i]   = ct[i] - at[i];
        wt[i]    = tat[i] - bt[i];
        avg_tat += tat[i];
        avg_wt  += wt[i];
    }
    avg_tat /= n;
    avg_wt  /= n;

    /* Gantt Chart */
    printf("\n--- Gantt Chart ---\n|");
    for (i = 0; i < g; i++)
        printf(" P%d |", gantt_proc[i]);
    printf("\n%d", gantt_time[0]);
    for (i = 1; i <= g; i++)
        printf("    %d", gantt_time[i]);

    /* Results Table */
    printf("\n\n%-10s %-14s %-12s %-18s %-18s %-14s\n",
           "Process", "Arrival Time", "Burst Time",
           "Completion Time", "Turnaround Time", "Waiting Time");
    printf("-----------------------------------------------------------------------\n");

    for (i = 0; i < n; i++) {
        printf("P%-9d %-14d %-12d %-18d %-18d %-14d\n",
               i + 1, at[i], bt[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time : %.2f\n", avg_tat);
    printf("Average Waiting Time    : %.2f\n",  avg_wt);

    return 0;
}
```

---

## Compilation and Execution

```bash
$ gcc sjf.c -o sjf
$ ./sjf
```

---

## Output

```
=== SJF Scheduling (Non-Preemptive) ===

Enter number of processes: 4

Enter Arrival Time and Burst Time:
Process P1 -> Arrival Time: 0
Process P1 -> Burst Time  : 6
Process P2 -> Arrival Time: 1
Process P2 -> Burst Time  : 3
Process P3 -> Arrival Time: 2
Process P3 -> Burst Time  : 8
Process P4 -> Arrival Time: 3
Process P4 -> Burst Time  : 2

--- Gantt Chart ---
| P1 | P4 | P2 | P3 |
0    6    8   11   19

Process    Arrival Time   Burst Time   Completion Time   Turnaround Time   Waiting Time
-----------------------------------------------------------------------
P1         0              6            6                 6                 0
P2         1              3            11                10                7
P3         2              8            19                17                9
P4         3              2            8                 5                 3

Average Turnaround Time : 9.50
Average Waiting Time    : 4.75
```

---

## Result

The SJF (Shortest Job First) non-preemptive scheduling algorithm was successfully implemented in C. The process with the smallest burst time was selected for execution at each scheduling decision point, resulting in reduced average waiting time compared to FCFS.
