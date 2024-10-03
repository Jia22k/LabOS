
## Processes

A **process** is more than just program code; it's an active entity in the system. It's essentially a running instance of a program being executed by the operating system.

### Key components of a process:

1. **Program Code**: Set of instructions the process executes.  
2. **Data**: Information the process works with (user input, system data, etc.).  
3. **Program Counter**: Tracks the current instruction being executed.  
4. **Stack**: Stores temporary information (function parameters, return addresses) to manage operations.

A process needs resources like memory and CPU time allocated by the operating system for smooth execution.

---

## Process Control Block

A **Process Control Block (PCB)** contains information about the process and is essential for the operating system to manage processes.

### Key components of a PCB:

1. **Process ID (PID)**: A unique identifier for the process.  
2. **Process State**: Indicates the current state of the process (e.g., New, Ready, Running, Waiting, Terminated).  
3. **Program Counter**: Holds the address of the next instruction the process needs to execute.  
4. **CPU Registers**: Stores the process's current working registers, which need to be saved during a context switch.  
5. **Memory Management Information**: Information regarding the process's memory allocation, including pointers to the process's code, data, and stack segments.  
6. **I/O Status Information**: Tracks I/O devices assigned to the process, open files, etc.  
7. **Scheduling Information**: Includes priority information and other details used by the scheduler to determine process execution order.

---

## Memory Management

Memory management is a critical part of an operating system, responsible for handling the allocation and management of memory for processes.

Here’s a breakdown of how memory works in a typical program, like the one you provided, using different segments of memory in the system:

### 1. **Text Segment**
- The **text segment** is where the actual compiled code of the program is stored, meaning the binary instructions that the CPU will execute.
- In the provided C program, the function definitions like `main()` and `example_function()` are part of the text segment. The text segment is usually **read-only** to prevent accidental modifications to instructions during execution, which could cause crashes or undefined behavior.

### 2. **Data Segment**
- The **data segment** holds global variables and static variables that are **explicitly initialized** before runtime. These variables persist throughout the lifetime of the program.
- In your program, the variable `global_var = 10` is part of the **data segment** because it’s a global variable that has been initialized.
- This segment is typically divided into two sub-segments: **read-only data** (e.g., constants) and **read-write data** (e.g., modifiable global/static variables).

### 3. **BSS Segment (Block Started by Symbol)**
- The **BSS segment** holds uninitialized global and static variables. Before the program runs, these variables are automatically initialized to zero.
- In the program, `uninitialized_var` is part of the **BSS segment** because it's a global variable that hasn't been explicitly initialized.
- This segment is usually larger in programs that reserve memory but don’t immediately initialize it.

### 4. **Heap Segment**
- The **heap** is a dynamic memory area. It grows as needed and is used for dynamically allocated memory (e.g., memory allocated using `malloc()` in C).
- In your program, `heap_var` is a pointer to a dynamically allocated block of memory in the heap segment. It was allocated using `malloc()`.
- Heap memory needs to be managed manually, which means the programmer is responsible for allocating (`malloc`) and deallocating (`free`) it. If you forget to free memory, it could cause **memory leaks**.

### 5. **Stack Segment**
- The **stack** is used to store local variables and function call information. The stack grows downwards (towards lower memory addresses).
- When `example_function()` is called, the local variable `local_var = 5` is stored in the **stack**. Once the function finishes, the stack unwinds, and `local_var` is discarded.
- The stack is also where function call information like return addresses and function parameters are stored. Every time a function is called, a new "frame" is added to the stack. When the function returns, the stack frame is popped off.
- Stack memory is managed automatically, meaning the OS cleans up when a function completes. However, if too much memory is used in the stack (like with deeply nested recursive functions), you could run into a **stack overflow**.

### 6. **Kernel Space**
- The **kernel space** is reserved for the operating system, and user programs cannot access it. If a program attempts to access this memory, it will cause a **segmentation fault**.
- This separation ensures that user programs cannot accidentally or maliciously interfere with the core functions of the operating system.

