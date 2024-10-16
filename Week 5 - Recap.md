---
layout: default
title: Week 5 - Recap
nav_order: 5
---

### **Operating Systems: Weekly Recap and Key Concepts**  

This week, the professor guided us through several fundamental **operating system concepts**, focusing on **process scheduling, memory management, process monitoring, and useful terminal tools like `vi`**. We explored how the **Linux kernel** allocates CPU time among processes, the **memory layout of running programs**, and effective ways to **monitor processes using the `/proc` filesystem**. The professor also emphasized **hands-on tools** for efficient file editing with `vi` and demonstrated how Linux's **Completely Fair Scheduler (CFS)** ensures fair CPU allocation. Here’s a detailed breakdown of the topics discussed:

---

## **1. Process Scheduling with the Completely Fair Scheduler (CFS)**

One of the most critical parts of this week’s lessons was the **Linux scheduler**, specifically **CFS (Completely Fair Scheduler)**. The professor explained how CFS ensures that all processes receive a fair share of CPU time. Key terms from CFS include:

- **Virtual Runtime (`vruntime`)**:  
  `vruntime` is a metric used to measure how much CPU time a process has received.  
  - **Lower `vruntime`**: The process has run for less time and will be prioritized by the scheduler.  
  - **Higher `vruntime`**: The process has consumed more CPU time and is temporarily deprioritized to allow others to run.

This week, the professor demonstrated how **sorting by `vruntime`** helps the scheduler decide which process should run next. Processes with **smaller `vruntime`** are run first to ensure fair CPU usage among tasks.

- **Impact of Priority (`prio`)**:  
  The professor noted that processes with different **priorities** can impact how quickly they accumulate `vruntime`. For example, **high-priority processes** add to their `vruntime` more slowly, giving them more frequent CPU time.

---

## **2. Process Monitoring with the `/proc` Filesystem**

The **`/proc` filesystem** is a virtual interface provided by Linux to expose detailed information about the **current state of processes**. This week, the professor introduced commands and scripts to extract valuable process information, such as:

- **`/proc/[pid]/sched`**: Displays scheduling information for a specific process by its PID. Fields include:
  - **`prio`**: Priority of the process.
  - **`sum_exec_runtime`**: Total time the process has spent executing on the CPU.
  - **`start`**: The process's start time relative to system uptime.
  - **`#threads`**: The number of threads the process is currently using.

The **`awk` script** the professor showed allows us to track changes in **runtime and scheduling priority** every 500ms, giving a real-time view of how the scheduler balances CPU time between processes. This script emphasizes the importance of monitoring **how processes consume resources** to ensure smooth system performance.

---

## **3. Memory Management: Page Size and Alignment**  
The professor also walked through **memory mapping concepts**, with an emphasis on **page size and address alignment**. 

- **Page Size**:  
  In most systems, the default **page size** is **4KB (4096 bytes)**. This means that each memory block loaded into RAM corresponds to a 4KB chunk. Using pages simplifies the management of both physical and virtual memory.

- **Address Alignment**:  
  In the memory map, **all addresses end in `000`** in hexadecimal. This indicates that they are **aligned on 4KB boundaries**. Alignment ensures efficient use of memory by avoiding partial pages, making the memory mapping process more reliable and faster.

- **Reading the Memory Map**:  
  The professor used **`/proc/[pid]/maps`** to illustrate how a process's memory is divided between **code (text), data, stack, and heap** segments. This command helps monitor how the OS allocates and protects these segments, crucial for preventing unauthorized access.

---

## **4. vi Editor: Essential Commands for System Admins**

The professor spent time introducing essential **`vi` commands**, which are useful for **editing files directly in the terminal**. This week’s focus was on **editing, navigating, and searching within files**, which is essential for those working on remote servers or in Linux environments. Here are the key commands covered:

- **Insert Mode**:  
  - `i`: Insert text before the cursor.  
  - `a`: Insert text after the cursor.  
  - `o`: Open a new line below the current one.  

- **Navigation**:  
  - `0`: Move to the beginning of the current line.  
  - `$`: Move to the end of the current line.  
  - `w`: Jump forward to the next word.  
  - `b`: Move back to the previous word.  
  - `G`: Go to the last line of the file.  
  - `1G`: Jump to the first line of the file.  

- **Editing Commands**:  
  - `cw`: Change the word under the cursor.  
  - `dw`: Delete the word under the cursor.  
  - `Y`: Copy (yank) the current line.  
  - `p`: Paste the copied content below the cursor.  

- **Saving and Exiting**:  
  - `[ESC] :wq`: Save changes and quit.  
  - `[ESC] /foo`: Search for the word “foo” in the text.

These commands allow for **quick and efficient file manipulation**, which is especially useful when managing configuration files or writing scripts on a server.

---

## **5. Hands-on Demo: Monitoring Process Behavior in Real-Time**

The professor’s **`awk` script** demonstrated how we can monitor process statistics continuously. The script outputs the **`vruntime`**, **priority**, and **runtime** for each process every **500ms**, allowing us to observe:

- **How processes accumulate `sum_exec_runtime`.**
- **How `prio` affects the scheduling behavior** of the CFS.
- **Which processes are running vs. waiting** to be scheduled.

This real-time view provides critical insights into **process behavior**, especially in systems with many running tasks. Such monitoring is vital for identifying bottlenecks and balancing system load.

---

## **6. Recap of Useful Commands from This Week**

In addition to working with **`/proc`** and **`vi`**, the professor provided a list of other useful commands for monitoring and managing processes:

- **`top` and `htop`**:  
  - These commands provide an interactive view of system resources, including **CPU and memory usage** by each process.

- **`ps aux`**:  
  - Shows a detailed list of all running processes, including their **PID, CPU usage, and memory usage**.

- **`kill`**:  
  - Used to **terminate processes** by sending signals such as `SIGKILL` (force kill) or `SIGTERM` (polite termination).

- **`nice` and `renice`**:  
  - Used to change the **priority of a process**. The professor demonstrated how these commands can impact **CPU scheduling**.

---

## **Conclusion**

This week’s lectures provided a comprehensive look into **process scheduling, memory management, and process monitoring**. The professor emphasized the importance of **fair CPU allocation through CFS**, efficient **memory mapping** with aligned pages, and **real-time process monitoring** using tools like `/proc` and `top`. Additionally, mastering **`vi` commands** is essential for anyone working in Linux environments to manage files quickly and efficiently.

These concepts are not only important for understanding how operating systems work under the hood but also essential for practical tasks such as **troubleshooting, system performance tuning, and managing workloads** in a real-world environment.
