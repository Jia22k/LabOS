---
layout: default
title: Week 3 - System calls, User mode vs Kernel
nav_order: 3
---


### **User Mode vs. Kernel Mode in Operating Systems**

In an operating system, there are two distinct modes of operation: **user mode** and **kernel mode**. These modes are crucial to the overall design and security of the operating system and help protect system resources.

1. **User Mode:**
   - **Definition:** User mode is where normal applications and processes run. In this mode, programs have restricted access to hardware and system resources (e.g., memory, CPU, I/O devices). They can only interact with the operating system through controlled interfaces like system calls.
   - **Purpose:** Prevent user applications from directly accessing critical system resources, which could lead to instability or security vulnerabilities.
   - **Limited Privileges:** Programs in user mode cannot execute sensitive operations, such as writing to protected memory areas, directly accessing hardware, or managing processes. Any attempt to perform these tasks in user mode results in a **trap** to the operating system.

2. **Kernel Mode:**
   - **Definition:** Kernel mode is where the operating system kernel runs. In this mode, the OS has full access to all hardware resources and can execute privileged instructions, like interacting with device drivers, managing memory, and scheduling processes.
   - **Purpose:** Kernel mode allows the OS to control and protect the system's hardware and enforce security policies.
   - **Full Privileges:** The OS can directly manage memory, execute I/O operations, and perform low-level system functions. However, only the OS kernel and trusted code (like device drivers) can run in kernel mode, ensuring that untrusted programs cannot disrupt system stability.

---

### **System Calls: The Bridge Between User Mode and Kernel Mode**

Programs running in **user mode** occasionally need to perform tasks that require privileged access, such as reading from a file, communicating with hardware, or managing memory. Since these tasks require **kernel mode** privileges, the operating system provides a mechanism called a **system call** to enable this controlled access.

1. **What is a System Call?**
   - A **system call** is a request made by a user program to the operating system to perform an action that requires higher privileges, such as interacting with hardware or managing system resources.
   - When a program needs to perform a task that cannot be done in user mode, it invokes a system call, which causes a controlled switch from user mode to kernel mode. Once the task is completed, control is returned to the user program.

2. **How System Calls Work:**
   - **Mode Transition:** When a program in user mode makes a system call (e.g., reading a file), the CPU switches to kernel mode so the operating system can execute the requested operation. After the system call completes, the CPU switches back to user mode.
   - **Privilege Separation:** This separation between user mode and kernel mode ensures that user programs can only access system resources in a controlled manner, preventing security risks or system crashes due to buggy or malicious code.

3. **Examples of Common System Calls:**
   - **File Operations:**
     - `open`, `read`, `write`, `close` — These system calls allow programs to interact with files by reading and writing data, as well as opening or closing files.
   - **Process Management:**
     - `fork` — Creates a new process.
     - `exec` — Replaces the current process image with a new program.
     - `wait` — Suspends execution until a child process finishes.
     - `exit` — Terminates the calling process.
   - **Memory Management:**
     - `mmap` — Maps files or anonymous memory into the process’s address space.
     - `brk` — Adjusts the end of the process’s heap for dynamic memory allocation.

---

### 1. **`mmap` (Memory Mapping)**

- **Purpose**: The `mmap` system call allows a process to map files or anonymous memory into its own address space. This means that a file or a block of memory can be treated as if it were directly part of the program’s memory, allowing fast access.
- **Use Cases**:
  - **File-backed mapping**: A file, like a database or a large log, can be mapped to memory, so that reading and writing to that file feels like reading and writing to memory. This speeds up file I/O operations since file data is handled similarly to memory data.
  - **Anonymous mapping**: Allocates memory not linked to any file. This is useful for allocating large blocks of memory, for example, when creating large buffers.

- **Example**:
  ```c
  void *addr = mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_PRIVATE | MAP_ANONYMOUS, -1, 0);
  ```
  In this example, the code maps 4 KB of anonymous memory (which is memory not tied to a file) into the process’s address space. The program can then read from and write to this memory.

