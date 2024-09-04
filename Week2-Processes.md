---
layout: default
title: Week 2 - Processes State
nav_order: 3
---

## CSDS 338 Review Session - Processes and Calls

<br>

## **Contents**

1. [Processes](#1-processes)  
2. [Process States](#2-process-states)  
3. [Waiting Processes](#3-waiting-processes)  
4. [Background vs. Foreground Processes](#4-background-vs-foreground-processes)  
5. [Useful Options and Commands](#5-useful-options-and-commands)  
6. [Introduction to Scheduling](#6-introduction-to-scheduling)  
7. [Summary](#7-summary)  

---

### Processes and Their Components

A **process** is an instance of a program in execution. It includes several components such as the program code (instructions), data, and the current state of execution (like variable values, the state of the processor registers, etc.). The operating system's job is to manage these processes, ensuring they are executed efficiently by the CPU.

#### Key Components of a Process

1. **PID (Process ID)**:  
   Every process has a unique identifier called a **Process ID (PID)**. The operating system uses this to track and manage processes. When a program starts executing, the OS assigns a PID to it.

2. **Memory Segments of a Process**:
   The process’s memory is divided into different segments:
   
   - **Stack**:  
     The stack holds local variables and function call information. It grows and shrinks dynamically as functions are called and return.  
     It also keeps track of the program's execution by storing return addresses and frame pointers, ensuring the program can resume properly after functions finish executing.

   - **Heap**:  
     The heap is where dynamically allocated memory resides. When your program needs memory during execution (e.g., through `malloc` in C or `new` in Java), it comes from the heap. This area grows and shrinks based on the program’s needs and can become fragmented over time.

   - **Code (Text)**:  
     The text segment contains the compiled program code (instructions to be executed). This part is typically read-only to prevent modification during execution.

   - **Data**:  
     The data segment stores global variables and static data that are used by the program. It is divided into two sections: initialized data (global variables that are explicitly initialized) and uninitialized data (variables that are not explicitly initialized).

#### Commands to View and Manage Processes

The operating system provides several commands to interact with processes. Some of the most common are `ps` and `top`.

1. **ps** (Process Status):  
   The `ps` command lists currently running processes on your system. It provides a snapshot of processes at the time it was run, not real-time data. There are various options to customize the output:
   
   - `ps aux`:  
     This is a commonly used variation that gives detailed information about all processes on the system. Here’s a breakdown:
     - **a**: Shows processes for all users, not just the current user.
     - **u**: Displays more detailed information (e.g., user name, CPU usage, memory usage).
     - **x**: Includes processes that are not attached to a terminal, such as background processes (also known as daemon processes).
     
     Example usage:
     ```bash
     ps aux
     ```

2. **top** (Real-time Process Monitoring):  
   The `top` command provides a dynamic, real-time view of system processes, similar to a task manager. It updates automatically and displays information like CPU and memory usage for each process, allowing you to monitor system performance continuously.
   
   Output of `top` typically includes:
   - **PID**: The process ID.
   - **USER**: The user who started the process.
   - **PR (Priority)**: The scheduling priority.
   - **%CPU**: Percentage of CPU usage.
   - **%MEM**: Percentage of memory usage.
   - **TIME**: Total CPU time the process has used.

   You can interact with `top` by using keyboard commands while it’s running:
   - Press `q` to quit.
   - Press `k` to kill a process by its PID.
   - Press `r` to change the priority of a process.

   Example usage:
   ```bash
   top
   ```

   Example output from `top`:
   ```
   PID USER   PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
   1   root   20   0  1024  428  392 S  0.0  0.1   0:00.03 init
   2   root   20   0     0    0    0 S  0.0  0.0   0:00.00 kthreadd
   3   root   20   0     0    0    0 S  0.0  0.0   0:00.00 ksoftirqd/0
   ```

By understanding these commands and the structure of a process, you can better monitor, manage, and troubleshoot processes on a Linux system. This knowledge is essential when working with system performance and managing process lifecycles.

---

### Process Lifecycle and States

A process moves through various stages (states) as it executes. These states reflect whether the process is actively using system resources, waiting for something to happen, or if it has completed its execution. The operating system tracks these states and manages transitions between them using a process scheduler.

#### Process Lifecycle

1. **New**:  
   The process is being created. At this stage, the operating system is preparing the process control block (PCB), allocating resources, and loading the program into memory.

2. **Ready (R)**:  
   The process has been created, has all the necessary resources (except the CPU), and is ready to run. It is placed in the ready queue, where it waits for CPU time. Multiple processes can be in this state, and the OS scheduler picks one to move to the running state.

3. **Running (R)**:  
   The process is actively executing on the CPU. During this time, the CPU is working on the instructions of the process. Only one process can be in the running state per core at a time (unless the system supports multi-threading or multiprocessing). 

4. **Waiting**:  
   The process cannot proceed because it is waiting for some event or resource, such as I/O operations or the availability of memory. There are two types of waiting states:
   
   - **Interruptible Sleep (S)**:  
     The process is waiting for a specific event (e.g., user input or network data). It can be interrupted by signals, such as a keyboard interrupt, and returned to the ready state.
   
   - **Uninterruptible Sleep (D)**:  
     The process is waiting for resources that cannot be interrupted, such as I/O operations that must complete without interruption. These processes cannot handle signals until the resource becomes available.

5. **Terminated (Z)**:  
   Also known as a **zombie** state, the process has completed execution but still has an entry in the process table. This state allows the parent process to retrieve the exit status of the terminated process. Once the parent process acknowledges the exit status, the zombie process is removed from the process table.

6. **Blocked**:  
   This is a special case where a process is waiting for resources like memory or I/O devices. While similar to the waiting state, "blocked" specifically refers to resource contention.

#### Process States in `top`

When using `top`, you can observe these process states in the **S** column. Here's how to interpret them:

- **R (Running)**:  
  The process is actively using CPU time.
  
- **S (Sleeping/Interruptible Sleep)**:  
  The process is waiting for an event to occur, such as user input or the completion of an I/O operation. It can be interrupted by signals.
  
- **D (Uninterruptible Sleep)**:  
  The process is waiting for a resource and cannot be interrupted until that resource becomes available. For example, a process waiting for disk I/O might be in this state.

- **Z (Zombie)**:  
  The process has terminated, but its entry still exists in the process table because the parent process hasn't yet retrieved its exit status.

- **T (Stopped or Traced)**:  
  The process has been stopped (paused) by a signal or is being traced during debugging.

#### Example Process Lifecycle Scenario

1. A user launches a program, which creates a new process (**New** state).
2. The process is loaded into memory and moves to the **Ready** state.
3. The OS scheduler assigns CPU time, and the process begins executing (**Running** state).
4. During execution, the process requests data from the disk, moving to **Waiting** (interruptible sleep) until the disk operation completes.
5. Once the data is retrieved, the process returns to the **Ready** state, waiting for more CPU time.
6. After finishing all tasks, the process enters the **Terminated** (or **Zombie**) state until the parent process collects the exit code.

### Understanding Process States with `top`

You can monitor the state of processes using the `top` command. The **S** column in `top` displays the state of each process:

- Running (R): Process is using the CPU.
- Sleeping (S): Process is waiting for an event.
- Uninterruptible Sleep (D): Process is waiting for a resource that cannot be interrupted.
- Zombie (Z): Process has completed but has not been cleaned up.


---

### 3. Waiting Processes

**Waiting processes** occur when a process needs some resource that isn't immediately available, such as I/O operations, memory, or other system resources. During this time, the process is placed in a non-executing state, allowing the CPU to focus on other tasks. This ensures that the CPU isn’t left idle while waiting for slower operations, like disk reads or network access, to complete.

#### Importance of Waiting Processes

- **I/O Operations vs. CPU Speed**:  
  I/O operations, such as reading from a hard disk, network communication, or interacting with external devices, are much slower compared to CPU tasks. For example, while a CPU operation can take nanoseconds, reading data from a disk might take milliseconds. If a process is solely waiting for I/O to complete, the CPU would be wasted if the OS didn’t have a mechanism to manage these idle periods.

- **Efficient CPU Utilization**:  
  Instead of waiting for these slower operations to finish, the operating system temporarily pauses the process and switches to another process that can utilize the CPU. This ensures that the CPU is kept busy, improving overall system performance.

#### Context Switching

**Context switching** is the process by which the operating system saves the state of the currently running process and restores the state of another process so it can be executed. This happens when the current process is put into a waiting state or when the scheduler decides to allocate CPU time to a different process.

- **How it works**:
  - The OS saves the **state** (registers, program counter, etc.) of the current process into its Process Control Block (PCB).
  - The OS then loads the state of the next process to be executed.
  - The newly scheduled process begins or resumes its execution on the CPU.
  - This switching between processes happens rapidly, allowing the system to appear as though multiple processes are running simultaneously (multitasking).

This ensures that processes waiting on slow I/O operations don’t block the system, and the CPU can be reallocated to processes that can make use of it.

#### Example of a Waiting Process Scenario

Consider a situation where a program requests data from the disk:
1. The program (process) makes a request for a file.
2. The OS sends the request to the disk, but retrieving the data takes some time.
3. Instead of making the CPU wait for the data to be read, the OS moves this process into the **waiting state**.
4. While the process waits for the disk to provide the data, the OS switches to another process that is ready to run.
5. Once the data is available from the disk, the process is moved from the waiting state back to the ready state and eventually gets scheduled to run again.

### 4. Background vs. Foreground Processes (Expanded)

When interacting with the terminal, you can choose to run processes in two modes: **foreground** and **background**. Understanding the difference allows you to manage multiple tasks more efficiently without waiting for each one to complete before starting the next.

#### **Foreground Process**

- A **foreground process** is one that directly interacts with the terminal. It receives input from the user and outputs to the terminal. While it's running, you cannot execute new commands until the process finishes.
- Example:
    ```bash
    ./my_program
    ```
    In this case, `my_program` runs in the foreground, and you won’t regain control of the terminal until it completes execution.

#### **Background Process**

- A **background process** runs without blocking the terminal, meaning you can continue using the terminal for other commands while the process executes.
- To run a process in the background, append an `&` to the command:
    ```bash
    ./my_program &
    ```
    This sends `my_program` to the background, allowing the terminal to accept further commands immediately.

#### **Pausing and Resuming Processes**

- **Pausing (Ctrl+Z)**:  
  Pressing `Ctrl+Z` pauses (stops) the currently running foreground process, placing it in the background in a suspended state.
  
- **Resuming in Background (bg)**:  
  Once a process is paused, you can send it to run in the background with the `bg` command. This allows the terminal to remain free while the process continues to run.
  
- **Resuming in Foreground (fg)**:  
  The `fg` command brings a background process back to the foreground, where it resumes its interaction with the terminal.

#### Example Workflow

1. Start a long-running program in the foreground:
    ```bash
    ./long_running_program
    ```
2. Pause it with `Ctrl+Z`, freeing the terminal for other tasks.
3. Move the process to the background:
    ```bash
    bg
    ```
    This allows the program to continue running in the background.
4. If you need to interact with it again, bring it back to the foreground:
    ```bash
    fg
    ```

This ability to manage processes between the foreground and background is crucial for multitasking in a terminal environment, enabling you to handle long-running tasks without waiting.

---

## **5. Useful Options and Commands**

Here are some helpful options and commands that make managing processes easier:

- **Listing Processes**:
  - `ps aux` gives a snapshot of the current processes along with details like CPU and memory usage, user, and process ID.

    Example:
    ```bash
    ps aux | grep firefox
    ```
    This command lists all processes and filters those containing the word `firefox`.

- **Terminating a Process**:
  - **kill**: Sends a signal to a process to terminate it. Use the **PID** from `ps` or `top` to kill the process.
    ```bash
    kill <PID>
    ```
  - **kill -9**: Force kills the process, which can’t be ignored by the process.
    ```bash
    kill -9 <PID>
    ```

- **Nice and Renice**: Adjusting Process Priority
  - **nice**: Start a process with a specific priority. Lower values have higher priority, while higher values have lower priority.
    ```bash
    nice -n 10 ./my_program
    ```
  - **renice**: Change the priority of an already running process.
    ```bash
    renice -n -5 -p <PID>
    ```

**Filtering Processes in `top`**:
- You can filter processes in `top` using options:
  - Show only processes owned by a user:
    ```bash
    top -u <username>
    ```
  - Set the refresh delay to 5 seconds:
    ```bash
    top -d 5
    ```

---

### 6. Introduction to Scheduling (Expanded)

**Scheduling** is a critical function of the operating system, managing how processes access the CPU and determining which processes should be executed and for how long. The primary goals of scheduling are to maximize CPU utilization, minimize wait times, and ensure fair allocation of resources among processes.

#### **Types of Scheduling Algorithms**

1. **First-Come, First-Served (FCFS)**:
   - In FCFS, processes are scheduled in the order they arrive in the ready queue. The process that arrives first will be executed first, regardless of its size or required processing time.
   - **Pros**: Simple to implement and easy to understand. There’s no need for complex calculations.
   - **Cons**: It can lead to the **convoy effect**, where shorter processes have to wait for a long process to finish, leading to inefficient CPU use. This could cause long average waiting times, especially for shorter processes that follow longer ones.

2. **Round-Robin (RR)**:
   - In Round-Robin scheduling, each process is assigned a small unit of CPU time, called a **time slice** or **quantum**. After its time expires, the process is placed back in the ready queue, and the next process in line gets a turn.
   - **Pros**: This approach is fair because every process gets an equal share of CPU time. It’s particularly useful in time-sharing systems, where the goal is to make each user feel like they have exclusive access to the system.
   - **Cons**: If the time slice is too small, the system could spend too much time context-switching between processes, reducing overall efficiency. Conversely, if the time slice is too long, it can behave like FCFS.

3. **Priority Scheduling**:
   - In Priority Scheduling, each process is assigned a priority level. The scheduler selects processes with the highest priority first. Priority can be static (set at the start) or dynamic (adjusted during execution).
   - **Pros**: Important or time-sensitive tasks can be executed sooner, making the system more responsive for critical processes.
   - **Cons**: This approach can lead to **starvation**, where lower-priority processes might never get CPU time if higher-priority processes continually arrive. To prevent this, some systems implement **aging**, where a process’s priority increases the longer it waits.

4. **Multilevel Queue Scheduling**:
   - Processes are divided into different queues based on factors like priority or process type (e.g., system processes, interactive processes, batch jobs). Each queue has its own scheduling algorithm, and the system schedules processes within each queue independently.
   - **Pros**: Allows for specialized scheduling for different types of tasks. For example, interactive tasks can be given priority over background tasks.
   - **Cons**: Once a process is assigned to a queue, it may be difficult to adjust its priority or move it between queues.

#### **Linux Scheduling – Completely Fair Scheduler (CFS)**

Linux uses the **Completely Fair Scheduler (CFS)**, which aims to allocate CPU time in a fair and balanced way based on the concept of **virtual runtime**. Each process gets a fair share of CPU time according to its priority, and CFS ensures that every process gets enough CPU time relative to others. Interactive tasks, like responding to user inputs, generally have higher priority, ensuring that the system remains responsive to user actions.

CFS avoids the issues of older algorithms (like Round-Robin’s context switching overhead or Priority Scheduling’s starvation risk) by balancing fairness and efficiency.

#### **Adjusting Scheduling with `nice`**

The **`nice`** command in Linux allows you to influence the scheduling priority of a process. The nice value ranges from -20 (highest priority) to 19 (lowest priority). By default, processes are started with a nice value of 0. Lower nice values mean higher priority, meaning the process will get more CPU time.

- **Example of setting a process with a higher priority**:
  ```bash
  nice -n -5 ./important_task
  ```
  This command runs `important_task` with a nice value of -5, meaning it will get more CPU time compared to processes with a default or higher nice value.

- **Example of reducing a process's priority**:
  ```bash
  nice -n 10 ./background_task
  ```
  This command sets `background_task` to run with a lower priority, allowing other more important processes to use the CPU first.

By adjusting the nice value, you can fine-tune how the system allocates CPU resources to different tasks, ensuring that critical processes are prioritized, while less important tasks run in the background without hogging CPU time.

 #### **SomeTrade-offs:**  
 
- FCFS: Simple, low context switch overhead, but long wait times for shorter processes.
- Round-Robin: Fair time allocation but can incur high context switching overhead.
- Priority Scheduling: Good for handling important tasks first but risks starvation for lower-priority processes.
- Multilevel Queue: Offers flexibility for different process types but can be complex and may lead to starvation in lower-priority queues.
- CFS: Balanced approach with minimal context switching, though not always suited for real-time applications.

#### **Balancing Context Switching and Performance**

The key to choosing the right scheduling algorithm is understanding the trade-offs in context switching and system responsiveness:

- **Too many context switches** reduce overall CPU efficiency due to the time spent saving and restoring process states.
- **Too few context switches,** on the other hand, can lead to unfairness, with some processes hogging the CPU while others starve or become unresponsive.

For example, in Round-Robin, a short time slice leads to too many context switches, causing overhead. In FCFS, long-running processes can dominate the CPU, causing other processes to wait too long.
---

## **Summary**

### **1. Processes**
- A **process** is a program in execution, consisting of program code, memory, and execution state.
- **Threads** are smaller execution units within a process.
- **Commands**:
  - `ps aux`: Lists running processes.
  - `top`: Displays real-time process information (e.g., CPU and memory usage).

---

### **2. Process States**
- Processes move through states: **Running**, **Ready**, **Waiting**, **New**, and **Terminated**.
- **Running**: The process is actively using the CPU.
- **Ready**: Waiting for CPU time.
- **Waiting**: Awaiting resources (e.g., I/O).
- **Terminated**: The process has completed.

**Key Command**:  
`top` - Check the `S` column to see a process's state.

---

### **3. Waiting Processes**
- **Waiting** processes free up the CPU while waiting for resources (e.g., I/O operations).
- The OS switches between processes to maximize CPU usage (**context switching**).

---

### **4. Background vs. Foreground Processes**
- **Foreground** processes block the terminal until completed.
- **Background** processes run while the terminal remains available for other commands.
- **Commands**:
  - Run a process in the background with `&`:  
    ```bash
    ./program &
    ```
  - Use `Ctrl+Z` to pause a process, `bg` to move it to the background, and `fg` to bring it back to the foreground.

---

### **5. Useful Options and Commands**
- **Terminating a Process**:  
  ```bash
  kill <PID>
  ```
- **Adjust Process Priority**:
  - `nice -n <value> ./program` - Start a process with a specified priority.
  - `renice -n <value> -p <PID>` - Adjust the priority of a running process.

---

### **6. Scheduling**
- The OS determines which processes run using scheduling algorithms:
  - **First-Come, First-Served (FCFS)**: Simple, but can cause long wait times.
  - **Round-Robin (RR)**: Each process gets a time slice to run.
  - **Priority Scheduling**: Higher-priority processes run first.
  - **Linux CFS**: A fair scheduler that allocates CPU time based on the process's priority (niceness value).
  
**Commands**:
- `nice`: Set the priority of a new process.
- `renice`: Adjust the priority of an existing process.


