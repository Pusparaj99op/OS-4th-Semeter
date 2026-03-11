---
# OPERATING SYSTEM LAB
### Experiment No.: 08
---

| Field      | Details               |
|------------|-----------------------|
| Name       | Pranay K Gajbhiye     |
| Course     | B.Tech CSE            |
| Semester   | 4th Semester          |
| Subject    | Operating System Lab  |
| Experiment | 08                    |

---

## TITLE: Producer-Consumer Problem using Semaphores

---

## AIM

To simulate the **Producer-Consumer Problem** (Bounded Buffer Problem) using semaphores for process synchronization in Linux.

---

## THEORY

### Process Synchronization:
When multiple processes share resources (like memory, buffers, printers), they must coordinate their access to avoid **race conditions** and **data inconsistency**. This is called **process synchronization**.

### Critical Section Problem:
The **critical section** is a segment of code where a shared resource is accessed. The OS must ensure that:
1. **Mutual Exclusion** — Only one process is in the critical section at a time.
2. **Progress** — A process outside the critical section cannot block others.
3. **Bounded Waiting** — Every process eventually gets to enter the critical section.

### Semaphore:
A **semaphore** is an integer variable `S` accessed only via two atomic operations:
- **wait(S)** / `P(S)`: Decrement S; if S < 0, block the process.
- **signal(S)** / `V(S)`: Increment S; wake up a waiting process.

### Producer-Consumer Problem:
- A **Producer** produces data items and places them into a shared buffer.
- A **Consumer** consumes data items from the buffer.
- The buffer has a **finite capacity (N)**.

#### Semaphores Used:
| Semaphore | Initial Value | Purpose                              |
|-----------|---------------|--------------------------------------|
| `mutex`   | 1             | Mutual exclusion on buffer access    |
| `empty`   | N             | Count of empty slots in buffer       |
| `full`    | 0             | Count of filled slots in buffer      |

#### Pseudocode:
```
// PRODUCER
while (true) {
    produce item;
    wait(empty);   // Wait for empty slot
    wait(mutex);   // Enter critical section
    add item to buffer;
    signal(mutex); // Exit critical section
    signal(full);  // One more full slot
}

// CONSUMER
while (true) {
    wait(full);    // Wait for full slot
    wait(mutex);   // Enter critical section
    remove item from buffer;
    signal(mutex); // Exit critical section
    signal(empty); // One more empty slot
    consume item;
}
```

---

## APPARATUS / REQUIREMENTS

- **Hardware:** Personal Computer / Laptop
- **Operating System:** Ubuntu Linux
- **Compiler:** GCC with POSIX thread support (`-lpthread`)
- **Header Files:** `<semaphore.h>`, `<pthread.h>`, `<stdio.h>`
- **Editor:** VS Code / nano / vim

---

## PROCEDURE / PROGRAM

```c
// File: producer_consumer.c
// Author: Pranay K Gajbhiye
// Producer-Consumer Problem using POSIX Semaphores and pthreads

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>

#define BUFFER_SIZE  5
#define PRODUCE_COUNT 8
#define CONSUME_COUNT 8

int buffer[BUFFER_SIZE];
int in  = 0;   // Producer inserts at 'in'
int out = 0;   // Consumer removes from 'out'

sem_t mutex;
sem_t empty;
sem_t full;

void* producer(void* arg) {
    for (int i = 1; i <= PRODUCE_COUNT; i++) {
        int item = i * 10;  // Produce item

        sem_wait(&empty);   // Decrement empty count
        sem_wait(&mutex);   // Enter critical section

        buffer[in] = item;
        printf("[PRODUCER] Produced item: %d  at buffer[%d]\n", item, in);
        in = (in + 1) % BUFFER_SIZE;

        sem_post(&mutex);   // Exit critical section
        sem_post(&full);    // Increment full count

        sleep(1);
    }
    return NULL;
}

void* consumer(void* arg) {
    for (int i = 1; i <= CONSUME_COUNT; i++) {
        sem_wait(&full);    // Decrement full count
        sem_wait(&mutex);   // Enter critical section

        int item = buffer[out];
        printf("[CONSUMER] Consumed item: %d from buffer[%d]\n", item, out);
        out = (out + 1) % BUFFER_SIZE;

        sem_post(&mutex);   // Exit critical section
        sem_post(&empty);   // Increment empty count

        sleep(2);
    }
    return NULL;
}

int main() {
    pthread_t prod_thread, cons_thread;

    // Initialize semaphores
    sem_init(&mutex, 0, 1);
    sem_init(&empty, 0, BUFFER_SIZE);
    sem_init(&full,  0, 0);

    printf("=== Producer-Consumer Problem ===\n");
    printf("Buffer Size: %d\n\n", BUFFER_SIZE);

    // Create Producer and Consumer threads
    pthread_create(&prod_thread, NULL, producer, NULL);
    pthread_create(&cons_thread, NULL, consumer, NULL);

    // Wait for both threads to complete
    pthread_join(prod_thread, NULL);
    pthread_join(cons_thread, NULL);

    // Destroy semaphores
    sem_destroy(&mutex);
    sem_destroy(&empty);
    sem_destroy(&full);

    printf("\n=== Simulation Complete ===\n");
    return 0;
}
```

**Compile & Run:**
```bash
gcc producer_consumer.c -o producer_consumer -lpthread
./producer_consumer
```

---

## OUTPUT / OBSERVATIONS

```
=== Producer-Consumer Problem ===
Buffer Size: 5

[PRODUCER] Produced item: 10  at buffer[0]
[CONSUMER] Consumed item: 10 from buffer[0]
[PRODUCER] Produced item: 20  at buffer[1]
[PRODUCER] Produced item: 30  at buffer[2]
[CONSUMER] Consumed item: 20 from buffer[1]
[PRODUCER] Produced item: 40  at buffer[3]
[PRODUCER] Produced item: 50  at buffer[4]
[CONSUMER] Consumed item: 30 from buffer[2]
[PRODUCER] Produced item: 60  at buffer[0]
[CONSUMER] Consumed item: 40 from buffer[3]
[PRODUCER] Produced item: 70  at buffer[1]
[PRODUCER] Produced item: 80  at buffer[2]
[CONSUMER] Consumed item: 50 from buffer[4]
[CONSUMER] Consumed item: 60 from buffer[0]
[CONSUMER] Consumed item: 70 from buffer[1]
[CONSUMER] Consumed item: 80 from buffer[2]

=== Simulation Complete ===
```

---

## RESULT

The Producer-Consumer problem was successfully implemented using POSIX semaphores and pthreads in C. The semaphores `mutex`, `empty`, and `full` ensured:
1. **Mutual exclusion** — Only one thread accessed the buffer at a time.
2. **Synchronization** — Producer waited when the buffer was full; consumer waited when it was empty.
3. **No race conditions** — Data integrity was maintained throughout the simulation.

---

*Name: Pranay K Gajbhiye | B.Tech CSE | 4th Semester | Experiment 08*