### Memory Layout Summary:
```
+-------------------+ 0xFFFFFFFF
|  Kernel Space     | (inaccessible to user programs)
+-------------------+ 
|  Stack            | (local variables, function call info - grows downward)
+-------------------+ 
|  Heap             | (dynamically allocated memory - grows upward)
+-------------------+ 
|  BSS              | (uninitialized global/static variables)
+-------------------+ 
|  Data             | (initialized global/static variables)
+-------------------+ 
|  Text             | (compiled program code - read-only)
+-------------------+ 0x00000000
```

### Key Points:
1. **Local Variables (Stack)**: Temporary data used within a function. They're created when the function is called and destroyed when the function returns.
2. **Global Variables (Data/BSS)**: Persist throughout the program's execution. Global variables that are initialized go in the data segment, while uninitialized ones go in the BSS.
3. **Dynamically Allocated Memory (Heap)**: Used when you don’t know how much memory you’ll need at compile time. It’s up to the programmer to free this memory.
4. **Text Segment**: Contains the program’s instructions and is read-only for protection.

This structure helps the operating system manage memory efficiently and ensures proper execution of processes.

Let me know if you need any more clarifications!

## Memory Example

Here's an example of how different memory segments are used in a simple C program:

```c
#include <stdio.h>
#include <stdlib.h>

// Initialized global variable - Data Segment
int global_var = 10;

// Uninitialized global variable - BSS Segment
int uninitialized_var;

void example_function() {
    // Local variable - Stack Segment
    int local_var = 5;
    printf("Local variable: %d\n", local_var);
}

int main() {
    // Dynamically allocated memory - Heap Segment
    int *heap_var = (int *)malloc(sizeof(int));
    *heap_var = 20;

    // Print global and dynamically allocated variables
    printf("Global variable: %d\n", global_var);
    printf("Dynamically allocated variable: %d\n", *heap_var);

    // Call function to use local variable
    example_function();

    // Free dynamically allocated memory
    free(heap_var);

    return 0;
}
```

In this example:
- The **global_var** is stored in the Data Segment.
- The **uninitialized_var** is stored in the BSS Segment.
- **local_var** is stored in the Stack Segment.
- **heap_var** is dynamically allocated in the Heap Segment.

---

## Process Lifecycle

A process goes through several states in its lifecycle, managed by the operating system. These states are:

- **New**:
  - The process is being created.
  - The operating system allocates necessary resources, like memory, to the process.

- **Ready**:
  - The process is ready to run but is waiting for CPU time.
  - It has everything it needs to execute, except CPU access.

- **Running**:
  - The process is actively being executed by the CPU.
  - Instructions are executed in this state.

- **Waiting (Blocked)**:
  - The process is waiting for some event to occur (e.g., I/O operation).
  - It cannot proceed until the event is completed.

- **Terminated**:
  - The process has completed its execution.
  - The operating system cleans up the resources allocated to the process.


## Process Lifecycle Example

A process goes through several states in its lifecycle, managed by the operating system. Consider the example of a **web browser** process. When you open the browser, it undergoes the following stages:

### New:
- The web browser is being created.
- The operating system allocates necessary resources, like memory, to the process.
- **Example**: When you click to open the browser, the process is created, and the operating system starts setting up resources like memory.

### Ready:
- The web browser is ready to run but is waiting for CPU time.
- It has everything it needs to execute, except CPU access.
- **Example**: The browser is loaded into memory and ready to run, but the CPU is currently working on other tasks, so the browser waits in the ready queue.

### Running:
- The web browser is actively being executed by the CPU.
- Instructions are executed in this state.
- **Example**: Once the CPU is free, it starts executing the browser’s instructions, rendering the interface and handling user inputs.

