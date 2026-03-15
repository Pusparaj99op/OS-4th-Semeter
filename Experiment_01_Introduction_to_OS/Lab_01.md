# Experiment 01 — Introduction to Operating System

| Field      | Details                 |
|------------|-------------------------|
| **Name**   | Pranay K Gajbhiye       |
| **Course** | B.Tech Computer Science |
| **Sem**    | 4th Semester            |
| **Sub**    | Operating System Lab    |

---

## Aim

To study and understand the fundamental concepts of an Operating System, its types, functions, and structure.

---

## Theory

### What is an Operating System?

An **Operating System (OS)** is system software that acts as an interface between the computer hardware and the user. It manages hardware resources and provides an environment in which application programs can run.

### Functions of an Operating System

| Function                   | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| Process Management         | Creates, schedules, and terminates processes; handles inter-process communication |
| Memory Management          | Allocates and deallocates memory; manages virtual memory                    |
| File System Management     | Organizes files on storage devices; controls read/write access              |
| Device Management          | Manages I/O devices via device drivers                                      |
| Security & Protection      | Prevents unauthorized access to resources                                   |
| User Interface             | Provides CLI (Command Line Interface) or GUI (Graphical User Interface)     |
| Error Detection & Handling | Detects hardware errors and takes corrective action                         |

### Types of Operating Systems

1. **Batch Operating System** — Jobs are collected, grouped into batches, and processed without user interaction. Example: Early IBM systems.

2. **Time-Sharing Operating System** — Multiple users share CPU time in small time slices (quanta). Example: UNIX.

3. **Distributed Operating System** — Multiple computers connected over a network share resources and work together. Example: LOCUS.

4. **Embedded Operating System** — Designed for embedded devices with limited resources. Example: FreeRTOS, VxWorks.

5. **Real-Time Operating System (RTOS)** — Processes tasks within strict time constraints. Two sub-types:
   - **Hard RTOS**: Missing a deadline results in system failure (e.g., aircraft control).
   - **Soft RTOS**: Missing a deadline is undesirable but not catastrophic (e.g., video streaming).

6. **Multiprocessor Operating System** — Manages multiple CPUs that share memory and other resources. Example: Windows Server.

7. **Network Operating System** — Provides services to computers connected in a network. Example: Novell NetWare.

### OS Architecture

```
+---------------------------------------------+
|           User Applications                 |
+---------------------------------------------+
|         System Utilities / Shell            |
+---------------------------------------------+
|         Operating System Kernel             |
|  (Process, Memory, I/O, File Management)   |
+---------------------------------------------+
|            Hardware Layer                   |
|   (CPU, RAM, HDD, I/O Devices)             |
+---------------------------------------------+
```

### Kernel

The **kernel** is the core of the OS that runs in privileged mode and has direct access to hardware. It is always resident in memory. Types of kernels:

- **Monolithic Kernel**: All OS services run in kernel space. Fast but large. Example: Linux.
- **Microkernel**: Minimal kernel; most services run in user space. Example: Minix, QNX.
- **Hybrid Kernel**: Combination of monolithic and microkernel. Example: Windows NT, macOS.

### Key Concepts

| Concept        | Definition                                                                 |
|----------------|----------------------------------------------------------------------------|
| Process        | A program in execution; an active entity                                   |
| Thread         | Smallest unit of CPU execution; a lightweight process                      |
| System Call    | Interface through which a user program requests OS services                |
| Virtual Memory | Technique that allows processes to use more memory than physically available |
| Deadlock       | A state where two or more processes wait indefinitely for each other's resources |

---

## Apparatus / Requirements

| Component       | Specification                          |
|-----------------|----------------------------------------|
| Computer System | Any PC with minimum 2 GB RAM          |
| Operating System| Linux (Ubuntu 20.04 or later) / Windows |
| Software        | Terminal / Command Prompt              |

---

## Procedure

1. Study the definition and purpose of an Operating System.
2. Understand the layered architecture of a modern OS (User → Shell → Kernel → Hardware).
3. Identify the different types of OS and their real-world application domains.
4. Study the key functions: process management, memory management, file management, device management.
5. Review key concepts: processes, threads, system calls, virtual memory, deadlock.
6. Open a Linux terminal and observe OS interaction via basic commands.
7. Document observations and draw conclusions.

---

## Observation

After studying OS fundamentals, the following observations were made:

- The OS acts as a **resource manager** and **extended machine** (hiding hardware complexity from users).
- The **kernel** is the critical component running in privileged mode.
- Different OS types are suited for different environments (real-time, batch, distributed, etc.).
- Every user interaction ultimately involves **system calls** that translate to hardware instructions.

---

## Result

The fundamental concepts of an Operating System — its definition, architecture, types, functions, and key terminology — were studied and understood successfully.
