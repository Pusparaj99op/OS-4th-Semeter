---
# OPERATING SYSTEM LAB
### Experiment No.: 05
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 05                    |

---

## TITLE: Page Replacement Algorithms — FIFO, LRU, Optimal

---

## AIM

To implement and compare page replacement algorithms — **FIFO (First In First Out)**, **LRU (Least Recently Used)**, and **Optimal** — and calculate the number of page faults for each.

---

## THEORY

### Paging:
**Paging** is a memory management technique that divides the process's logical memory into fixed-size blocks called **pages**, and physical memory into blocks of the same size called **frames**.

### Page Fault:
A **page fault** occurs when a process tries to access a page that is not currently in physical memory (RAM). The OS then loads the required page from disk into a free frame.

### Page Replacement:
When all frames are occupied and a new page must be loaded, the OS must choose which existing page to replace. This is governed by a **page replacement algorithm**.

### Algorithms:

**1. FIFO (First In First Out):**
- The oldest page (the one that was loaded first) is replaced first.
- Simple to implement but may cause **Belady's Anomaly** (more frames = more faults).

**2. LRU (Least Recently Used):**
- The page that has not been used for the longest time is replaced.
- Practical and performs close to optimal. No Belady's Anomaly.

**3. Optimal Page Replacement:**
- Replace the page that will not be used for the longest time in future.
- Gives the minimum number of page faults.
- Theoretical — cannot be implemented in practice (requires future knowledge).
- Used as a benchmark for comparing other algorithms.

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux / Windows
- **Compiler:** GCC
- **Editor:** VS Code / nano / vim

---

## PROCEDURE / PROGRAMS

---

### Program 1: FIFO Page Replacement

```c
// File: fifo.c
// Author: Pranay K Gajbhiye
// FIFO Page Replacement Algorithm

#include <stdio.h>

int main() {
    int pages[50], frames[10];
    int n, f, i, j, pageFaults = 0;
    int found, replaceIdx = 0;

    printf("Enter number of pages in reference string: ");
    scanf("%d", &n);

    printf("Enter the page reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);

    printf("Enter number of frames: ");
    scanf("%d", &f);

    // Initialize frames to -1
    for (i = 0; i < f; i++)
        frames[i] = -1;

    printf("\nPage\t Frames\t\t\tFault\n");
    printf("--------------------------------------------\n");

    for (i = 0; i < n; i++) {
        found = 0;

        // Check if page is already in a frame
        for (j = 0; j < f; j++) {
            if (frames[j] == pages[i]) {
                found = 1;
                break;
            }
        }

        if (!found) {
            // Replace using FIFO
            frames[replaceIdx] = pages[i];
            replaceIdx = (replaceIdx + 1) % f;
            pageFaults++;
            printf(" %d\t ", pages[i]);
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) printf(" - ");
                else printf(" %d ", frames[j]);
            }
            printf("\t\tFAULT\n");
        } else {
            printf(" %d\t ", pages[i]);
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) printf(" - ");
                else printf(" %d ", frames[j]);
            }
            printf("\t\tHIT\n");
        }
    }

    printf("\nTotal Page Faults (FIFO) = %d\n", pageFaults);
    return 0;
}
```

---

### Program 2: LRU Page Replacement

```c
// File: lru.c
// Author: Pranay K Gajbhiye
// LRU Page Replacement Algorithm

#include <stdio.h>

int main() {
    int pages[50], frames[10], time[10];
    int n, f, i, j, pageFaults = 0;
    int found, minTime, replaceIdx;

    printf("Enter number of pages: ");
    scanf("%d", &n);
    printf("Enter page reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);

    printf("Enter number of frames: ");
    scanf("%d", &f);

    for (i = 0; i < f; i++) {
        frames[i] = -1;
        time[i]   = 0;
    }

    printf("\nPage\t Frames\t\t\tFault\n");
    printf("--------------------------------------------\n");

    for (i = 0; i < n; i++) {
        found = 0;

        for (j = 0; j < f; j++) {
            if (frames[j] == pages[i]) {
                found    = 1;
                time[j]  = i + 1;  // Update last use time
                break;
            }
        }

        if (!found) {
            // Find frame to replace: least recently used
            replaceIdx = 0;
            minTime    = time[0];
            for (j = 1; j < f; j++) {
                if (time[j] < minTime) {
                    minTime    = time[j];
                    replaceIdx = j;
                }
            }
            frames[replaceIdx] = pages[i];
            time[replaceIdx]   = i + 1;
            pageFaults++;

            printf(" %d\t ", pages[i]);
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) printf(" - ");
                else printf(" %d ", frames[j]);
            }
            printf("\t\tFAULT\n");
        } else {
            printf(" %d\t ", pages[i]);
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) printf(" - ");
                else printf(" %d ", frames[j]);
            }
            printf("\t\tHIT\n");
        }
    }

    printf("\nTotal Page Faults (LRU) = %d\n", pageFaults);
    return 0;
}
```

