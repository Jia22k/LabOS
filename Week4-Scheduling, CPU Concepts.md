---
layout: default
title: Week 4 - Scheduling, CPU Concepts
nav_order: 4
---


### Process Affinity (CPU Affinity)

**Definition**: Process affinity, also known as CPU affinity, is the practice of binding a process or thread to specific CPU core(s), ensuring that it runs on the designated core(s) when scheduled.

#### Key Concept:
- **CPU Mask**: A bitmask that specifies which cores a process can execute on. Each bit in the mask corresponds to a CPU core; if the bit is set to 1, the core is eligible for running the process.

#### Types of Affinity:
- **Soft Affinity**: 
   - The OS attempts to keep the process on the same core but can migrate it if necessary.
- **Hard Affinity**:
   - The process is strictly bound to specific CPU cores and will not migrate to other cores under any circumstances.

#### Advantages of Process Affinity:
- **Reduced Context Switching**:
   - Ensures that processes are not frequently moved between cores, minimizing the performance hit from switching.
- **Improved Cache Locality**:
   - When processes remain on the same core, they retain cached data, minimizing cache misses and enhancing performance.

#### Linux Example:
- Linux allows developers to set CPU affinity for processes or threads using the `sched_setaffinity()` system call, giving control over where processes execute in a multicore system.

---

### Process Migration

**Definition**: Process migration is the act of moving a process from one CPU core to another by the operating system’s scheduler to optimize performance and resource utilization.

#### Key Reasons for Migration:
- **Load Balancing**: 
   - Distributes processes across multiple cores to prevent overload on a single core.
- **Improved Resource Utilization**:
   - Redistributes tasks to ensure all cores are efficiently used.

#### Potential Downsides:
- **Cache Invalidation**:
   - When a process is migrated, the new core may not have the cached data from the previous core, causing cache misses and requiring data to be reloaded.
- **Context Switching Overhead**:
   - Frequent migrations increase the cost of saving and restoring process state information.

#### When Migration is Helpful:
- **Cache Thrashing Reduction**:
   - Migrating a process that’s causing cache contention on one core can alleviate thrashing.
- **NUMA Systems**:
   - In systems with Non-Uniform Memory Access (NUMA), migration can optimize memory access patterns by moving a process closer to the memory it frequently accesses.

#### Linux Example:
- Linux uses the Completely Fair Scheduler (CFS) to decide when and where to migrate processes across cores for optimal performance.

---

### Fork vs Threads

#### Fork:
- **Definition**: A system call used to create a new process by duplicating the current (parent) process. The new process (child process) runs independently but shares the same memory space initially.

#### pthread_create (Threads):
- **Definition**: A function used to create a new thread within the same process, allowing concurrent execution of tasks. Threads share the same memory space but have independent execution paths.

#### Key Differences:
- **Memory Usage**:
   - Forked processes have separate memory spaces (with some shared resources), while threads share the same memory space, including heap and global variables.
- **Performance**:
   - Threads are lighter and faster for communication within the same process, while forked processes are more isolated but incur more overhead.

#### Advantages of Fork:
- **Isolation**: 
   - Child processes are independent and do not affect the parent process’s memory directly.
- **Security**:
   - Forked processes can be terminated without disrupting the parent.

#### Advantages of Threads:
- **Shared Memory**: 
   - Easier and faster communication between threads since they share the same memory space.
- **Lower Overhead**: 
   - Threads are more efficient in terms of resource usage compared to processes created by fork.

---

### Preemption

**Definition**: Preemption is the act of forcibly interrupting a running process by the operating system’s scheduler to allocate CPU time to another process.

#### How it Works:
- The scheduler decides which process should run next based on priority or scheduling algorithms.
- The interrupted process has its state saved, allowing it to resume later without losing progress.

#### Key Concept:
- **Involuntary Context Switch**:
   - The process does not voluntarily yield control of the CPU. Instead, the OS forces it to stop to give other processes a chance to execute.

#### Use Cases:
- **Multitasking**:
   - Ensures multiple processes can run seemingly simultaneously by switching between them rapidly.
- **Real-Time Systems**:
   - High-priority tasks can preempt lower-priority ones to meet time-sensitive requirements.

#### Advantages:
- **Fair CPU Allocation**:
   - Prevents a single process from monopolizing the CPU.
- **Improved System Responsiveness**:
   - Critical tasks get the CPU time they need when they need it.
---

### CPU Scheduling

**Definition**:  
CPU scheduling is the process of determining which process or thread gets access to the CPU at any given time. It plays a critical role in optimizing the performance of multitasking systems by efficiently allocating CPU resources among multiple processes.

---

### Load Balancing

**Definition**:  
Load balancing refers to the distribution of workloads across multiple CPU cores to ensure that no single core is overwhelmed.

**Goal**:  
To improve system performance and resource utilization by preventing bottlenecks and ensuring that CPU cores are used efficiently.

**Single Shared Queue**:  
Some operating systems, like Linux, implement a single shared queue for all CPU cores. In this model, processes are placed in a global queue, and any CPU can pull tasks from this queue. This ensures an even distribution of tasks across all cores.

