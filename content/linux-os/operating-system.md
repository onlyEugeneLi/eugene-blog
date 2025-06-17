[GeeksforGeeks - Operating System Interview Questions](https://www.geeksforgeeks.org/operating-systems/operating-systems-interview-questions/)

[University of Cambridge - Operating Systems Slides](http://cl.cam.ac.uk/teaching/1011/OpSystems/os1a-slides.pdf)


# Operating Systems

## Process

process table

States of the process

## Thread

> ℹ️ **Definition** \
> Segment of a process (application instruction or request)

### Differences between process and thread

**Process** is program under action and **thread** is the smallest **segment of instructions** (segment of a process) that can be handled independently by a scheduler.

### Benefits of multithreaded programming
* makes the system more responsive and enables resource sharing.
* leads to the use of multiprocess architecture
* Known as a lightweight process
* To achieve parallelism by dividing a process into multiple threads
* Threads within the same process run in shared memory space


## Thrashing

Thrashing is a situation when the performance of a computer degrades or collapses. 

Thrashing occurs when a system spends more time *processing page faults* than executing transactions.

What is demand paging? \ 
The process of loading the page into memory on demand (whenever a page fault occurs)

## Buffer

A buffer is a memory area that stores data being transferred between two devices or between a device and an application

## Virtual Memory

Modern operating systems use **virtual memory**, which allows programs to access more memory than is physically available in RAM. Pages of memory are stored on **disk** and swapped into RAM as needed.

* Page fault: A page fault occurs when a program tries to access memory that's not currently loaded in RAM, but resides in virtual memory (like the hard drive). This triggers the operating system to load the necessary page (a block of memory) from storage into RAM.

## The main purpose of an operating system

An operating system acts as an intermediary between the user of a computer and computer hardware. The purpose of an operating system is to provide an environment in which a user can execute programs conveniently and efficiently. 

An operating system is a software that manages computer hardware. The hardware must provide appropriate mechanisms to ensure the correct operation of the computer system and to prevent user programs from interfering with the proper operation of the system.

## Kernel

A kernel is the central component of an operating system that manages the operations of computers and hardware. It basically manages operations of memory and CPU time.

## Scheduling algorithms

1. First-Come, First-Served (FCFS) Scheduling
1. Shortest-Job-Next (SJN) Scheduling
1. Priority Scheduling
1. Shortest Remaining Time
1. Round Robin(RR) Scheduling
1. Multiple-Level Queues Scheduling

### Round Robin(RR) Scheduling

fix time for each job

## Multi-programming

Multi-programming increases CPU utilization by organizing jobs (code and data) so that the CPU always has one to execute. The main objective of multi-programming is to keep multiple jobs in the main memory. 

## time-sharing system

Time-sharing is a logical extension of multiprogramming. The CPU performs many tasks by switches that are so frequent that the user can interact with each program while it is running.

## What problem we face in computer system without OS?

Poor resource management
Lack of User Interface
No File System
No Networking
Error handling



