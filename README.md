# Concurrent and Distributed Systems

## Introduction
A system that performs several activities at the same time is called a **concurrent system**.

On the other hand, a **distributed system** is a particular case of concurrent system, when multiple machines contribute to performing a task, behaving as a single machine from the user perspective.

Some systems are inherently concurrent, such as operating systems, as they need to at least to be able to run the OS itself and another (or more) program.

## Processes
A process is an abstraction for the OS of a **program** in execution (code + variables + CPU access).

Running processes in parallel (**multitasking**) makes the resources more reusable (virtual memory, CPU cycles management).

Without parallel running programs in parallels, a computer would need a CPU for each program in execution.

### Process Life-cycle

![Process Life-cycle](http://academic.udayton.edu/SaverioPerugini/courses/cps346/lecture_notes/images/procstates.png)

A process can be on one of the following states:

- **new** ─ The process has been created.
- **ready** ─ The process is waiting to be assigned.
- **running** ─ The program's instructions are being executed.
- **blocked** ─ The process is waiting for an event to occur.
- **done** ─ The process has finished execution.

### Context Switching
**Context switching** is the process of storing the state of a _process_ or of a _thread_, so that it can be restored and execution resumed from the same point later. 

This allows multiple processes to share a single CPU, and is an essential feature of a multitasking operating system. 

Context switching adds an _overhead_ to the execution of process, the OS spends time switching the context, and is not executing any of the process.

### Creating child process
Each process is spawned from a parent process, creating a tree of processes.

Terminating the parent process will cause children processes to be terminated too.

-- TODO What do they share?

Example of spawning a child process in C++ using the Linux `fork()` function:

```c++
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[])
{
  int ppid = getpid(); // the parent process id
  
  printf("parent pid: %d\n", ppid);
  
  int pid = fork(); // returns the child process id

  // Failed to fork process
  if(pid < 0) {
    fprintf(stderr, "Fork failed\n");
    exit(-1);
  }

  // If run from the child process
  if(getpid() != ppid){
    printf("child pid: %d\n", getpid());
    exit(0);
  }

  // Wait for the other process to finish
  wait(NULL);
  printf("Child completed\n");
}
```

### Cooperating Processes
Separate processes do not share the same address space, so they cannot share memory directly, so some sort of Inter-Process Communication (**IPC**) must be established. Usually the OS provide IPC support, alternatively it can be implemented directly by the developer.

In order for processes to communicate they need to be synchronized in order to prevent data inconsistency (i.e. process A reading while process B is writing the same file). There is a whole set of Race Conditions that needs to be handled in order to prevent inconsistencies.

### Threads

Most operating systems allow a process to have multiple thread of execution. A thread is like a process, but shares memory space and resources with the other threads in the same process. This makes thread switching and thread scheduling less cost intensive than the process equivalents.

Benefits of using threads are:

- **Responsiveness** – an application continues running even if part of it is blocked or performing a lengthy operation.
- **Resource sharing** – Threads share memory and resources of the process they belong to.
- **Economy** – allocating memory and resources to processes is costly. Creating and switching between threads is more cost effective as the overhead is smaller.
- **Utilisation** of multiprocessor architectures – each thread may run on a different processor. In a single processor environment, the context switching allows pseudo parallelism.

Threads have states like processes.

#### Multi-threading models
Different operating systems provide different approach to multi-threading, and how each thread communicates with the kernel.

Operating systems usually fall in one the three categories:

- **Many-to-One** ─ All the user-level threads communicate with a single kernel-level thread. This causes each thread to be scheduled and does not support multiple cores.
- **One-to-One** ─ A kernel-level thread is created for each user-level thread. This allows a better use of multiple cores, but has a greater overhead, as creating a new user-space thread will create also a new kernel-space thread.
- **Many-to-Many** ─ The number of kernel threads depend on the machine, and each user-level thread is assigned to one of the existing kernel-threads. This has a lower overhead than the _one-to-one_ and supports multiple cores.

## Concurrency


## Distributed Systems

