
### **CSDS 338 Review Session - Processes and Calls**

---

### **Contents**

1. [Processes](#1-processes)  
   1.1 [Definition of a Process](#definition)  
   1.2 [Key Components of a Process](#components)  
   1.3 [Commands to View and Manage Processes](#commands)  

2. [Process States](#2-process-states)  
   2.1 [Understanding Process States](#understanding-states)  
   2.2 [Process States in `top`](#states-in-top)  

3. [Waiting Processes](#3-waiting-processes)  
   3.1 [Why Waiting Processes are Important](#importance-of-waiting)  
   3.2 [Context Switching](#context-switching)  

4. [Background vs. Foreground Processes](#4-background-vs-foreground-processes)  
   4.1 [Foreground Processes](#foreground-processes)  
   4.2 [Background Processes](#background-processes)  
   4.3 [Pausing and Resuming Processes](#pausing-and-resuming)  

5. [Useful Options and Commands](#5-useful-options-and-commands)  
   5.1 [Listing Processes with `ps`](#listing-processes)  
   5.2 [Terminating a Process with `kill`](#terminating-process)  
   5.3 [Adjusting Process Priority with `nice` and `renice`](#adjusting-priority)  

6. [Introduction to Scheduling](#6-introduction-to-scheduling)  
   6.1 [What is Scheduling?](#what-is-scheduling)  
   6.2 [Types of Scheduling Algorithms](#types-of-scheduling)  
   6.3 [Linux Scheduling (CFS)](#linux-scheduling)  

7. [Summary](#7-summary)  
   7.1 [Processes Recap](#processes-recap)  
   7.2 [Process States Recap](#states-recap)  
   7.3 [Waiting Processes Recap](#waiting-recap)  
   7.4 [Background vs. Foreground Recap](#bg-vs-fg-recap)  
   7.5 [Useful Commands Recap](#useful-commands-recap)  
   7.6 [Scheduling Recap](#scheduling-recap)

---

### **1. Processes**

**Definition**:  
A **process** is an executing program that includes the program code, its data, and the state of the execution (e.g., variables, registers, stack, heap). The operating system manages the execution of processes by scheduling them on the CPU.

**Key Components of a Process**:
- **PID (Process ID)**: A unique identifier for each process.
- **Memory Segments**:
  - **Stack**: Stores local variables and function call information.
  - **Heap**: Dynamically allocated memory.
  - **Code (Text)**: The instructions of the program.
  - **Data**: Global variables and program data.
  
- **Commands to View and Manage Processes**:
  - **`ps`**: Lists currently running processes.
    ```bash
    ps aux
    ```
    - `a` lists processes for all users, `u` shows detailed info, `x` includes processes not connected to a terminal.
  
  - **`top`**: Displays real-time information about processes, including CPU and memory usage.

    Example:
    ```bash
    top
    ```

    ![Top Output Example](https://example.com/top-output.png)

---

### **2. Process States**

Processes can be in different states depending on whether they are currently using the CPU, waiting for resources, or have completed their execution.

**Process States**:
- **Running (R)**: The process is actively using the CPU.
- **Ready (R)**: The process has everything it needs but is waiting for CPU time.
- **Waiting (S or D)**: The process is waiting for resources (like I/O operations or memory).
  - **S** (Interruptible Sleep): Can be interrupted by signals.
  - **D** (Uninterruptible Sleep): Waiting for resources that cannot be interrupted.
- **Terminated (Z)**: The process has finished execution.
- **New**: The process is being created.

**Understanding Process States in `top`**:
- The `S` column in the `top` command output shows the process's state, which can be running (R), sleeping (S), or in other states like zombie (Z).

**Example**:
- **Running `top`**:
  ```bash
  top
  ```
  - Look for the `S` column to identify whether processes are running (R), sleeping (S), or in other states.
  - **PID**: Process ID.
  - **USER**: The user running the process.
  - **NI**: The "nice" value (priority of the process).

---

### **3. Waiting Processes**

Processes may enter a waiting state when they need resources that are not currently available. This state is critical for efficient CPU utilization, as it allows the operating system to switch out a process waiting on I/O or other resources and run another process that is ready.

**Why Waiting Processes are Important**:
- I/O operations (like reading from disk or network) are much slower than CPU operations.
- Instead of leaving the CPU idle, the OS puts waiting processes in a **waiting state** and switches the CPU to run another process. This is called **context switching**.

**Example Scenario**:  
A process requests data from the hard drive, but while waiting for the data, the OS switches to another process that’s ready to run, so the CPU isn't idle.

**Context Switching**:
- The OS saves the state of the current process and loads the state of the next process.
  
**Command to See Waiting Processes**:
- In `top`, processes with a state of `S` (Sleeping) or `D` (Uninterruptible Sleep) are waiting for resources.
- Example:
    ```bash
    top
    ```
    Look at the processes that are in the "S" or "D" states and observe which resources they might be waiting for.

---

### **4. Background vs. Foreground Processes**

When working in the terminal, a process can either run in the **foreground**, where it actively interacts with the user, or in the **background**, where it runs independently of the terminal's input.

**Foreground Process**:
- A foreground process is the currently active process that takes input from the terminal and blocks further commands until it completes.
- For example, running:
    ```bash
    ./my_program
    ```
    will execute `my_program` in the foreground. You won’t be able to run other commands until it finishes.

**Background Process**:
- A background process runs without occupying the terminal, allowing you to continue using it for other tasks.
- To run a process in the background, add `&` to the end of the command:
    ```bash
    ./my_program &
    ```
    Now the terminal is free to accept more commands while `my_program` runs in the background.

**Pausing and Resuming Processes**:
- **Ctrl+Z**: Pauses (stops) the foreground process and returns control to the terminal.
- **bg**: Resumes a paused process in the background.
- **fg**: Brings a background process back to the foreground.

**Example**:
1. Start a process:
    ```bash
    ./long_running_program
    ```
2. Pause it with `Ctrl+Z`.
3. Move it to the background with:
    ```bash
    bg
    ```
4. Bring it back to the foreground with:
    ```bash
    fg
    ```

---

### **5. Useful Options and Commands**

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

### **6. Introduction to Scheduling**

**What is Scheduling?**  
Scheduling is the process by which the operating system determines which process gets to run on the CPU and for how long. The goal is to optimize CPU utilization and ensure processes are fairly and efficiently handled.

**Types of Scheduling Algorithms**:
- **First-Come, First-Served (FCFS)**:  
  The simplest scheduling algorithm. Processes are executed in the order they arrive.
  - **Pros**: Simple.
  - **Cons**: Can cause long wait times if a large process arrives before smaller ones.
  
- **Round-Robin (RR)**:  
  Each process is given a fixed time slice (quantum), and after that time expires, it goes back into the ready queue.
  - **Pros**: Fair time allocation for each process.
  - **Cons**: Lots of context switches can reduce efficiency.

- **Priority Scheduling**:  
  Each process is assigned a priority. Higher-priority processes get to run first.
  - **Pros**: Important processes get more CPU time.
  - **Cons**: Lower-priority processes can starve if higher-priority ones are constantly running.

- **Multilevel Queue Scheduling**:  
  Divides processes into multiple queues based on priority or process type. Each queue has its own scheduling algorithm.
  
**Linux Scheduling (CFS)**:  
Linux uses the **Completely Fair Scheduler (CFS)**, which aims to give each process a fair share of the CPU based on its priority (niceness value). Interactive tasks typically have higher priority to ensure they get CPU time quickly.

**Adjusting Scheduling with `nice`**:
- Use the `nice` command to influence how the scheduler prioritizes processes. Processes with lower nice values will receive more CPU time.
  
  Example:
  ```bash
  nice -n -5 ./important_task
  ```

---

### **Summary**

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


