[GeeksforGeeks - Operating System Interview Questions](https://www.geeksforgeeks.org/operating-systems/operating-systems-interview-questions/)

[University of Cambridge - Operating Systems Slides](http://cl.cam.ac.uk/teaching/1011/OpSystems/os1a-slides.pdf)


# Operating Systems

## Process

### Process Control Block (PCB)

process control block (PCB) is a block that is used to track the process’s execution status. 

A process control block (PCB) contains information about the process, i.e. registers, quantum, **priority**, etc

### process table

process table is an array of PCBs, that means logically contains a PCB for all of the current processes in the system

### States of the process: 

running, ready, or waiting



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

### Paging

Memory management method accustomed fetch processes from the secondary memory into the main memory in the form of pages

Page: secondary memory area unit

Frames: main memory area unit

Paging in action:
1. A process needs data → checks page table → not in RAM?

1. Page fault occurs → OS loads page from secondary memory (disk).

1. If RAM is full, the OS may swap out another page to make room.

1. The new page is loaded into main memory, and execution resumes

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

### Preemptive and non-preemptive scheduling

- Preemptive scheduling, the CPU is allocated to the processes for a limited time
- Non-preemptive scheduling, the CPU is allocated to the process till it terminates or switches to waiting for the state. 

### starvation and aging

Starvation: Starvation is a resource management problem where a process does not get the resources it needs for a long time because the resources are being allocated to other processes.

Aging: Aging is a technique to avoid starvation in a scheduling system. It works by adding an aging factor to the priority of each request. The aging factor must increase the priority of the request as time passes and must ensure that a request will eventually be the highest priority request


## Multi-programming

Multi-programming increases CPU utilization by organizing jobs (code and data) so that the CPU always has one to execute. The main objective of multi-programming is to keep multiple jobs in the main memory. 

## Multitasking

a logical extension of a multiprogramming system that supports multiple programs to run **concurrently**

In multitasking, more than one task is executed at the same time.

## time-sharing system

Time-sharing is a logical extension of multiprogramming. The CPU performs many tasks by switches that are so frequent that the user can interact with each program while it is running.

## 🧵 synchronization techniques - Multithreaded Programming Concepts

- Mutexes
- Condition variables
- Semaphores
- File locks

### concurrency

A state in which a process exists simultaneously with another process than those it is said to be concurrent


### 🔐 Mutex (Mutual Exclusion)
A **mutex** is a locking mechanism used to ensure that only **one thread** can access a **critical section** of code at a time. It prevents **race conditions** by enforcing exclusive access to shared resources like memory, files, or database connections.

#### 🖥 Example: Two programs writing to the same file

- Suppose two threads are trying to write to a **log.txt** file.
- Without a lock, their messages **get jumbled** or overwrite each other.
- With a mutex, only one thread writes at a time — the other waits.

```python
from threading import Lock

log_lock = Lock()

def write_log(message):
    with log_lock:
        with open("log.txt", "a") as f:
            f.write(message + "\n")
```

### ⚔️ Race Condition
A **race condition** happens when two or more threads access shared data simultaneously, and the result depends on the order of execution. This often leads to **unexpected behavior** or bugs in concurrent systems.

### 🧵 Thread Safety
A piece of code is **thread-safe** if it functions correctly when accessed by multiple threads at the same time, usually by using mutexes, locks, or atomic operations to prevent shared data corruption.

### 🔄 Deadlock
A **deadlock** occurs when two or more threads are **waiting on each other to release locks**, and none of them can proceed. It's often caused by poor ordering of resource acquisition.

Example:

- Thread A locks File1, then tries to lock File2.

- Thread B locks File2, then tries to lock File1.

- Now both threads are waiting on each other. Deadlock!

✅ Used to teach:

- Resource acquisition order matters

- Avoiding circular wait

- Deadlock prevention strategies (timeouts, lock hierarchy)

**How to recover from a deadlock**:

- Process termination
    - Abort all the deadlock processes
    - Abort one process at a time until the deadlock is eliminated
- Resource preemption
    - Rollback
    - Selecting a victim


```python
import threading

lock1 = threading.Lock()
lock2 = threading.Lock()

def thread_a():
    with lock1:
        with lock2:
            print("Thread A")

def thread_b():
    with lock2:
        with lock1:
            print("Thread B")
```

### 🚦 Semaphore
A **semaphore** is a counter-based lock that allows **a fixed number of threads** to access a resource. It’s useful when limiting concurrent access, like in connection pools.

Examples:
* Your web server can only handle 10 simultaneous DB connections.

```python
from threading import Semaphore

db_semaphore = Semaphore(10)

def access_database():
    with db_semaphore:
        # safe to connect and query the DB
        pass
```

#### advantages of semaphores

- They are machine-independent.
- Easy to implement.
- Correctness is easy to determine.
- Can have many different critical sections with different semaphores.
- Semaphores acquire many resources simultaneously.
- No waste of resources due to busy waiting.

---

### 🎯 Sample Interview Answer (Bringing It All Together)
"Multithreading helps with parallel execution, but it introduces challenges like race conditions. To handle this, we use mutexes to protect critical sections and ensure thread safety. I've used mutexes in scenarios like synchronizing access to a shared cache or log file. I also watch out for deadlocks by keeping lock acquisition order consistent."

### Banker's algorithm

a resource allocation and deadlock avoidance algorithm that tests for safety by simulating the allocation for the predetermined maximum possible amounts of all resources

## What problem we face in computer system without OS?

- Poor resource management
- Lack of User Interface
- No File System
- No Networking
- Error handling

## RAID - Redundant Array of Independent Disks

RAID stands for Redundant Array of Independent Disks. It's a technique that combines multiple physical drives into a single logical unit to improve data redundancy, performance, or both. In an SRE context, it's important for ensuring storage reliability and fault tolerance—especially in systems where uptime and data integrity are critical

## overlays

The process of transferring a block of program code or other data into internal memory, replacing what is already stored

A process is running it will not use the complete program at the same time, it will use only some part of it


## Caching

The cache is a smaller and faster memory that stores copies of the data from frequently used main memory locations

## CPU components

| Component       | Function                                |
|-----------------|---------------------------------------|
| Control Unit    | Manages and controls CPU operations   |
| ALU             | Performs calculations and logic       |
| Registers       | Fast temporary data storage            |
| Cache           | Speeds up access to frequently used data|
| Buses           | Transfer data and signals              |
| Clock           | Synchronizes operations                |

## Bootstraping

Bootstrapping is the process of loading a set of instructions when a computer is first turned on or booted.


## Inter-process communication (IPC) 

mechanism that allows processes to communicate with each other and synchronize their actions

methods in IPC:

- Pipes (Same Process): This allows a flow of data in one direction only. Analogous to simplex systems (Keyboard). Data from the output is usually buffered until the input process receives it which must have a common origin.
- Named Pipes (Different Processes): This is a pipe with a specific name it can be used in processes that don’t have a shared common process origin. E.g. FIFO where the details written to a pipe are first named.
- **Message Queuing**: This allows messages to be passed between processes using either a single queue or several message queues. This is managed by the system kernel these messages are coordinated using an API.
- **Semaphores**: This is used in solving problems associated with *synchronization* and *avoiding race conditions*. These are integer values that are greater than or equal to 0.
- **Shared Memory**: This allows the interchange of data through a defined area of memory. **Semaphore** values have to be obtained before data can get access to shared memory.
- Sockets: This method is mostly used to communicate over a network between a client and a server. It allows for a standard connection which is computer and OS independent


## Context Switching

Switching of CPU to another process means **saving the state** of the old process and loading the saved state for the new process. In Context Switching the process is stored in the Process Control Block to serve the new process so that the old process can be *resumed from the same part it was left*.

## kernel

### Monolithic kernel

Structure: \ 
All OS services (device drivers, file system, etc.) run in the same address space (kernel space)

### Microkernel

Structure:
Only minimal functionalities (e.g., memory management, process scheduling) run in kernel space. Other services (like file systems, device drivers) run as user-space processes

## Linux File System

A file is a collection of related information that is recorded on secondary storage (SSD).

From the user’s perspective, a file is the **smallest** allotment of logical secondary storage

| Directory | Purpose | Real-Life Analogy or Example |
|-----------|---------|------------------------------|
| `/`       | Root of the filesystem | Like the `C:\` drive in Windows |
| `/bin`    | Essential user binaries | `ls`, `cp`, `mv` – basic commands for all users |
| `/sbin`   | System binaries | `reboot`, `ifconfig` – mostly used by root/admin |
| `/etc`    | Configuration files | `/etc/passwd`, `/etc/ssh/sshd_config` |
| `/home`   | User home directories | `/home/eugene`, `/home/alex` – personal files and settings |
| `/root`   | Root user’s home directory | Admin’s private folder |
| `/lib`    | Shared libraries | Files like `libc.so.6` used by `/bin` and `/sbin` programs |
| `/opt`    | Optional/third-party software | `/opt/google/chrome` |
| `/var`    | Variable data like logs and emails | `/var/log/`, `/var/mail/`, `/var/www/` |
| `/tmp`    | Temporary files | Files deleted on reboot, used by many programs |
| `/usr`    | User applications and data | `/usr/bin/vim`, `/usr/share/locale` |
| `/boot`   | Bootloader and kernel files | `vmlinuz`, `initrd.img` |
| `/dev`    | Device files | `/dev/sda` (disk), `/dev/null`, `/dev/tty` |
| `/proc`   | Virtual filesystem for processes | `/proc/cpuinfo`, `/proc/1234/` (PID info) |
| `/sys`    | Kernel and hardware info | `/sys/class/net/`, `/sys/block/` |
| `/mnt`    | Manual mount point | Used when mounting drives or partitions temporarily |
| `/media`  | Auto-mounted external devices | `/media/usb0` (when USB is plugged in) |
| `/srv`    | Service-related data | Web/FTP content: `/srv/www/`, `/srv/ftp/` |
| `/run`    | Runtime data (e.g., PID files) | `/run/nginx.pid`, `/run/systemd/` |

### file allocation table (FAT)

a table that an operating system maintains on a hard disk that provides a **map** of the cluster (the basic units of logical storage on a hard disk) that a file has been stored in

## dispatcher

dispatcher is the module that gives process control over the CPU after it has been selected by the short-term scheduler

function involves the following:

- Switching context
- Switching to user mode
- Jumping to the proper location in the user program to restart that program


## CPU scheduling benefits

- Max CPU utilization [Keep CPU as busy as possible]Fair allocation of CPU.
- Max throughput [Number of processes that complete their execution per time unit]
- Min turnaround time [Time taken by a process to finish execution]
- Min waiting time [Time a process waits in ready queue]
- Min response time [Time when a process produces the first response]

