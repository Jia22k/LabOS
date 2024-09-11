---
layout: default
title: Week 3.2 - Intro to some concepts
nav_order: 5
---


### A brief Intro 

> This is a brief introduction to help prepare you for Thursday's class, which I find a bit complex. To make the transition smoother, we'll start with an overview of the key concepts.---

#### **1. Preemption: Involuntary Context Switch**
- **What is Preemption?**
  - Preemption occurs when the operating system interrupts a running process to schedule another one, typically to ensure responsiveness or fairness in a multitasking system. 
  - It’s an **involuntary context switch**, meaning the process doesn’t give up the CPU voluntarily; instead, the OS forces it to pause, saving its current state and switching to a more urgent or higher-priority process.

- **Why is Preemption Important?**
  - Preemption allows the OS to quickly respond to high-priority or time-sensitive tasks, improving system responsiveness.
  - Without preemption, long-running processes could hog the CPU, slowing down critical tasks.
  
  **Example:** If you're using your computer and a background process is running, preemption ensures that your interaction with the system (like typing or opening apps) doesn’t become sluggish.

---

#### **2. CPU Affinity: Binding Processes to Specific CPUs**
- **What is CPU Affinity?**
  - **CPU Affinity** refers to the ability to bind a process or thread to a specific CPU core or set of cores. This can improve performance by keeping a process on the same core, which can reduce **cache misses** and improve **cache locality**.
  
- **CPU Mask:**
  - A **CPU Mask** is a bitmask that defines which CPUs a process can run on. For example, if you have a 4-core CPU, the mask can specify that a particular process can only run on core 1 and core 3.

- **Why Use CPU Affinity?**
  - Keeping a process on the same CPU can help maintain **cache locality**, meaning that data the CPU has already fetched remains in the cache, improving performance.

  **Example:** If you're running a heavy computation process, binding it to a specific core might keep it from jumping between CPUs, reducing cache reloading times.

---

#### **3. Migration: Moving Processes Between CPUs**
- **What is Migration?**
  - **Migration** refers to the movement of processes from one CPU core to another, typically done by the operating system's scheduler to balance the load across all CPUs.
  
- **Why is Migration Used?**
  - If one CPU core becomes overloaded with too many tasks, the OS can migrate some tasks to another core to spread the load, ensuring that no single CPU is overburdened.
  
- **Impact on Cache Performance:**
  - While migration can help balance CPU usage, it may also negatively affect **cache performance**. When a process is moved, the new CPU might not have the data cached, leading to slower performance due to cache reloading.

  **Example:** Imagine a multi-core server where certain cores are overloaded. The OS might migrate some tasks to less busy cores to balance the workload.

---

#### **4. Fork vs. `pthread_create`: Process vs. Thread Creation**
- **What is `fork()`?**
  - **`fork()`** is a system call that creates a new process. The new process (called the child) is an exact copy of the parent process, but it has its own separate memory space.
  - **Resources:** After a fork, the child process shares resources like file descriptors with the parent, but each process has its own **stack** and **heap**.
  
- **What is `pthread_create()`?**
  - **`pthread_create()`** is used to create a new thread within an existing process. Threads share the same memory space as the parent process (the heap), but each thread has its own **stack**.
  
- **Key Differences:**
  - **Processes (created by `fork()`)** are heavier than threads and have separate memory spaces. Processes are good for running completely independent tasks.
  - **Threads (created by `pthread_create()`)** are lighter-weight and share memory, which allows them to communicate more easily but requires careful management to avoid conflicts (e.g., race conditions).

  **Example:** In a web server, you might use `fork()` to create independent processes for handling client requests, or `pthread_create()` to create lightweight threads that share memory and work together on tasks.

---

#### **5. Load Balancing: Distributing Workloads Across CPUs**
- **What is Load Balancing?**
  - **Load balancing** ensures that no single CPU core is overburdened while others remain idle. The OS dynamically distributes tasks across all available cores, preventing bottlenecks and optimizing system performance.

- **Why is it Important?**
  - Without load balancing, some CPU cores might become overworked, leading to slower overall system performance. By distributing tasks more evenly, the system runs more efficiently.
  
  **Example:** Imagine a system with 8 CPU cores. If 7 are idle but one is overloaded with tasks, the system is not being used efficiently. Load balancing helps distribute those tasks to idle cores.

---

#### **6. Demonstrating Affinity and Migration (Linux Demo)**
- **How Can You Control CPU Affinity?**
  - In Linux, tools like `taskset` allow you to set CPU affinity for a process. This means you can bind a process to run only on specific CPUs, allowing you to see how this impacts performance.
  
- **How Does Migration Work?**
  - You can observe how the operating system migrates tasks between CPUs in real time using system monitoring tools like `htop`. Migration helps balance the CPU load but may have a trade-off with cache performance.

  **Example:** Run a computationally intensive task and observe how setting CPU affinity keeps it locked to a specific CPU, while without affinity, it might migrate across cores.

---

### **Summary of Key Concepts:**
- **Preemption** ensures high-priority tasks get CPU time, interrupting running processes.
- **CPU Affinity** binds processes to specific CPU cores to improve cache performance.
- **Migration** helps balance CPU loads but may affect cache performance.
- **`fork()`** creates separate processes with their own memory spaces, while **`pthread_create()`** creates threads that share memory.
- **Load Balancing** ensures all CPU cores are used efficiently, preventing bottlenecks.

---