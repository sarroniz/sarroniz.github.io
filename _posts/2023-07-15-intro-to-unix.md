---
layout: post
title: Introduction to Unix
date: 2023-07-15 15:09:00
description: A basic tutorial to Unix for Computational Linguistics
tags: CL tutorial
categories: 
featured: true
---

1. **Introduction to Unix:**
   - Unix is a powerful and versatile operating system known for its command-line interface (CLI) and multitasking capabilities.
   - Key features of Unix include file management, process control, networking, and security.

2. **Terminal and Shell:**
   - Unix commands are executed in a terminal, which provides a text-based interface.
   - The terminal runs a shell, such as Bash (Bourne Again SHell), where you type commands.

3. **Navigating the File System:**
   - Use the `ls` command to list files and directories in the current location.
   - Use `cd` to change directories (e.g., `cd /path/to/directory`).
   - Use `pwd` to print the current working directory.

4. **File and Directory Operations:**
   - Create a new directory: `mkdir directory_name`.
   - Create a new empty file: `touch file_name`.
   - Copy files/directories: `cp source destination`.
   - Move files/directories: `mv source destination`.
   - Remove files: `rm file_name`.
   - Remove directories: `rm -r directory_name` (be cautious with this command).

5. **Working with Files:**
   - View file content: `cat file_name` or `less file_name`.
   - Edit files: Use the text editor `vi` or `nano` (e.g., `nano file_name`).

6. **Redirection and Pipes:**
   - Redirect output: Use `>` to overwrite and `>>` to append (e.g., `echo "Hello" > output.txt`).
   - Redirect input: Use `<` to read from a file (e.g., `cat < input.txt`).
   - Use pipes `|` to send the output of one command as input to another (e.g., `ls | grep keyword`).

7. **File Permissions:**
   - Use `chmod` to change file permissions (e.g., `chmod 755 file_name`).
   - Permissions are represented as three sets of characters: owner, group, and others (e.g., `rwxr-xr-x`).

8. **Process Management:**
   - View running processes: `ps` or `top`.
   - Terminate a process: `kill PID` (replace PID with the process ID).

9. **User Management:**
   - Add a user: `sudo adduser username`.
   - Delete a user: `sudo deluser username`.
   - Change user: `su username`.

10. **Package Management (Linux-specific):**
    - Package managers install and manage software packages on Linux.
    - Common package managers include `apt` (Ubuntu, Debian), `yum` (Red Hat, CentOS), and `dnf` (Fedora).

11. **Getting Help:**
    - Use `man` to access the manual pages for commands (e.g., `man ls`).
    - Online resources like tutorials and forums are valuable for learning Unix.

Remember, Unix has a vast array of commands and features beyond what's covered here. This tutorial provides a foundation for getting started, but continued learning and practice will help you become proficient in Unix. Good luck!