### 2. **`brk` (Heap Management)**

- **Purpose**: The `brk` system call adjusts the size of the process's heap, which is a part of memory used for dynamic memory allocations (like when you use `malloc()` in C). The heap can grow or shrink depending on the memory requirements of the process.
- **How it works**: The `brk` call changes the end of the data segment (heap). When a program needs more memory, functions like `malloc()` will use `brk` to increase the heap size. Conversely, when memory is no longer needed, the heap size can be reduced.
  
- **Example**:
  ```bash
  brk(0x600000)  # Expands the heap to address 0x600000
  brk(0x601000)  # Further expands the heap by 4096 bytes (4KB)
  ```
  This changes the size of the heap, allowing the program to use more memory.

### 3. **`mmap` vs. `brk`**:

- **`brk`**: The `brk` system call can only grow the heap **contiguously**. This means the memory allocated by `brk` must be in one continuous block. If there’s no free space immediately after the heap, the `brk` call will fail, meaning the program cannot allocate more memory.
- **`mmap`**: More flexible compared to `brk`. `mmap` can allocate non-contiguous blocks of memory and is typically used for **large allocations** or when you need memory that doesn’t need to be next to the current heap. Additionally, `mmap` is useful for **mapping files** directly into memory, allowing programs to handle large files more efficiently.

### 4. **When to Use Each**:
- **Use `brk`** for small, contiguous allocations, like those done by `malloc()` in C.
- **Use `mmap`** for large memory allocations, when memory doesn’t need to be contiguous, or when mapping files directly into memory.

In summary, **`brk`** is limited to managing the heap and requires memory to be allocated in one continuous block, while **`mmap`** is more versatile, able to map files into memory and handle non-contiguous memory blocks.

### **Using `strace` to Trace System Calls**

To understand how system calls work in real-time, we can use a tool called **`strace`**. This tool captures and logs system calls made by a program, allowing you to see exactly what requests the program is making to the OS.

1. **What is `strace`?**
   - **`strace`** is a diagnostic and debugging tool for Linux. It traces all system calls made by a running program and outputs details such as the system call name, arguments, and the return value.
   - **Why use `strace`?** It helps developers and system administrators understand how a program interacts with the OS, debug errors, and monitor system resource usage.

2. **Example Usage of `strace`:**
   - **Command:**
     ```bash
     strace ./my_program
     ```
     This command will trace the system calls made by `my_program`.

3. **Tracking Memory Management with `strace`:**
   - **Tracking `brk`:**
     ```bash
     brk(0x600000) = 0x600000
     brk(0x601000) = 0x601000
     ```
     This output shows that the program is expanding its heap by using the `brk` system call to allocate additional memory.
   
   - **Tracking `mmap`:**
     ```bash
     mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f2bfae00000
     ```
     This shows that 8 KB of anonymous memory has been mapped into the process’s address space using `mmap`.

4. **Interpreting `strace` Output:**
   - The `strace` output provides detailed information about the system calls a process is making. For instance:
     ```bash
     open("/etc/passwd", O_RDONLY) = 3
     read(3, "root:x:0:0:root:/root:/bin/bash\n", 1024) = 34
     close(3) = 0
     ```
     This shows that the program is opening the `/etc/passwd` file, reading its contents, and then closing the file descriptor.

---

### **Connecting the Concepts:**

1. **User Mode and Kernel Mode Interaction via System Calls:**
   - Applications in **user mode** cannot directly access hardware or system resources. Instead, they rely on **system calls** to request services from the **kernel mode**.
   - System calls like `mmap`, `brk`, `fork`, `exec`, and `exit` are key tools for memory management, process creation, and resource allocation in an OS.

2. **Memory Management with `mmap` and `brk`:**
   - Efficient memory management is crucial for program performance. The OS provides **`mmap`** and **`brk`** to allow programs to manage memory dynamically, enabling them to request, map, and release memory as needed.
   - By using tools like **`strace`**, developers can monitor how programs use these system calls to interact with the OS, optimize memory usage, and debug issues related to resource management.

---
