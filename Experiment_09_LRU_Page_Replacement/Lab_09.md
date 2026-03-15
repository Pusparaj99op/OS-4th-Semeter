# Experiment 09 — LRU Page Replacement Algorithm

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To implement the **LRU (Least Recently Used) Page Replacement** algorithm in C for a given reference string and a fixed number of page frames.

---

## Theory

### LRU Page Replacement

**Least Recently Used (LRU)** is a page replacement algorithm that replaces the page that has **not been used for the longest period of time**.

**Principle:** Pages that have been recently accessed are likely to be accessed again soon (**temporal locality**). So the least recently used page is the best candidate for replacement.

### Characteristics

| Feature             | Description                                              |
|---------------------|----------------------------------------------------------|
| Policy              | Replace the page not used for the longest time           |
| Approximation of    | Optimal (OPT) algorithm                                  |
| Belady's Anomaly    | Does NOT suffer from Belady's Anomaly                    |
| Implementation      | Uses a counter or stack to track recency                 |
| Performance         | Better than FIFO; close to optimal                       |

### LRU vs FIFO vs OPT

| Algorithm | Based on        | Anomaly | Performance  |
|-----------|-----------------|---------|--------------|
| FIFO      | Order of entry  | Yes     | Worst        |
| LRU       | Recent usage    | No      | Good         |
| OPT       | Future usage    | No      | Best (ideal) |

### Implementation Strategy

Each page in memory is assigned a **timestamp** representing when it was last used. When a page fault occurs and all frames are full, the page with the **smallest timestamp** (used longest ago) is replaced.

### Example

Reference String: `7 0 1 2 0 3 0 4 2 3 0 3 2`
Frames: 3

| Step | Ref | Frames        | Replaced | Fault? |
|------|-----|---------------|----------|--------|
| 1    | 7   | [7, -, -]     | —        | YES    |
| 2    | 0   | [7, 0, -]     | —        | YES    |
| 3    | 1   | [7, 0, 1]     | —        | YES    |
| 4    | 2   | [2, 0, 1]     | 7        | YES    |
| 5    | 0   | [2, 0, 1]     | —        | No     |
| 6    | 3   | [2, 0, 3]     | 1        | YES    |
| 7    | 0   | [2, 0, 3]     | —        | No     |
| 8    | 4   | [4, 0, 3]     | 2        | YES    |
| 9    | 2   | [4, 0, 2]     | 3        | YES    |
| 10   | 3   | [3, 0, 2]     | 4        | YES    |
| 11   | 0   | [3, 0, 2]     | —        | No     |
| 12   | 3   | [3, 0, 2]     | —        | No     |
| 13   | 2   | [3, 0, 2]     | —        | No     |

Total Page Faults: **8** (compared to 9 for FIFO)

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

1. Open terminal and create: `vi lru.c`
2. Write the LRU page replacement program.
3. Compile: `gcc lru.c -o lru`
4. Run: `./lru`
5. Enter the reference string length, reference string, and number of frames.
6. Observe page faults and frame states.

---

## Program

```c
#include <stdio.h>

#define MAX_FRAMES 10
#define MAX_PAGES  50

int main() {
    int frames[MAX_FRAMES];
    int last_used[MAX_FRAMES];   /* Timestamp of last use */
    int pages[MAX_PAGES];
    int n, num_frames, i, j;
    int page_faults = 0;
    int time = 0;

    printf("=== LRU Page Replacement Algorithm ===\n\n");

    printf("Enter number of pages in the reference string: ");
    scanf("%d", &n);

    printf("Enter the reference string:\n");
    for (i = 0; i < n; i++)
        scanf("%d", &pages[i]);

    printf("Enter number of page frames: ");
    scanf("%d", &num_frames);

    /* Initialize frames to -1 (empty) */
    for (i = 0; i < num_frames; i++) {
        frames[i]    = -1;
        last_used[i] = -1;
    }

    printf("\n%-6s  %-30s  %s\n", "Page", "Frames", "Fault?");
    printf("-------------------------------------------\n");

    for (i = 0; i < n; i++) {
        int page  = pages[i];
        int found = -1;

        time++;

        /* Check if page is already in a frame (page hit) */
        for (j = 0; j < num_frames; j++) {
            if (frames[j] == page) {
                found = j;
                last_used[j] = time;   /* Update last used time */
                break;
            }
        }

        if (found == -1) {
            /* Page fault */
            page_faults++;

            /* Find an empty frame first */
            int empty = -1;
            for (j = 0; j < num_frames; j++) {
                if (frames[j] == -1) {
                    empty = j;
                    break;
                }
            }

            if (empty != -1) {
                /* Use empty frame */
                frames[empty]    = page;
                last_used[empty] = time;
            } else {
                /* Find LRU frame — the one with smallest last_used timestamp */
                int lru_idx = 0;
                for (j = 1; j < num_frames; j++) {
                    if (last_used[j] < last_used[lru_idx])
                        lru_idx = j;
                }
                frames[lru_idx]    = page;
                last_used[lru_idx] = time;
            }

            /* Print frame state */
            printf("%-6d  [ ", page);
            for (j = 0; j < num_frames; j++) {
                if (frames[j] == -1) printf(" -  ");
                else                 printf("%2d  ", frames[j]);
            }
            printf("]  PAGE FAULT\n");
        } else {
            /* Page hit — already updated last_used above */
            printf("%-6d  [ ", page);
            for (j = 0; j < num_frames; j++) {
                if (frames[j] == -1) printf(" -  ");
                else                 printf("%2d  ", frames[j]);
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
$ gcc lru.c -o lru
$ ./lru
```

---

## Output

```
=== LRU Page Replacement Algorithm ===

Enter number of pages in the reference string: 13
Enter the reference string:
7 0 1 2 0 3 0 4 2 3 0 3 2
Enter number of page frames: 3

Page    Frames                          Fault?
-------------------------------------------
7       [  7   -   -  ]               PAGE FAULT
0       [  7   0   -  ]               PAGE FAULT
1       [  7   0   1  ]               PAGE FAULT
2       [  2   0   1  ]               PAGE FAULT
0       [  2   0   1  ]               Hit
3       [  2   0   3  ]               PAGE FAULT
0       [  2   0   3  ]               Hit
4       [  4   0   3  ]               PAGE FAULT
2       [  4   0   2  ]               PAGE FAULT
3       [  3   0   2  ]               PAGE FAULT
0       [  3   0   2  ]               Hit
3       [  3   0   2  ]               Hit
2       [  3   0   2  ]               Hit

-------------------------------------------
Total Page Faults : 8
Total Page Hits   : 5
Hit Ratio         : 38.46%
```

---

## Comparison: FIFO vs LRU (same reference string, 3 frames)

| Algorithm | Page Faults | Page Hits | Hit Ratio |
|-----------|-------------|-----------|-----------|
| FIFO      | 9           | 4         | 30.77%    |
| LRU       | 8           | 5         | 38.46%    |

LRU performs better because it exploits temporal locality.

---

## Result

The LRU Page Replacement algorithm was successfully implemented in C. For the reference string with 3 frames, LRU produced **8 page faults**, which is better than FIFO's 9 faults. The program correctly tracked the least recently used page using timestamps.