### Waiting (Blocked):
- The web browser process is waiting for an event to occur (e.g., loading a webpage from the internet).
- It cannot proceed until the event is completed.
- **Example**: When you click on a link, the browser may wait for data from the web server. During this time, it’s not using the CPU but is waiting for an I/O event (network data).

### Terminated:
- The web browser process has completed its execution (e.g., you closed the browser).
- The operating system cleans up the resources allocated to the process.
- **Example**: When you close the browser, the process terminates, and the OS releases the memory and other resources that were allocated to it.


---

## Context Switching

### Definition:
Context switching is the process of saving the state of a currently running process and loading the state of another process to allow multitasking.

### Purpose:
It enables the CPU to switch between multiple processes, sharing CPU time and ensuring all processes make progress.

### Steps Involved:
- Save the current process state (program counter, CPU registers, etc.) into its Process Control Block (PCB).
- Load the state of the new process from its PCB.

### Overhead:
Context switching introduces some overhead because the CPU spends time saving and loading process states instead of executing instructions.

### Triggers:
- Context switching occurs due to interrupts (e.g., I/O completion).
- It happens during CPU scheduling to give other processes time on the CPU.

### Importance:
Ensures effective multitasking and allows the system to run multiple processes concurrently.

## Context Switching Example

Imagine you are using a **web browser** and also have a **text editor** like Microsoft Word open. Both of these applications need CPU time to execute their instructions, but your computer's CPU can only execute one instruction at a time. To make it seem like both applications are running simultaneously, the operating system uses **context switching**.

### Example:

1. **Web Browser Running**: 
   - The CPU is currently executing the web browser process. You're watching a video in your browser, and the CPU is rendering the video frames.
   
2. **Context Switch to Text Editor**: 
   - Suddenly, you switch to Microsoft Word to continue typing a document. 
   - The CPU performs a **context switch**: it saves the current state of the web browser (like the program counter, registers, etc.) into the **Process Control Block (PCB)** and loads the saved state of the text editor from its PCB.

3. **Text Editor Running**: 
   - Now, the CPU is executing the text editor, allowing you to type.
   
4. **Context Switch Back to Web Browser**: 
   - After typing for a few seconds, the video in the web browser needs to continue playing.
   - The CPU performs another context switch: it saves the current state of the text editor into its PCB and reloads the web browser's state from its PCB to continue executing the video rendering.

In this way, the CPU switches back and forth between processes, making it seem like both are running simultaneously, even though only one process is active at any given time.

### Overhead:
Every time a context switch occurs, the CPU spends time saving and loading process states, which adds a small amount of overhead.

This is how context switching allows multitasking between multiple processes running on your computer.
---

## User Mode vs Kernel Mode

### User Mode:
- **Definition**: User mode is where regular applications and programs run. It has limited access to system resources.
- **Restricted Access**: Programs in user mode cannot directly access hardware or critical memory. If they need to, they must make a system call, which requests the kernel to perform privileged tasks.
- **Crash Isolation**: If an application crashes in user mode, it does not affect the rest of the system. The operating system remains stable.
- **Examples**: Running programs like browsers, text editors, and user applications.

### Kernel Mode:
- **Definition**: Kernel mode is where the operating system’s core functions run. It has full access to system hardware and memory.
- **Full Access**: The kernel can directly access hardware, manage memory, handle device drivers, and control system processes.
- **Critical Role**: Because of its full access, any errors in kernel mode (such as a bug in a driver) can cause the entire system to crash.
- **Examples**: System processes, device drivers, memory management, and hardware-level operations.

### Key Differences to Remember:
1. **Access Level**:
   - **User Mode**: Limited access to system resources.
   - **Kernel Mode**: Full access to all system resources.

2. **System Calls**:
   - Programs in **user mode** must request access to resources through system calls.
   - **Kernel mode** processes these system calls.

3. **Stability**:
   - Crashes in **user mode** are isolated to the application.
   - Crashes in **kernel mode** can bring down the entire system.

---

