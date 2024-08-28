---
layout: default
title: Week 1 - Intro
nav_order: 2
---

## Command Line and Server Basics
<br>
Hey there! Welcome to a guide on some essential command-line and server basics. This is essential for our class and assignments. I'll cover some of the most basic stuff here, but the best way to learn is by being curious and toying around. I may go a little fast today to cover everything, if we have time we can do another Vim exercise.

## Contents
1. [Operating Systems: The Heart of Your Computer](#operating-systems-the-heart-of-your-computer)
2. [Command Line: Your Computer's Secret Superpower](#command-line-your-computers-secret-superpower)
3. [Server: The Internet's Heavy Lifter](#server-the-internets-heavy-lifter)
4. [SSH: Your Secure Gateway](#ssh-your-secure-gateway)
   - [Using SSH](#using-ssh)
5. [Introduction to Shells](#introduction-to-shells)
6. [Using and Understanding Man Pages](#using-and-understanding-man-pages)
7. [System Monitoring and Performance Tuning](#system-monitoring-and-performance-tuning)
   - [Exercise: Working with `top` and `htop`](#exercise-working-with-top-and-htop)
8. [Using Vim in Terminal](#using-vim-in-terminal)
   - [Exercise: Coding a Simple C Program in Vim](#exercise-coding-a-simple-c-program-in-vim)
9. [Coding in C](#coding-in-c)
10. [Useful Command List to Know](#useful-command-list-to-know)
   - [Basic File Operations](#basic-file-operations)
   - [Viewing and Editing Files](#viewing-and-editing-files)
   - [Basic Networking Commands](#basic-networking-commands)
   - [Process Management](#process-management)
11. [Recap Time!](#recap-time)


## Operating Systems: The Heart of Your Computer

An **operating system (OS)** is the main software that runs on your computer. It manages all the hardware and software, making sure everything works together smoothly. Think of it as the computer's brain. There are a few popular operating systems you might have heard of:

- **Windows**: Developed by Microsoft, Windows is very popular and used by most PCs. It's known for its user-friendly interface and wide compatibility with software and games.
- **macOS**: Made by Apple, macOS is used on Mac computers. It's known for its sleek design, reliability, and seamless integration with other Apple products.
- **Linux**: An open-source OS that's popular among developers and tech enthusiasts. It's highly customizable and known for its security and performance.

In this lesson, we'll be working with **Linux commands**. Even if you're using Windows or macOS, don't worry! We'll connect to a Linux server to ensure everyone has access to Linux and can follow along.

## Command Line: Your Computer's Secret Superpower

The **command line** might look a bit intimidating at first, but it's like having a magic wand for your computer. Instead of clicking around, you type commands to tell your computer what to do. It's fast, powerful, and pretty cool once you get the hang of it!

### Key Commands

- **`cd`**: This stands for "change directory". Think of it as telling your computer, "Hey, let's go to this folder!" It's how you move around in your computer's file system.
- **`ls`**: This means "list". It shows you all the files and folders inside your current directory (or folder), kind of like opening a folder in your computer and seeing what's inside.

```bash
# Example usage:
cd Documents  # Let's go to the Documents folder
ls            # Show me what's inside!
```

Imagine you're in your home directory (your main folder). You want to check out what's in your Documents folder, so you type `cd Documents` to move into it, then `ls` to see all the files and folders in there. Easy peasy!

## Server: The Internet's Heavy Lifter

A **server** is like a super-strong computer that stores websites, files, and runs applications. Whenever you visit a website, you're actually talking to a server that sends you the website's data. Servers are always on, always ready, and handle tons of requests from people like you and me.

Think of a server as a huge library. When you request a book (a file or a webpage), the librarian (the server) finds it and hands it to you. Servers are what keep the internet running smoothly.

## SSH: Your Secure Gateway

**SSH** stands for Secure Shell, but you can think of it as a super safe way to connect to another computer (like a server) over the internet. It keeps everything encrypted, so no one can snoop on your connection.

### Using SSH

To use SSH, you type a command that looks something like this:

```bash
ssh username@hostname
```

- **`username`**: Your login name on the server.
- **`hostname`**: The address of the server, like an IP address or a website name.

```bash
ssh user@example.com
```

This command means, "Hey server at example.com, let me in! I'm user."

### SSH into CWRU servers 

This class is centered around the Linux operating system, so there may be times when using Mac or Windows isn't sufficient. To make things easier, you can connect to the **eecslab** Linux server. 

To connect, open your laptop's terminal and type:

```bash
ssh CASEID@eecslab-#.case.edu
```
CASEID@eecslab-#.case.edu: Replace CASEID with your username and # with the server number (1-4).
Once prompted, enter your password (it won't show for security reasons).

> **Important:**
> If one server crashes, try another number (1-4).

## Introduction to Shells

A **shell** is a program that takes your commands from the keyboard and gives them to the operating system to perform. The most common shell is **Bash** (Bourne Again SHell), but there are others like **Zsh** and **Fish**. Each shell has its own features and can be customized to fit your needs.

## Using and Understanding Man Pages

**Man pages** are the built-in help files for command-line tools. They provide detailed information on how to use a command, its options, and examples.

```bash
# Example usage:
man ls  # Display the manual page for the ls command
```

Using man pages is a great way to learn more about the tools available to you and how to use them effectively.

## System Monitoring and Performance Tuning

Monitoring your system's performance helps ensure it runs smoothly. Here are some tools you can use:

- **`top`**: Displays a dynamic view of your system's processes. It shows you which processes are using the most CPU, memory, and other resources.
- **`htop`**: An interactive version of `top` with more features, like the ability to scroll through the list of processes and sort by different criteria.
- **`iotop`**: Shows I/O usage by processes, helping you identify which processes are using the most disk input/output.

### What Does `top` Show?

The `top` command provides a real-time view of your system's performance. Here's a breakdown of what you'll see:

- **`PID`**: Process ID, a unique identifier for each running process.
- **`USER`**: The user who owns the process.
- **`PR`**: Process priority.
- **`NI`**: Nice value, which affects process priority.
- **`VIRT`**: Virtual memory used by the process.
- **`RES`**: Resident memory (physical memory) used by the process.
- **`SHR`**: Shared memory used by the process.
- **`S`**: Process status (e.g., running, sleeping).
- **`%CPU`**: CPU usage percentage.
- **`%MEM`**: Memory usage percentage.
- **`TIME+`**: Total CPU time used by the process.
- **`COMMAND`**: The command that started the process.

### Exercise: Working with `top` and `htop`

1. Open a terminal and run the `top` command:
   ```bash
   top
   ```
   - Observe the different columns and values. Identify which processes are using the most CPU and memory.

2. Install `htop` (if not already installed) and run it:
   ```bash
   sudo apt-get install htop  # For Debian-based systems
   sudo yum install htop      # For Red Hat-based systems
   htop
   ```
   - Use the arrow keys to navigate through the list of processes.
   - Press `F6` to sort by different criteria (e.g., CPU, memory).
   - Press `F9` to kill a process (be careful with this!).

By monitoring your system, you can ensure it runs efficiently and identify any potential issues.

## Using Vim in Terminal

**Vim** is a powerful text editor that's built into most Linux systems. It's a bit tricky to learn at first, but it's incredibly powerful once you get the hang of it.

### Why We Will Use Vim

We'll use Vim because it's available on almost all Unix-like systems, it's lightweight, and it's powerful enough for all kinds of text editing tasks. Plus, learning Vim will make you more efficient and versatile in your work.

### Basics of Vim

Here are some basic commands to get you started with Vim:

- **`i`**: Enter insert mode (to start editing the text).
- **`Esc`**: Exit insert mode.
- **`:w`**: Save the file.
- **`:q`**: Quit Vim.
- **`:wq`**: Save and quit.
- **`:q!`**: Quit without saving.

```bash
# Example usage:
vim filename  # Open a file in Vim
```

### Exercise: Coding a Simple C Program in Vim

Let's create a simple C program that prints "Hello, World!" to the terminal.

1. **Open a Terminal** and create a new file named `hello.c` using Vim:
   ```bash
   vim hello.c
   ```

2. **Enter Insert Mode** by pressing `i`.

3. **Type the Following C Code**:
   ```c
   #include <stdio.h>

   int main() {
       printf("Hello, World!\n");
       return 0;
   }
   ```

4. **Exit Insert Mode** by pressing `Esc`.

5. **Save the File** by typing `:w` and pressing `Enter`.

6. **Quit Vim** by typing `:q` and pressing `Enter`.

### Compiling and Running Your C Program

1. **Compile Your C Program** using `gcc` (GNU Compiler Collection):
   ```bash
   gcc hello.c
   ```

   This command compiles `hello.c` and creates an executable named `a.out` by default. You can change the executable name by specifying a different name after the `-o` option. 

2. **Run Your Program**:
   ```bash
   ./a.out
   ```

   You should see the output:
   ```
   Hello, World!
   ```

By practicing this exercise, you'll get comfortable using Vim for editing and writing code, as well as compiling and running C programs in a Unix-like environment. Happy coding!

## Coding in C

In this class, we will be coding in **C**. C is a powerful, efficient, and widely-used programming language that's particularly popular for system-level programming. It gives you a lot of control over the system resources and memory, which is why it's often used for operating system development.

### Why We Are Coding in C

- **Performance**: C is known for its speed and efficiency, making it ideal for performance-critical applications.
- **Low-level Access**: C provides low-level access to memory and system processes, which is essential for understanding how operating systems work.
- **Portability**: C code can run on virtually any machine with

 minimal changes.
- **Foundation for Other Languages**: Many modern programming languages are based on or influenced by C, so learning C provides a strong foundation for learning other languages.

### Things to Be Careful With When Coding in C

**Hint**: These could be exam questions, and we will explore these concepts in later classes. Just wanted to mention them here (don't worry if this makes no sense right now!)

- **Memory Management**: Unlike some higher-level languages, C requires you to manually manage memory (e.g., using `malloc` and `free`). Be careful to avoid memory leaks and segmentation faults.
- **Pointer Arithmetic**: Pointers are powerful but can be tricky. Incorrect pointer arithmetic can lead to bugs that are hard to track down.
- **Buffer Overflows**: Always ensure that you don't write more data to a buffer than it can hold. This can lead to crashes or security vulnerabilities.
- **Undefined Behavior**: Certain operations in C can lead to undefined behavior if not handled correctly, such as dividing by zero or using uninitialized variables.
- **Code Readability**: C code can become difficult to read and maintain if not well-documented and structured. Always use clear and consistent naming conventions and comment your code.

## Useful Command List to Know

### Basic File Operations

- **`touch`**: Create an empty file.
- **`mkdir`**: Make a new directory (folder).
- **`rm`**: Remove a file.
- **`cp`**: Copy files and directories.
- **`mv`**: Move or rename files and directories.

### Viewing and Editing Files

- **`cat`**: Display the contents of a file.
- **`nano`**: A simple text editor.
- **`vim`**: A more advanced text editor.
- **`less`**: View the contents of a file page by page.

### Basic Networking Commands

- **`ping`**: Check the network connection to another host.
- **`ifconfig`**: Display network configuration.
- **`wget`**: Download files from the internet.
- **`curl`**: Transfer data from or to a server.

### Process Management

- **`ps`**: Display current processes.
- **`top`**: Display and manage running processes.
- **`kill`**: Terminate a process by its PID.
- **`htop`**: An interactive process viewer.

## Recap Time!

- **Operating Systems**: The main software that runs your computer. We'll focus on Linux.
- **Command Line**: A text-based way to talk to your computer.
  - **`cd`**: Change directory (move to a different folder).
  - **`ls`**: List files and directories (see what's in the folder).
- **Server**: A powerful computer that stores and runs things for other computers.
- **SSH**: A secure way to connect to another computer over the internet.
- **Introduction to Shells**: Understanding what a shell is and its role.
- **Using and Understanding Man Pages**: How to use built-in help files.
- **System Monitoring and Performance Tuning**: Tools to keep your system running smoothly.
- **Using Vim in Terminal**: A powerful text editor that's essential for coding in a terminal environment.
- **Coding in C**: Why we're using C, and important considerations for writing C code safely and effectively.
- **Useful Commands**: Key commands for basic file operations, viewing and editing files, basic networking, and process management.

And that's it! You now know some of the basics to navigate your computer, understand how servers work, and start coding in C. Keep practicing.

