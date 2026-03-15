# Experiment 07 — Non-Preemptive Priority Scheduling Algorithm

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To implement the **Non-Preemptive Priority Scheduling** algorithm in C and calculate waiting time, turnaround time, and average times.

---

## Theory

### Priority Scheduling

In **Priority Scheduling**, each process is assigned a **priority number**. The CPU is allocated to the process with the **highest priority** (convention: lower number = higher priority).

**Non-Preemptive Priority**: Once a process starts executing, it cannot be interrupted even if a higher-priority process arrives.

**Preemptive Priority (SRTF variant)**: If a new process with higher priority arrives, the running process is preempted.

### Characteristics

| Feature             | Description                                       |
|---------------------|---------------------------------------------------|
| Selection Criteria  | Highest priority (lowest priority number)         |
| Preemption          | No (non-preemptive version)                       |
| Starvation          | Yes — low-priority processes may never execute    |
| Solution to Starvation | **Aging**: gradually increase priority of waiting processes |

### Priority Types

- **Static Priority**: Assigned at creation; does not change during execution.
- **Dynamic Priority**: Changes based on waiting time or other factors (aging).

### Internal vs External Priority

- **Internal**: Determined by OS (e.g., memory requirements, CPU usage, I/O usage).
- **External**: Assigned by the user or system administrator.

### Example

| Process | AT | BT | Priority |
|---------|----|----|----------|
| P1      |  0 | 4  | 2        |
| P2      |  1 | 3  | 1 (HIGH) |
| P3      |  2 | 5  | 3        |
| P4      |  3 | 2  | 2        |

**Gantt Chart (Non-Preemptive Priority, lower = higher priority):**
```
| P1 | P2 | P4 |   P3   |
0    4    7    9       14
```
- At t=0: Only P1 is available → P1 runs
- At t=4: P2(pr=1), P3(pr=3), P4(pr=2) → P2 has highest priority
- At t=7: P3(pr=3), P4(pr=2) → P4 runs
- At t=9: P3 runs

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

1. Open terminal and create: `vi priority.c`
2. Write the priority scheduling program.
3. Compile: `gcc priority.c -o priority`
4. Run: `./priority`
5. Enter process details with priorities.
6. Observe the Gantt chart and result table.

---

## Program

```c
#include <stdio.h>

#define MAX 20

int main() {
    int n, i, j;
    int at[MAX], bt[MAX], pr[MAX], ct[MAX], tat[MAX], wt[MAX];
    int done[MAX];
    float avg_tat = 0, avg_wt = 0;
    int gantt_proc[MAX], gantt_time[MAX + 1];
    int g = 0;

    printf("=== Non-Preemptive Priority Scheduling ===\n");
    printf("(Lower priority number = Higher priority)\n\n");
    printf("Enter number of processes: ");
    scanf("%d", &n);

    printf("\nEnter Arrival Time, Burst Time, and Priority:\n");
    for (i = 0; i < n; i++) {
        printf("Process P%d -> Arrival Time: ", i + 1);
        scanf("%d", &at[i]);
        printf("Process P%d -> Burst Time  : ", i + 1);
        scanf("%d", &bt[i]);
        printf("Process P%d -> Priority    : ", i + 1);
        scanf("%d", &pr[i]);
        done[i] = 0;
    }

    int current_time = 0, completed = 0;
    gantt_time[0] = 0;

    while (completed < n) {
        /* Find highest priority process that has arrived */
        int sel = -1;
        for (i = 0; i < n; i++) {
            if (!done[i] && at[i] <= current_time) {
                if (sel == -1 || pr[i] < pr[sel])
                    sel = i;
            }
        }

        if (sel == -1) {
            current_time++;   /* CPU idle */
            continue;
        }

        gantt_proc[g] = sel + 1;
        current_time += bt[sel];
        ct[sel] = current_time;
        gantt_time[++g] = current_time;
        done[sel] = 1;
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
    printf("\n\n%-10s %-14s %-12s %-10s %-18s %-18s %-14s\n",
           "Process", "Arrival Time", "Burst Time", "Priority",
           "Completion Time", "Turnaround Time", "Waiting Time");
    printf("------------------------------------------------------------------------------\n");

    for (i = 0; i < n; i++) {
        printf("P%-9d %-14d %-12d %-10d %-18d %-18d %-14d\n",
               i + 1, at[i], bt[i], pr[i], ct[i], tat[i], wt[i]);
    }

    printf("\nAverage Turnaround Time : %.2f\n", avg_tat);
    printf("Average Waiting Time    : %.2f\n",  avg_wt);

    return 0;
}
```

---

## Compilation and Execution

```bash
$ gcc priority.c -o priority
$ ./priority
```

---

## Output

```
=== Non-Preemptive Priority Scheduling ===
(Lower priority number = Higher priority)

Enter number of processes: 4

Enter Arrival Time, Burst Time, and Priority:
Process P1 -> Arrival Time: 0
Process P1 -> Burst Time  : 4
Process P1 -> Priority    : 2
Process P2 -> Arrival Time: 1
Process P2 -> Burst Time  : 3
Process P2 -> Priority    : 1
Process P3 -> Arrival Time: 2
Process P3 -> Burst Time  : 5
Process P3 -> Priority    : 3
Process P4 -> Arrival Time: 3
Process P4 -> Burst Time  : 2
Process P4 -> Priority    : 2

--- Gantt Chart ---
| P1 | P2 | P4 | P3 |
0    4    7    9   14

Process    Arrival Time   Burst Time   Priority   Completion Time   Turnaround Time   Waiting Time
------------------------------------------------------------------------------
P1         0              4            2          4                 4                 0
P2         1              3            1          7                 6                 3
P3         2              5            3          14                12                7
P4         3              2            2          9                 6                 4

Average Turnaround Time : 7.00
Average Waiting Time    : 3.50
```

---

## Result

The Non-Preemptive Priority Scheduling algorithm was successfully implemented in C. Processes were executed in the order of their priority (lower number = higher priority), and the Gantt chart along with turnaround and waiting times were computed correctly.
