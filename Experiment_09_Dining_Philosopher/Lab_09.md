---
# OPERATING SYSTEM LAB
### Experiment No.: 09
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 09                    |

---

## TITLE: Dining Philosopher Problem

---

## AIM

To simulate the **Dining Philosopher Problem** as a classical synchronization problem, and implement a deadlock-free solution using semaphores/mutexes.

---

## THEORY

### Dining Philosopher Problem:
Proposed by **Edsger Dijkstra** in 1965, it is a classic example to illustrate synchronization problems and solutions.

### Problem Description:
- **N philosophers** sit around a circular dining table.
- Each philosopher has a **plate of food** and needs **two forks** (one on the left, one on the right) to eat.
- A philosopher alternates between **thinking** and **eating**.
- A philosopher picks up the **left fork**, then the **right fork** to eat.
- After eating, they put both forks down and resume thinking.

### The Problem:
If all N philosophers simultaneously pick up their left fork, all will be waiting for the right fork — causing **DEADLOCK**.

```
         Philosopher 1
        /              \
 Fork 5                Fork 1
     |                    |
Phil 5                 Phil 2
     |                    |
 Fork 4                Fork 2
        \              /
         Philosopher 4
              |
           Fork 3
              |
         Philosopher 3
```

### Deadlock Conditions in This Problem:
All four Coffman conditions are met when philosophers simultaneously hold one fork each.

### Solution (Asymmetric Approach):
- **Odd-numbered philosophers** pick up the **left fork first**.
- **Even-numbered philosophers** pick up the **right fork first**.
- This breaks the circular wait condition and prevents deadlock.

### Alternative Solution:
Allow at most **(N-1)** philosophers to eat simultaneously — using a semaphore initialized to N-1.

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux
- **Compiler:** GCC with POSIX thread support (`-lpthread`)
- **Header Files:** `<pthread.h>`, `<semaphore.h>`, `<stdio.h>`
- **Editor:** VS Code / nano / vim

---

## PROCEDURE / PROGRAM

```c
// File: dining_philosopher.c
// Author: Pranay K Gajbhiye
// Dining Philosopher Problem — Deadlock-free using Asymmetric Solution

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define N 5  // Number of philosophers

sem_t  fork_sem[N];   // One semaphore per fork
sem_t  room;          // Allows at most N-1 philosophers to sit (prevents deadlock)

void think(int id) {
    printf("Philosopher %d is THINKING...\n", id);
    sleep(rand() % 3 + 1);
}

void eat(int id) {
    printf("Philosopher %d is EATING.\n", id);
    sleep(rand() % 2 + 1);
    printf("Philosopher %d finished eating.\n", id);
}

void* philosopher(void* arg) {
    int id = *(int*)arg;

    for (int i = 0; i < 3; i++) {  // Each philosopher eats 3 times
        think(id);

        // Allow only N-1 philosophers in the "dining room" at once
        sem_wait(&room);

        // Asymmetric: odd picks left first, even picks right first
        if (id % 2 == 0) {
            sem_wait(&fork_sem[id]);
            printf("  Philosopher %d picked up LEFT  fork %d\n", id, id);
            sem_wait(&fork_sem[(id + 1) % N]);
            printf("  Philosopher %d picked up RIGHT fork %d\n", id, (id+1)%N);
        } else {
            sem_wait(&fork_sem[(id + 1) % N]);
            printf("  Philosopher %d picked up RIGHT fork %d\n", id, (id+1)%N);
            sem_wait(&fork_sem[id]);
            printf("  Philosopher %d picked up LEFT  fork %d\n", id, id);
        }

        eat(id);

        // Put down forks
        sem_post(&fork_sem[id]);
        sem_post(&fork_sem[(id + 1) % N]);
        printf("  Philosopher %d put down both forks.\n", id);

        sem_post(&room);  // Leave dining room
    }

    printf("Philosopher %d is DONE.\n", id);
    return NULL;
}

int main() {
    pthread_t tid[N];
    int ids[N];

    printf("=== Dining Philosopher Problem ===\n");
    printf("Number of Philosophers: %d\n\n", N);

    // Initialize semaphores
    for (int i = 0; i < N; i++) {
        sem_init(&fork_sem[i], 0, 1);  // Each fork is initially free
        ids[i] = i;
    }
    sem_init(&room, 0, N - 1);  // At most N-1 allowed at once

    // Create philosopher threads
    for (int i = 0; i < N; i++)
        pthread_create(&tid[i], NULL, philosopher, &ids[i]);

    // Wait for all threads to finish
    for (int i = 0; i < N; i++)
        pthread_join(tid[i], NULL);

    // Cleanup
    for (int i = 0; i < N; i++)
        sem_destroy(&fork_sem[i]);
    sem_destroy(&room);

    printf("\n=== All Philosophers Done ===\n");
    return 0;
}
```

**Compile & Run:**
```bash
gcc dining_philosopher.c -o dining_philosopher -lpthread
./dining_philosopher
```

---

## OUTPUT / OBSERVATIONS

```
=== Dining Philosopher Problem ===
Number of Philosophers: 5

Philosopher 0 is THINKING...
Philosopher 1 is THINKING...
Philosopher 2 is THINKING...
Philosopher 3 is THINKING...
Philosopher 4 is THINKING...
  Philosopher 1 picked up RIGHT fork 2
  Philosopher 1 picked up LEFT  fork 1
Philosopher 1 is EATING.
  Philosopher 3 picked up RIGHT fork 4
  Philosopher 3 picked up LEFT  fork 3
Philosopher 3 is EATING.
Philosopher 1 finished eating.
  Philosopher 1 put down both forks.
Philosopher 1 is THINKING...
Philosopher 3 finished eating.
  Philosopher 3 put down both forks.
Philosopher 3 is THINKING...
  Philosopher 0 picked up LEFT  fork 0
  Philosopher 0 picked up RIGHT fork 1
Philosopher 0 is EATING.
...
=== All Philosophers Done ===
```

---

## RESULT

The Dining Philosopher Problem was successfully implemented and simulated using POSIX semaphores and pthreads. The **room semaphore** (initialized to N-1) and **asymmetric fork-picking** strategy effectively prevented deadlock. All 5 philosophers successfully alternated between thinking and eating without any indefinite blocking, demonstrating a correct and deadlock-free solution.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 09*
