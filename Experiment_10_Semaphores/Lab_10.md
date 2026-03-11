---
# OPERATING SYSTEM LAB
### Experiment No.: 10
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 10                    |

---

## TITLE: Semaphores — Reader-Writer Problem

---

## AIM

To implement the **Reader-Writer Problem** using semaphores to demonstrate process synchronization, ensuring multiple readers can read simultaneously but only one writer can write at a time.

---

## THEORY

### Semaphore:
A **semaphore** is an integer synchronization variable maintained by the OS, accessed only via two atomic operations:

| Operation          | Effect                                           |
|--------------------|--------------------------------------------------|
| `wait(S)` / `P(S)` | Decrement S; if S < 0, block the calling process |
| `signal(S)` / `V(S)` | Increment S; wake up a blocked process         |

### Types of Semaphores:

| Type              | Description                                                     |
|-------------------|-----------------------------------------------------------------|
| **Binary Semaphore** | Value is always 0 or 1. Acts like a mutex (mutual exclusion lock). |
| **Counting Semaphore** | Value ranges over an unrestricted domain. Used to control access to a pool of resources. |

### Reader-Writer Problem:
A classic synchronization problem where:
- **Multiple readers** can read a shared resource simultaneously (no conflict).
- **Only one writer** can write at a time (exclusive access needed).
- **No reader** can read while a writer is writing.

### Roles:
| Role   | Constraint                                              |
|--------|---------------------------------------------------------|
| Reader | Multiple readers can read concurrently                  |
| Writer | Exclusive access — no readers or other writers allowed  |

### Semaphores Used:
| Semaphore    | Initial Value | Purpose                                     |
|--------------|---------------|---------------------------------------------|
| `mutex`      | 1             | Protect `read_count` variable               |
| `rw_mutex`   | 1             | Exclusive access for writer (and 1st/last reader) |

### Variables:
- `read_count`: Number of readers currently reading.

### Pseudocode:
```
// READER
wait(mutex);
  read_count++;
  if (read_count == 1)
    wait(rw_mutex);  // First reader locks out writers
signal(mutex);

// -- Read the shared data --

wait(mutex);
  read_count--;
  if (read_count == 0)
    signal(rw_mutex);  // Last reader unlocks for writers
signal(mutex);

// WRITER
wait(rw_mutex);
  // -- Write the shared data --
signal(rw_mutex);
```

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
// File: reader_writer.c
// Author: Pranay K Gajbhiye
// Reader-Writer Problem using POSIX Semaphores

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define NUM_READERS 4
#define NUM_WRITERS 2

sem_t  mutex;       // Protects read_count
sem_t  rw_mutex;    // Exclusive write lock

int    read_count = 0;
int    shared_data = 0;   // Shared resource

void* reader(void* arg) {
    int id = *(int*)arg;

    for (int i = 0; i < 3; i++) {
        sleep(rand() % 2);

        // Entry section
        sem_wait(&mutex);
        read_count++;
        if (read_count == 1)
            sem_wait(&rw_mutex);  // First reader blocks writers
        sem_post(&mutex);

        // Reading critical section
        printf("[READER %d] Reading shared_data = %d  (readers active: %d)\n",
               id, shared_data, read_count);
        sleep(1);

        // Exit section
        sem_wait(&mutex);
        read_count--;
        if (read_count == 0)
            sem_post(&rw_mutex);  // Last reader unblocks writers
        sem_post(&mutex);

        printf("[READER %d] Done reading.\n", id);
    }
    return NULL;
}

void* writer(void* arg) {
    int id = *(int*)arg;

    for (int i = 0; i < 2; i++) {
        sleep(rand() % 3 + 1);

        // Entry section — exclusive access
        sem_wait(&rw_mutex);

        // Writing critical section
        shared_data++;
        printf("[WRITER %d] Writing shared_data = %d\n", id, shared_data);
        sleep(1);

        // Exit section
        sem_post(&rw_mutex);

        printf("[WRITER %d] Done writing.\n", id);
    }
    return NULL;
}

int main() {
    pthread_t reader_tid[NUM_READERS];
    pthread_t writer_tid[NUM_WRITERS];
    int reader_ids[NUM_READERS];
    int writer_ids[NUM_WRITERS];

    sem_init(&mutex,    0, 1);
    sem_init(&rw_mutex, 0, 1);

    printf("=== Reader-Writer Problem ===\n");
    printf("Readers: %d  |  Writers: %d\n\n", NUM_READERS, NUM_WRITERS);

    for (int i = 0; i < NUM_READERS; i++) {
        reader_ids[i] = i + 1;
        pthread_create(&reader_tid[i], NULL, reader, &reader_ids[i]);
    }

    for (int i = 0; i < NUM_WRITERS; i++) {
        writer_ids[i] = i + 1;
        pthread_create(&writer_tid[i], NULL, writer, &writer_ids[i]);
    }

    for (int i = 0; i < NUM_READERS; i++)
        pthread_join(reader_tid[i], NULL);

    for (int i = 0; i < NUM_WRITERS; i++)
        pthread_join(writer_tid[i], NULL);

    sem_destroy(&mutex);
    sem_destroy(&rw_mutex);

    printf("\n=== Simulation Complete. Final shared_data = %d ===\n", shared_data);
    return 0;
}
```

**Compile & Run:**
```bash
gcc reader_writer.c -o reader_writer -lpthread
./reader_writer
```

---

## OUTPUT / OBSERVATIONS

```
=== Reader-Writer Problem ===
Readers: 4  |  Writers: 2

[READER 1] Reading shared_data = 0  (readers active: 1)
[READER 2] Reading shared_data = 0  (readers active: 2)
[READER 3] Reading shared_data = 0  (readers active: 3)
[READER 1] Done reading.
[READER 2] Done reading.
[READER 3] Done reading.
[WRITER 1] Writing shared_data = 1
[WRITER 1] Done writing.
[READER 4] Reading shared_data = 1  (readers active: 1)
[READER 1] Reading shared_data = 1  (readers active: 2)
[READER 4] Done reading.
[READER 1] Done reading.
[WRITER 2] Writing shared_data = 2
[WRITER 2] Done writing.
...
=== Simulation Complete. Final shared_data = 4 ===
```

---

### Comparison: Binary vs. Counting Semaphore

| Feature            | Binary Semaphore       | Counting Semaphore          |
|--------------------|------------------------|-----------------------------|
| Value Range        | 0 or 1                 | 0 to N                      |
| Use Case           | Mutual Exclusion (mutex)| Resource pool management   |
| Example            | `mutex` in this program| `empty/full` in Prod-Cons   |

---

## RESULT

The Reader-Writer problem was successfully implemented using POSIX semaphores and pthreads. The solution correctly enforced:
1. **Shared Read Access** — Multiple readers read simultaneously without blocking each other.
2. **Exclusive Write Access** — Writers obtained exclusive access, blocking all other readers and writers.
3. **No Data Corruption** — The shared data was modified safely without race conditions.

The experiment demonstrated the practical use of **binary semaphores** for mutual exclusion and synchronization.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 10*