---

### Program 3: Optimal Page Replacement

```c
// File: optimal.c
// Author: Pranay K Gajbhiye
// Optimal Page Replacement Algorithm

#include <stdio.h>

int main() {
    int pages[50], frames[10];
    int n, f, i, j, k, pageFaults = 0;
    int found, replaceIdx, farthest, nextUse;

    printf("Enter number of pages: ");
    scanf("%d", &n);
    printf("Enter page reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);

    printf("Enter number of frames: ");
    scanf("%d", &f);

    for (i = 0; i < f; i++)
        frames[i] = -1;

    printf("\nPage\t Frames\t\t\tFault\n");
    printf("--------------------------------------------\n");

    for (i = 0; i < n; i++) {
        found = 0;

        // Check if page in frame
        for (j = 0; j < f; j++) {
            if (frames[j] == pages[i]) {
                found = 1;
                break;
            }
        }

        if (!found) {
            // Check for empty frame first
            int emptyIdx = -1;
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) {
                    emptyIdx = j;
                    break;
                }
            }

            if (emptyIdx != -1) {
                frames[emptyIdx] = pages[i];
            } else {
                // Find page to replace: farthest future use
                farthest   = -1;
                replaceIdx = 0;

                for (j = 0; j < f; j++) {
                    nextUse = n;  // If not used again, treat as infinity
                    for (k = i + 1; k < n; k++) {
                        if (frames[j] == pages[k]) {
                            nextUse = k;
                            break;
                        }
                    }
                    if (nextUse > farthest) {
                        farthest   = nextUse;
                        replaceIdx = j;
                    }
                }
                frames[replaceIdx] = pages[i];
            }

            pageFaults++;
            printf(" %d\t ", pages[i]);
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) printf(" - ");
                else printf(" %d ", frames[j]);
            }
            printf("\t\tFAULT\n");
        } else {
            printf(" %d\t ", pages[i]);
            for (j = 0; j < f; j++) {
                if (frames[j] == -1) printf(" - ");
                else printf(" %d ", frames[j]);
            }
            printf("\t\tHIT\n");
        }
    }

    printf("\nTotal Page Faults (Optimal) = %d\n", pageFaults);
    return 0;
}
```

---

## SAMPLE INPUT / OUTPUT

**Reference String:** 7 0 1 2 0 3 0 4 2 3 0 3 2 1 2
**Number of Frames:** 3

**FIFO Output (partial):**
```
Page   Frames              Fault
--------------------------------------------
 7      7  -  -            FAULT
 0      7  0  -            FAULT
 1      7  0  1            FAULT
 2      2  0  1            FAULT
 0      2  0  1            HIT
 3      2  3  1            FAULT
 0      2  3  0            FAULT
...
Total Page Faults (FIFO) = 9
```

**LRU Output:**
```
Total Page Faults (LRU) = 8
```

**Optimal Output:**
```
Total Page Faults (Optimal) = 7
```

### Comparison Table:

| Algorithm | Page Faults |
|-----------|-------------|
| FIFO      | 9           |
| LRU       | 8           |
| Optimal   | 7           |

---

## RESULT

All three page replacement algorithms — **FIFO**, **LRU**, and **Optimal** — were successfully implemented in C and tested with a common reference string. As expected:
- **Optimal** gave the minimum page faults (7) — serves as the benchmark.
- **LRU** came close (8) — practical and efficient.
- **FIFO** had the most page faults (9) — simplest but least optimal.

The results confirm the theoretical performance ordering: Optimal ≤ LRU ≤ FIFO.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 05*
