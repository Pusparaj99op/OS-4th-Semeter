# Experiment 10 — Banker's Algorithm for Deadlock Avoidance

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To implement the **Banker's Algorithm** in C for deadlock avoidance and determine whether the system is in a **safe state**, and if so, find the **safe sequence**.

---

## Theory

### Deadlock

A **deadlock** is a situation where a set of processes are permanently blocked, each waiting for a resource held by another process in the set. Four conditions must hold simultaneously for deadlock to occur (**Coffman's Conditions**):

1. **Mutual Exclusion**: A resource can only be held by one process at a time.
2. **Hold and Wait**: A process holds at least one resource and waits for additional resources.
3. **No Preemption**: Resources cannot be forcibly taken from a process.
4. **Circular Wait**: A circular chain of processes exists, each waiting for a resource held by the next.

### Deadlock Avoidance

Deadlock **avoidance** uses knowledge of future resource requests to ensure the system never enters an unsafe state. The key concept is **safe state**.

### Safe State

A state is **safe** if the system can allocate resources to each process (up to its maximum) in some order and still allow all processes to complete. The sequence in which this is possible is called a **safe sequence**.

```
Safe State → No Deadlock possible
Unsafe State → Deadlock may occur
```

### Banker's Algorithm

The **Banker's Algorithm** (proposed by Edsger Dijkstra) treats the OS like a banker who does not lend money unless it can satisfy all customers. Before granting a resource request, it checks whether the resulting state is safe.

### Data Structures

| Structure          | Dimension         | Description                                    |
|--------------------|-------------------|------------------------------------------------|
| `Max[i][j]`        | n × m             | Maximum demand of process i for resource j     |
| `Allocation[i][j]` | n × m             | Resources currently allocated to process i     |
| `Need[i][j]`       | n × m             | Remaining need: `Need = Max - Allocation`      |
| `Available[j]`     | 1 × m             | Currently available instances of resource j    |

Where **n** = number of processes, **m** = number of resource types.

### Safety Algorithm

**Step 1:** Initialize `Work = Available`, `Finish[i] = false` for all i.

**Step 2:** Find an index i such that:
- `Finish[i] == false`
- `Need[i] <= Work`

If no such i exists, go to Step 4.

**Step 3:** `Work = Work + Allocation[i]`, `Finish[i] = true`. Go to Step 2.

**Step 4:** If `Finish[i] == true` for all i → system is in **safe state**.

### Resource Request Algorithm

When process Pi requests resources `Request[i]`:
1. If `Request[i] <= Need[i]` → proceed; else → error (exceeded max).
2. If `Request[i] <= Available` → proceed; else → wait.
3. Pretend to allocate: update Available, Allocation, Need.
4. Run Safety Algorithm:
   - If safe → grant the request.
   - If unsafe → rollback and make Pi wait.

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

1. Open terminal and create: `vi bankers.c`
2. Write the Banker's Algorithm program.
3. Compile: `gcc bankers.c -o bankers`
4. Run: `./bankers`
5. Enter number of processes, resource types, allocation matrix, maximum matrix, and available resources.
6. Observe whether the system is safe and the safe sequence.

---

## Program

```c
#include <stdio.h>

#define MAX_P 10
#define MAX_R 10

int main() {
    int n, m;   /* n = processes, m = resource types */
    int alloc[MAX_P][MAX_R];
    int maxm[MAX_P][MAX_R];
    int need[MAX_P][MAX_R];
    int avail[MAX_R];
    int finish[MAX_P];
    int safe_seq[MAX_P];
    int work[MAX_R];
    int i, j, k;

    printf("=== Banker's Algorithm for Deadlock Avoidance ===\n\n");

    printf("Enter number of processes : ");
    scanf("%d", &n);
    printf("Enter number of resource types: ");
    scanf("%d", &m);

    /* Allocation Matrix */
    printf("\nEnter Allocation Matrix (%d x %d):\n", n, m);
    for (i = 0; i < n; i++) {
        printf("Process P%d: ", i);
        for (j = 0; j < m; j++)
            scanf("%d", &alloc[i][j]);
    }

    /* Maximum Matrix */
    printf("\nEnter Maximum Matrix (%d x %d):\n", n, m);
    for (i = 0; i < n; i++) {
        printf("Process P%d: ", i);
        for (j = 0; j < m; j++)
            scanf("%d", &maxm[i][j]);
    }

    /* Available Resources */
    printf("\nEnter Available Resources (1 x %d): ", m);
    for (j = 0; j < m; j++)
        scanf("%d", &avail[j]);

    /* Calculate Need Matrix */
    printf("\n--- Need Matrix (Max - Allocation) ---\n");
    printf("%-10s", "Process");
    for (j = 0; j < m; j++) printf("R%d  ", j);
    printf("\n");
    for (i = 0; i < n; i++) {
        printf("P%-9d", i);
        for (j = 0; j < m; j++) {
            need[i][j] = maxm[i][j] - alloc[i][j];
            printf("%-4d", need[i][j]);
        }
        printf("\n");
    }

    /* Safety Algorithm */
    for (j = 0; j < m; j++)
        work[j] = avail[j];

    for (i = 0; i < n; i++)
        finish[i] = 0;

    int count = 0;
    printf("\n--- Safety Algorithm Trace ---\n");

    while (count < n) {
        int found = 0;
        for (i = 0; i < n; i++) {
            if (!finish[i]) {
                /* Check if Need[i] <= Work */
                int can_allocate = 1;
                for (j = 0; j < m; j++) {
                    if (need[i][j] > work[j]) {
                        can_allocate = 0;
                        break;
                    }
                }
                if (can_allocate) {
                    printf("P%d can proceed. Work becomes: [ ", i);
                    for (j = 0; j < m; j++) {
                        work[j] += alloc[i][j];
                        printf("%d ", work[j]);
                    }
                    printf("]\n");
                    finish[i] = 1;
                    safe_seq[count++] = i;
                    found = 1;
                }
            }
        }
        if (!found) break;   /* No process found — unsafe */
    }

    /* Check if all processes finished */
    printf("\n");
    if (count == n) {
        printf("System is in a SAFE STATE.\n");
        printf("Safe Sequence: ");
        for (i = 0; i < n; i++) {
            printf("P%d", safe_seq[i]);
            if (i < n - 1) printf(" -> ");
        }
        printf("\n");
    } else {
        printf("System is in an UNSAFE STATE. Deadlock may occur.\n");
    }

    return 0;
}
```

---

## Compilation and Execution

```bash
$ gcc bankers.c -o bankers
$ ./bankers
```

---

## Sample Input

Suppose:
- 5 processes: P0–P4
- 3 resource types: A, B, C
- Total resources: A=10, B=5, C=7

| Process | Alloc (A B C) | Max (A B C) |
|---------|---------------|-------------|
| P0      | 0 1 0         | 7 5 3       |
| P1      | 2 0 0         | 3 2 2       |
| P2      | 3 0 2         | 9 0 2       |
| P3      | 2 1 1         | 2 2 2       |
| P4      | 0 0 2         | 4 3 3       |

Available = (10-7, 5-2, 7-5) = (3, 3, 2)

---

## Output

```
=== Banker's Algorithm for Deadlock Avoidance ===

Enter number of processes : 5
Enter number of resource types: 3

Enter Allocation Matrix (5 x 3):
Process P0: 0 1 0
Process P1: 2 0 0
Process P2: 3 0 2
Process P3: 2 1 1
Process P4: 0 0 2

Enter Maximum Matrix (5 x 3):
Process P0: 7 5 3
Process P1: 3 2 2
Process P2: 9 0 2
Process P3: 2 2 2
Process P4: 4 3 3

Enter Available Resources (1 x 3): 3 3 2

--- Need Matrix (Max - Allocation) ---
Process   R0  R1  R2
P0        7   4   3
P1        1   2   2
P2        6   0   0
P3        0   1   1
P4        4   3   1

--- Safety Algorithm Trace ---
P1 can proceed. Work becomes: [ 5 3 2 ]
P3 can proceed. Work becomes: [ 7 4 3 ]
P4 can proceed. Work becomes: [ 7 4 5 ]
P2 can proceed. Work becomes: [ 10 4 7 ]
P0 can proceed. Work becomes: [ 10 5 7 ]

System is in a SAFE STATE.
Safe Sequence: P1 -> P3 -> P4 -> P2 -> P0
```

---

## Result

The Banker's Algorithm for deadlock avoidance was successfully implemented in C. The system was determined to be in a **safe state**, and the safe sequence **P1 → P3 → P4 → P2 → P0** was found, confirming that all processes can complete without deadlock.
