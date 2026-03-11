---
# OPERATING SYSTEM LAB
### Experiment No.: 04
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 04                    |

---

## TITLE: Banker's Algorithm — Deadlock Avoidance

---

## AIM

To implement the **Banker's Algorithm** for deadlock avoidance and determine whether the system is in a safe state, and if so, find the safe sequence.

---

## THEORY

### Deadlock:
A **deadlock** is a situation where a set of processes are blocked because each process is waiting for a resource that is held by another process, resulting in a circular wait.

### Four Conditions for Deadlock (Coffman Conditions):
1. **Mutual Exclusion** — Only one process can use a resource at a time.
2. **Hold and Wait** — A process holding resources can request more.
3. **No Preemption** — Resources cannot be forcibly taken from a process.
4. **Circular Wait** — A circular chain of processes waiting for each other.

### Banker's Algorithm:
Developed by **Edsger Dijkstra**, the Banker's Algorithm is a resource-allocation and deadlock-avoidance algorithm. It simulates a bank that only approves loans if the bank can still satisfy all clients even after granting the loan.

### Data Structures Used:

| Structure         | Description                                              |
|-------------------|----------------------------------------------------------|
| `Available[j]`    | Number of available instances of resource type j        |
| `Max[i][j]`       | Maximum demand of process i for resource type j         |
| `Allocation[i][j]`| Resources currently allocated to process i              |
| `Need[i][j]`      | Remaining resources needed: `Need = Max - Allocation`   |

### Safety Algorithm:
1. Let `Work = Available` and `Finish[i] = false` for all i.
2. Find an index i such that: `Finish[i] == false` and `Need[i] <= Work`.
3. If found: `Work = Work + Allocation[i]`, `Finish[i] = true`, goto Step 2.
4. If `Finish[i] == true` for all i → System is in **SAFE STATE**.

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux / Windows
- **Compiler:** GCC
- **Editor:** VS Code / nano / vim

---

## PROCEDURE / PROGRAM

```c
// File: bankers.c
// Author: Pranay K Gajbhiye
// Banker's Algorithm - Deadlock Avoidance

#include <stdio.h>

#define MAX_PROCESSES 10
#define MAX_RESOURCES 10

int main() {
    int n, m;  // n = processes, m = resource types
    int alloc[MAX_PROCESSES][MAX_RESOURCES];
    int maxDemand[MAX_PROCESSES][MAX_RESOURCES];
    int need[MAX_PROCESSES][MAX_RESOURCES];
    int available[MAX_RESOURCES];
    int finish[MAX_PROCESSES];
    int safeSeq[MAX_PROCESSES];
    int work[MAX_RESOURCES];
    int i, j, k, count = 0;

    printf("Enter number of processes: ");
    scanf("%d", &n);
    printf("Enter number of resource types: ");
    scanf("%d", &m);

    printf("\nEnter Allocation Matrix (%d x %d):\n", n, m);
    for (i = 0; i < n; i++) {
        printf("Process P%d: ", i);
        for (j = 0; j < m; j++)
            scanf("%d", &alloc[i][j]);
    }

    printf("\nEnter Max Demand Matrix (%d x %d):\n", n, m);
    for (i = 0; i < n; i++) {
        printf("Process P%d: ", i);
        for (j = 0; j < m; j++)
            scanf("%d", &maxDemand[i][j]);
    }

    printf("\nEnter Available Resources (%d values): ", m);
    for (j = 0; j < m; j++)
        scanf("%d", &available[j]);

    // Calculate Need Matrix
    printf("\n--- Need Matrix ---\n");
    for (i = 0; i < n; i++) {
        printf("P%d: ", i);
        for (j = 0; j < m; j++) {
            need[i][j] = maxDemand[i][j] - alloc[i][j];
            printf("%d ", need[i][j]);
        }
        printf("\n");
    }

    // Initialize finish and work arrays
    for (i = 0; i < n; i++) finish[i] = 0;
    for (j = 0; j < m; j++) work[j] = available[j];

    // Safety Algorithm
    while (count < n) {
        int found = 0;
        for (i = 0; i < n; i++) {
            if (!finish[i]) {
                int canAllocate = 1;
                for (j = 0; j < m; j++) {
                    if (need[i][j] > work[j]) {
                        canAllocate = 0;
                        break;
                    }
                }
                if (canAllocate) {
                    for (j = 0; j < m; j++)
                        work[j] += alloc[i][j];
                    safeSeq[count++] = i;
                    finish[i] = 1;
                    found = 1;
                }
            }
        }
        if (!found) {
            printf("\n*** SYSTEM IS IN UNSAFE STATE! DEADLOCK MAY OCCUR ***\n");
            return 0;
        }
    }

    // All processes finished — Safe State
    printf("\n*** SYSTEM IS IN SAFE STATE ***\n");
    printf("Safe Sequence: ");
    for (i = 0; i < n; i++) {
        printf("P%d", safeSeq[i]);
        if (i < n - 1) printf(" --> ");
    }
    printf("\n");

    return 0;
}
```

**Compile & Run:**
```bash
gcc bankers.c -o bankers
./bankers
```

---

## SAMPLE INPUT

```
Enter number of processes: 5
Enter number of resource types: 3

Allocation Matrix:
P0: 0 1 0
P1: 2 0 0
P2: 3 0 2
P3: 2 1 1
P4: 0 0 2

Max Demand Matrix:
P0: 7 5 3
P1: 3 2 2
P2: 9 0 2
P3: 2 2 2
P4: 4 3 3

Available Resources: 3 3 2
```

---

## OUTPUT

```
--- Need Matrix ---
P0: 7 4 3
P1: 1 2 2
P2: 6 0 0
P3: 0 1 1
P4: 4 3 1

*** SYSTEM IS IN SAFE STATE ***
Safe Sequence: P1 --> P3 --> P4 --> P0 --> P2
```

---

## RESULT

The Banker's Algorithm was successfully implemented in C. The program takes the Allocation Matrix, Maximum Demand Matrix, and Available Resources as input, computes the Need Matrix, and then runs the Safety Algorithm. The system was found to be in a **safe state** with the safe sequence **P1 → P3 → P4 → P0 → P2**, confirming that no deadlock will occur.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 04*
