---
layout: default
title: Week 2 - Processes
nav_order: 3
---

## CSDS 338 Processes and Calls
---

## Contents
1. [Introduction to Processes](#introduction-to-processes)
2. [Key Components of a Process](#key-components-of-a-process)
   - [Program Code (Text Segment)](#1-program-code-text-segment)
   - [Data Segment](#2-data-segment)
   - [Stack](#3-stack)
   - [Heap](#4-heap)
3. [Process Control Block (PCB)](#process-control-block-pcb)
4. [Process Lifecycle and States](#process-lifecycle-and-states)
   - [New State](#1-new-state)
   - [Ready State](#2-ready-state)
   - [Running State](#3-running-state)
   - [Waiting (Blocked) State](#4-waiting-blocked-state)
   - [Terminated (Zombie) State](#5-terminated-zombie-state)
5. [Understanding Process States in Real Time](#understanding-process-states-in-real-time)
6. [Waiting Processes and CPU Utilization](#waiting-processes-and-cpu-utilization)
7. [Foreground vs. Background Processes](#foreground-vs-background-processes)
   - [Foreground Processes](#foreground-processes)
   - [Background Processes](#background-processes)
8. [Essential Linux Commands for Process Management](#essential-linux-commands-for-process-management)
9. [Context Switching](#context-switching)
10. [Summary and Best Practices](#summary-and-best-practices)
## **Introduction to Processes**  

A **process** is an active instance of a program running on a computer. It represents the execution of code, from the time it is loaded into memory until it completes. A process can be thought of as the key element of multitasking in operating systems, where the CPU switches between processes, allowing multiple programs to run seemingly at the same time.  

### **Why Processes Matter**  
Processes are fundamental to how operating systems manage tasks and allocate resources. Whether you are opening a web browser, compiling code, or running background scripts, each task is a process that the OS manages to ensure efficient execution.  

A **program** is simply a set of instructions stored on disk. When executed, the operating system creates a process, allocating necessary memory and resources to run the program. This transformation from program to process is managed by the **process scheduler**.  

---

## **Key Components of a Process**  

A process consists of different segments that hold code, data, and temporary runtime information. These components help the process maintain its state during execution, allowing it to perform complex tasks and manage data effectively.  

### 1. **Program Code (Text Segment)**  
This section contains the actual machine code instructions that the CPU executes. The text segment is usually **read-only** to prevent accidental modification during runtime.  

### 2. **Data Segment**  
The data segment stores global and static variables. This can be further divided into:  
- **Initialized Data** – Variables explicitly initialized by the program.  
- **Uninitialized Data (BSS Segment)** – Variables that are declared but not initialized.  

### 3. **Stack**  
The stack is a dynamic area of memory that grows and shrinks as functions are called and return. It stores:  
- Local variables within functions.  
- Function parameters.  
- Return addresses, ensuring the program knows where to continue after a function completes.  

### 4. **Heap**  
The heap is a region for **dynamic memory allocation** during the process's lifetime. When a program requests memory at runtime (e.g., `malloc` in C), it is allocated from the heap. Unlike the stack, the heap requires manual memory management, meaning the developer must free memory when it’s no longer needed.  

---

## **Process Control Block (PCB)**  

When a process is created, the operating system generates a **Process Control Block (PCB)** – a data structure that stores all the information the OS needs to manage and track the process.  

The PCB holds:  
- **Process ID (PID):** A unique identifier assigned to the process.  
- **Program Counter:** The address of the next instruction to execute.  
- **CPU Registers:** The current state of the CPU registers, saved during context switches.  
- **Process State:** The current state of the process (new, ready, running, waiting, or terminated).  
- **Priority:** Determines the order of execution for processes. Higher priority processes may preempt lower-priority ones.  
- **Memory Pointers:** Tracks the location of the process’s stack, heap, and data segments in memory.  
- **I/O Status:** Information about files, network connections, and devices associated with the process.  

---

## **Process Lifecycle and States**  

A process transitions through multiple states during its lifecycle. The operating system's scheduler is responsible for moving processes between states to ensure efficient CPU usage and fair resource allocation.  

### 1. **New State**  
When a program is launched, the process enters the **New** state. At this point, the OS prepares resources by allocating memory, initializing the PCB, and loading the program’s code into RAM.  

### 2. **Ready State**  
After initialization, the process enters the **Ready** state. It is waiting in the ready queue for CPU time. The scheduler selects from the ready queue based on priority, fairness, or time-sharing policies.  

### 3. **Running State**  
A process in the **Running** state is actively executing on the CPU. Only one process per core can be in this state at any given time.  

### 4. **Waiting (Blocked) State**  
Processes often encounter situations where they need to wait for external resources, such as I/O operations or memory allocation. While waiting, the process is moved to the **Waiting** state to free up the CPU for other processes.  

- **Interruptible Sleep (S):** The process can return to the ready state when the waiting event is completed.  
- **Uninterruptible Sleep (D):** The process cannot be interrupted until the resource is fully available.  

### 5. **Terminated (Zombie) State**  
When a process completes, it moves to the **Terminated** state. However, its entry may linger in the process table (as a zombie process) until the parent process retrieves the exit code. The OS eventually cleans up zombie processes to free system resources.  

---

## **Understanding Process States in Real Time**  

In Linux, you can monitor process states using commands like `top` and `ps`. These tools provide a live view of system activity and resource utilization.  

- **R (Running):** The process is actively using the CPU.  
- **S (Sleeping):** The process is waiting for input/output or another resource.  
- **D (Uninterruptible Sleep):** The process is waiting for non-interruptible I/O (disk operations).  
- **Z (Zombie):** The process has completed execution but has not been fully cleaned up.  

Example:  
```bash
top
```  
This displays running processes and their resource usage.  

---

## **Waiting Processes and CPU Utilization**  

When a process performs I/O operations or waits for user input, the CPU cannot execute it. Instead of wasting CPU cycles, the OS switches to another ready process. This mechanism maximizes CPU efficiency by keeping the processor busy at all times.  

### **Example – Disk I/O:**  
Imagine a process reading a large file from disk. While the disk fetches the data, the OS temporarily suspends the process, allowing another task to use the CPU.  

---

## **Foreground vs. Background Processes**  

### **Foreground Processes**  
Foreground processes execute in the terminal and take over user input until completed. For example:  
```bash
gcc program.c
```  
In this case, the terminal remains occupied until compilation finishes.  

### **Background Processes**  
Background processes run independently without blocking the terminal. You can start a process in the background by appending `&`:  
```bash
./script.sh &
```  
This frees the terminal, allowing users to execute other commands while the script runs in the background.  

- Use **Ctrl + Z** to pause a foreground process.  
- Resume a paused process in the background by typing `bg`.  
- Bring it back to the foreground using `fg`.  

---

## **Essential Linux Commands for Process Management**  

1. **ps aux** – Displays all active processes, including user information, CPU/memory usage, and more.  
2. **top** – Provides real-time process monitoring, showing CPU usage, memory, and priorities.  
3. **kill <PID>** – Terminates a process by its PID.  
4. **kill -9 <PID>** – Forcefully kills an unresponsive process.  
5. **nice -n 10 ./task** – Starts a process with a lower priority.  
6. **renice -n -5 -p <PID>** – Adjusts the priority of an already running process.  

---

## **Context Switching**  

**Context switching** allows the OS to multitask by switching between processes. The OS saves the current process’s state in the PCB and loads another process from the ready queue.  

Steps:  
1. Save the current process’s state.  
2. Load the next process’s state from the PCB.  
3. Resume execution.  

Though context switching introduces overhead, it is essential for maintaining system responsiveness.  

---

## **Summary and Best Practices**  

- Processes are the foundation of multitasking in operating systems.  
- Effective process management improves system efficiency and responsiveness.  
- Regularly monitor process activity using `ps`, `top`, and `kill` commands.