**Advantages**:
- **Efficient Multicore Utilization**: Ensures that all CPU cores are actively processing tasks, preventing idle cores.
- **Scalability**: Helps maintain high performance as the number of cores increases by distributing workloads evenly.

---

### Priority Scheduling

**Definition**:  
Priority scheduling is a scheduling algorithm that assigns each process a priority level. Processes with higher priority are executed before those with lower priority.

**Types of Priority Scheduling**:

- **Preemptive**:  
  A running process can be interrupted if a higher-priority process becomes ready. This ensures that critical tasks are executed promptly.
  
- **Non-Preemptive**:  
  The CPU continues executing the current process until it finishes, even if a higher-priority process arrives. This method is simpler but can lead to longer waiting times for high-priority processes.

---

### Priority Inversion and Solutions

**Priority Inversion**:  
A scenario in which a lower-priority process holds a resource needed by a higher-priority process, causing delays for the higher-priority process.

**Solutions**:

- **Priority Inheritance**:  
  The lower-priority process temporarily inherits the higher priority until it releases the resource, helping to prevent the blocking of higher-priority tasks.

---

### Advantages and Disadvantages of Priority Scheduling

**Advantages**:
- Ensures that critical tasks are executed promptly.
- Suitable for real-time systems where deadlines must be met.

**Disadvantages**:
- **Starvation**: Lower-priority processes might never get CPU time if high-priority tasks continue to arrive.

---

### Dynamic Priority Adjustment and Aging

**Dynamic Priority Adjustment**:  
Priorities are adjusted based on factors like resource usage, system load, or waiting time. This prevents low-priority processes from being indefinitely delayed and ensures fairness in resource distribution.

**Example**:  
In Linux, the Completely Fair Scheduler (CFS) uses dynamic priority adjustments to distribute CPU time fairly among tasks.

**Aging Mechanism**:  
Aging is a technique used to prevent starvation by gradually increasing the priority of a process the longer it waits in the queue. This guarantees that even low-priority processes eventually get CPU time, balancing fairness with responsiveness.

---

### Scheduling Algorithms

1. **Preemptive Scheduling**:
   - **Round Robin (RR)**: Processes are assigned fixed time slices (quantums), ensuring fair sharing of CPU time. However, this can lead to increased context-switching overhead.
   - **Priority Scheduling**: Higher-priority processes preempt lower-priority ones.

2. **Non-Preemptive Scheduling**:
   - **First Come First Serve (FCFS)**: Processes are scheduled in the order they arrive. It is simple but can lead to poor performance in some cases (e.g., convoy effect).
   - **Shortest Job First (SJF)**: The process with the shortest execution time is selected first, optimizing the total waiting time.

3. **Multi-Level Feedback Queue (MLFQ)**:  
   This scheduling algorithm allows processes to move between different priority queues based on their behavior and CPU burst time. Shorter processes are prioritized, while long-running processes are handled fairly in lower-priority queues. This flexibility ensures that both short and long processes are treated effectively.

---

### Fairness in Scheduling

**Definition**:  
Fairness ensures that all processes receive an equitable share of CPU time. It prevents scenarios where some processes are perpetually delayed due to others dominating the CPU.

**Starvation**:  
Starvation occurs when a low-priority process is indefinitely delayed because higher-priority processes continuously take precedence. This can commonly happen in priority-based scheduling.

**Solutions to Starvation**:
- **Aging**:  
  Increases the priority of waiting processes over time, ensuring they eventually get CPU time.
  
- **Priority Adjustment**:  
  Dynamically adjusts the priorities of processes based on resource needs or system load to maintain fairness.

---

### Load Balancing in CPU Scheduling

**Definition**:  
Load balancing ensures that workloads are evenly distributed across multiple CPU cores to prevent any single core from being overworked while others remain idle. This improves overall system performance by utilizing available CPU resources efficiently.

**Approaches to Load Balancing**:

1. **Static Load Balancing**:  
   - Workload distribution is predefined and doesn’t consider the current system load. It is suitable for predictable workloads but may not adapt well to varying demands.

2. **Dynamic Load Balancing**:  
   - Adjusts workloads at runtime based on the system's current state. Examples include:
     - **Task Migration**: Processes are moved between cores to balance the load.
     - **Work-Stealing**: Idle cores "steal" tasks from busy cores, ensuring even distribution.

**Single vs. Multi-Queue Systems**:
- **Single Shared Queue**: All CPU cores pull tasks from a single global queue. While this ensures fairness, it can cause contention when many cores access the same queue.
- **Multi-Queue Scheduling**: Each CPU core has its own queue. Processes are either assigned to specific cores or migrated between queues to maintain load balance.

---

### Conclusion

Effective CPU scheduling and load balancing are vital to ensuring that modern multitasking systems operate smoothly. Scheduling algorithms, dynamic priority adjustments, and mechanisms like load balancing help manage CPU resources effectively, improving both system performance and fairness across all processes. Techniques like **aging** and **priority adjustment** prevent low-priority processes from being indefinitely delayed, creating a balanced, responsive system.
