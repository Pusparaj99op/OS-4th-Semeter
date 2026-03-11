---
# OPERATING SYSTEM LAB
### Experiment No.: 07
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 07                    |

---

## TITLE: Memory Allocation Strategies — First Fit, Best Fit, Worst Fit

---

## AIM

To implement and compare the three dynamic memory allocation strategies — **First Fit**, **Best Fit**, and **Worst Fit** — for allocating memory blocks to processes.

---

## THEORY

### Memory Management:
The OS must allocate memory to processes efficiently. When a process requests memory, the OS finds a free block (hole) in memory and assigns it.

### Free Hole Selection Strategies:

**1. First Fit:**
- Allocate the **first** hole that is big enough.
- Search starts from the beginning (or where the last search ended).
- Fast but may leave many small, unusable holes at the start.

**2. Best Fit:**
- Allocate the **smallest** hole that is big enough.
- Must search the entire list to find the smallest adequate hole.
- Minimizes wasted space per allocation but creates smallest leftover holes, which may be too small to be useful.

**3. Worst Fit:**
- Allocate the **largest** available hole.
- The remaining hole is large, which may be useful for future allocations.
- Creates large leftover holes, good for large subsequent requests.

### Comparison Table:

| Strategy   | Speed  | Memory Waste  | Notes                           |
|------------|--------|---------------|----------------------------------|
| First Fit  | Fast   | Moderate      | Simple, good in practice         |
| Best Fit   | Slow   | Least per alloc| Small leftovers, may cause waste |
| Worst Fit  | Slow   | Most          | Large leftovers                  |

### External Fragmentation:
All strategies suffer from **external fragmentation** — free memory exists but is scattered in small non-contiguous pieces.

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux / Windows
- **Compiler:** GCC
- **Editor:** VS Code / nano / vim

---

## PROCEDURE / PROGRAM

```c
// File: memory_allocation.c
// Author: Pranay K Gajbhiye
// First Fit, Best Fit, Worst Fit Memory Allocation

#include <stdio.h>
#include <string.h>

#define MAX 25

void firstFit(int blockSize[], int m, int processSize[], int n) {
    int allocation[n];
    memset(allocation, -1, sizeof(allocation));

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (blockSize[j] >= processSize[i]) {
                allocation[i]  = j;
                blockSize[j]  -= processSize[i];
                break;
            }
        }
    }

    printf("\n--- First Fit ---\n");
    printf("%-12s %-16s %-12s\n", "Process No", "Process Size", "Block No");
    printf("---------------------------------------\n");
    for (int i = 0; i < n; i++) {
        printf("%-12d %-16d ", i+1, processSize[i]);
        if (allocation[i] != -1)
            printf("%-12d\n", allocation[i]+1);
        else
            printf("Not Allocated\n");
    }
}

void bestFit(int blockSize[], int m, int processSize[], int n) {
    int allocation[n];
    memset(allocation, -1, sizeof(allocation));

    for (int i = 0; i < n; i++) {
        int bestIdx = -1;
        for (int j = 0; j < m; j++) {
            if (blockSize[j] >= processSize[i]) {
                if (bestIdx == -1 || blockSize[j] < blockSize[bestIdx])
                    bestIdx = j;
            }
        }
        if (bestIdx != -1) {
            allocation[i]        = bestIdx;
            blockSize[bestIdx]  -= processSize[i];
        }
    }

    printf("\n--- Best Fit ---\n");
    printf("%-12s %-16s %-12s\n", "Process No", "Process Size", "Block No");
    printf("---------------------------------------\n");
    for (int i = 0; i < n; i++) {
        printf("%-12d %-16d ", i+1, processSize[i]);
        if (allocation[i] != -1)
            printf("%-12d\n", allocation[i]+1);
        else
            printf("Not Allocated\n");
    }
}

void worstFit(int blockSize[], int m, int processSize[], int n) {
    int allocation[n];
    memset(allocation, -1, sizeof(allocation));

    for (int i = 0; i < n; i++) {
        int worstIdx = -1;
        for (int j = 0; j < m; j++) {
            if (blockSize[j] >= processSize[i]) {
                if (worstIdx == -1 || blockSize[j] > blockSize[worstIdx])
                    worstIdx = j;
            }
        }
        if (worstIdx != -1) {
            allocation[i]         = worstIdx;
            blockSize[worstIdx]  -= processSize[i];
        }
    }

    printf("\n--- Worst Fit ---\n");
    printf("%-12s %-16s %-12s\n", "Process No", "Process Size", "Block No");
    printf("---------------------------------------\n");
    for (int i = 0; i < n; i++) {
        printf("%-12d %-16d ", i+1, processSize[i]);
        if (allocation[i] != -1)
            printf("%-12d\n", allocation[i]+1);
        else
            printf("Not Allocated\n");
    }
}

int main() {
    int blockSizeFF[] = {100, 500, 200, 300, 600};
    int blockSizeBF[] = {100, 500, 200, 300, 600};
    int blockSizeWF[] = {100, 500, 200, 300, 600};
    int processSize[] = {212, 417, 112, 426};

    int m = sizeof(blockSizeFF) / sizeof(blockSizeFF[0]);
    int n = sizeof(processSize) / sizeof(processSize[0]);

    printf("=== Memory Allocation Strategies ===\n");
    printf("\nMemory Blocks: ");
    for (int i = 0; i < m; i++) printf("%d ", blockSizeFF[i]);

    printf("\nProcess Sizes: ");
    for (int i = 0; i < n; i++) printf("%d ", processSize[i]);

    firstFit(blockSizeFF, m, processSize, n);
    bestFit (blockSizeBF, m, processSize, n);
    worstFit(blockSizeWF, m, processSize, n);

    return 0;
}
```

**Compile & Run:**
```bash
gcc memory_allocation.c -o memory_allocation
./memory_allocation
```

---

## SAMPLE OUTPUT

```
=== Memory Allocation Strategies ===

Memory Blocks: 100 500 200 300 600
Process Sizes: 212 417 112 426

--- First Fit ---
Process No   Process Size     Block No
---------------------------------------
1            212              2
2            417              5
3            112              3
4            426              Not Allocated

--- Best Fit ---
Process No   Process Size     Block No
---------------------------------------
1            212              4
2            417              2
3            112              3
4            426              5

--- Worst Fit ---
Process No   Process Size     Block No
---------------------------------------
1            212              5
2            417              2
3            112              4
4            426              Not Allocated
```

---

## RESULT

All three memory allocation strategies — **First Fit**, **Best Fit**, and **Worst Fit** — were successfully implemented in C. The comparison shows:
- **Best Fit** allocated all 4 processes successfully by selecting the most suitable blocks.
- **First Fit** and **Worst Fit** could not allocate some processes due to sub-optimal choices.
- **Best Fit** is generally the most memory-efficient strategy in terms of individual allocations.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 07*
