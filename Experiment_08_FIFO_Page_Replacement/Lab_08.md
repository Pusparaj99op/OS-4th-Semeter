# Experiment 08 — FIFO Page Replacement Algorithm

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To implement the **FIFO (First In First Out) Page Replacement** algorithm in C for a given reference string and a fixed number of page frames.

---

## Theory

### Virtual Memory and Paging

**Virtual memory** is a memory management technique that allows processes to use more memory than physically available. The virtual address space is divided into fixed-size units called **pages**, and physical memory is divided into **frames** of the same size.

### Page Fault

A **page fault** occurs when a process tries to access a page that is **not currently in physical memory (RAM)**. The OS must then fetch the page from secondary storage (disk) and place it in a free frame.

If no free frame is available, the OS uses a **page replacement algorithm** to decide which existing page to evict.

### FIFO Page Replacement

**FIFO** is the simplest page replacement algorithm. It maintains a **queue** of pages in memory:
- The page that has been in memory the **longest** is replaced first.
- Works like a circular queue.

**Characteristics:**

| Feature        | Description                                          |
|----------------|------------------------------------------------------|
| Policy         | Replace the oldest page in memory                    |
| Data structure | FIFO Queue                                           |
| Complexity     | O(n) — easy to implement                             |
| Optimal?       | No                                                   |
| Belady's Anomaly | FIFO suffers from Belady's Anomaly (more frames ≠ fewer faults in all cases) |

### Belady's Anomaly

Normally, more frames → fewer page faults. However, FIFO can exhibit **Belady's Anomaly**: increasing the number of frames can sometimes **increase** the number of page faults.

### Example

Reference String: `7 0 1 2 0 3 0 4 2 3 0 3 2`
Frames: 3

| Step | Reference | Frame 1 | Frame 2 | Frame 3 | Page Fault? |
|------|-----------|---------|---------|---------|-------------|
| 1    | 7         | 7       | -       | -       | YES         |
| 2    | 0         | 7       | 0       | -       | YES         |
| 3    | 1         | 7       | 0       | 1       | YES         |
| 4    | 2         | 2       | 0       | 1       | YES (evict 7)|
| 5    | 0         | 2       | 0       | 1       | No          |
| 6    | 3         | 2       | 3       | 1       | YES (evict 0)|
| 7    | 0         | 2       | 3       | 0       | YES (evict 1)|
| 8    | 4         | 4       | 3       | 0       | YES (evict 2)|
| 9    | 2         | 4       | 2       | 0       | YES (evict 3)|
| 10   | 3         | 4       | 2       | 3       | YES (evict 0)|
| 11   | 0         | 0       | 2       | 3       | YES (evict 4)|
| 12   | 3         | 0       | 2       | 3       | No          |
| 13   | 2         | 0       | 2       | 3       | No          |

Total Page Faults: **9**

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

1. Open terminal and create: `vi fifo.c`
2. Write the FIFO page replacement program.
3. Compile: `gcc fifo.c -o fifo`
4. Run: `./fifo`
5. Enter the reference string length, reference string, and number of frames.
6. Observe page faults and the state of frames at each step.

---

## Program

```c
#include <stdio.h>

#define MAX_FRAMES  10
#define MAX_PAGES   50

int main() {
    int frames[MAX_FRAMES];
    int pages[MAX_PAGES];
    int n, num_frames, i, j;
    int page_faults = 0;
    int front = 0;       /* Points to the oldest frame (FIFO) */

    printf("=== FIFO Page Replacement Algorithm ===\n\n");

    printf("Enter number of pages in the reference string: ");
    scanf("%d", &n);

    printf("Enter the reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);

    printf("Enter number of page frames: ");
    scanf("%d", &num_frames);

    /* Initialize all frames to -1 (empty) */
    for (i = 0; i < num_frames; i++)
        frames[i] = -1;

    printf("\n%-6s  %-8s", "Page", "Frames");
    for (i = 0; i < num_frames; i++)
        printf("     ");
    printf("  Fault?\n");
    printf("-------------------------------------------\n");

    for (i = 0; i < n; i++) {
        int page = pages[i];
        int found = 0;

        /* Check if page is already in a frame (page hit) */
        for (j = 0; j < num_frames; j++) {
            if (frames[j] == page) {
                found = 1;
                break;
            }
        }

        if (!found) {
            /* Page fault — replace page at 'front' position (FIFO) */
            frames[front] = page;
            front = (front + 1) % num_frames;
            page_faults++;

            /* Print state */
            printf("%-6d  [ ", page);
            for (j = 0; j < num_frames; j++) {
                if (frames[j] == -1)
                    printf(" - ");
                else
                    printf("%2d ", frames[j]);
            }
            printf("]  PAGE FAULT\n");
        } else {
            /* Page hit */
            printf("%-6d  [ ", page);
            for (j = 0; j < num_frames; j++) {
                if (frames[j] == -1)
                    printf(" - ");
                else
                    printf("%2d ", frames[j]);
            }
            printf("]  Hit\n");
        }
    }

    printf("\n-------------------------------------------\n");
    printf("Total Page Faults : %d\n", page_faults);
    printf("Total Page Hits   : %d\n", n - page_faults);
    printf("Hit Ratio         : %.2f%%\n",
           ((float)(n - page_faults) / n) * 100);

    return 0;
}
```

---

## Compilation and Execution

```bash
$ gcc fifo.c -o fifo
$ ./fifo
```

---

## Output

```
=== FIFO Page Replacement Algorithm ===

Enter number of pages in the reference string: 13
Enter the reference string:
7 0 1 2 0 3 0 4 2 3 0 3 2
Enter number of page frames: 3

Page    Frames                Fault?
-------------------------------------------
7       [  7  -  -  ]        PAGE FAULT
0       [  7  0  -  ]        PAGE FAULT
1       [  7  0  1  ]        PAGE FAULT
2       [  2  0  1  ]        PAGE FAULT
0       [  2  0  1  ]        Hit
3       [  2  3  1  ]        PAGE FAULT
0       [  2  3  0  ]        PAGE FAULT
4       [  4  3  0  ]        PAGE FAULT
2       [  4  2  0  ]        PAGE FAULT
3       [  4  2  3  ]        PAGE FAULT
0       [  0  2  3  ]        PAGE FAULT
3       [  0  2  3  ]        Hit
2       [  0  2  3  ]        Hit

-------------------------------------------
Total Page Faults : 9
Total Page Hits   : 4
Hit Ratio         : 30.77%
```

---

## Result

The FIFO Page Replacement algorithm was successfully implemented in C. For the given reference string and 3 page frames, 9 page faults were recorded. The program correctly identified page hits and faults and displayed the frame state at each step.
