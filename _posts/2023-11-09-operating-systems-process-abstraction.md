---
layout: post
title: Operating Systems - Process Abstraction
category: [Operating Systems]
date: 2023-11-09 18:54 +0800
---

**Process vs Program**
- A process is an instance of a program in execution , that can be assigned to and executed on a CPU 
- To represent a running program, the OS needs to keep track of :
    i) Memory 
    ii) Resources in use 
    iii) Execution State 
- The process's view of its memory is called the **address space** 
![diagram](/assets/img/AddressSpace.png)
- In the process <u> execution life cycle </u>, it moves through a series of process states : 
1. **New (Initial)** : The process has just been created. Switch to <u>Ready</u> once its associated data structures have been created.
2. **Ready** : The process is **ready to run** on a processor and **is waiting** to be assigned to a processor. Once it's dispatched to be executed by the CPU (scheduled), it transitions to <u>Running</u> state.
3. **Running** : The process is **executing** on a processor. From here, there are three possible transitions - Terminated, Ready, Blocked. 
    (i) Terminated - when process goes to completion
    (ii) Ready - when process is descheduled
    (iii) Blocked - when process blocks waiting for resources (e.g. I/O)
4. **Blocked** : The process is **waiting** for some event to happen. Once the event occurs, it transitions to Ready state.
5. **Terminated** : The process has finished execution but has **not yet been cleaned up**. This allows parent process to examine the return code of the process and see if it executed succesfully. 

**Process Control Block (PCB)**
- A data structure to maintain information about a process
- Typically consists of : 
    - **Process Identification Number (PID)** : A unique ID
    - **Current Process State**
    - **Program Counter** : Indicates the address of next instruction
    - **Register Context** : A snapshot of the register contents in which the process was last running before it transitioned out of the running state 
    - **Scheduling Information** : Process Priority, Pointers to scheduling queues, etc
    - **Credentials** : Determines the resources this process can access
    - **Memory Management Information** : Memory areas allocated to the process
    - **Accounting Information** : CPU usage statistics, time limits, etc
    - A pointer to the proces's parent process, pointers to the process's child processes, pointers to allocated resources

**Process Table**
- To manage many processes, OS needs someway to quickly access to process's PCB
- OS keeps **pointers** to each process's PCB in a table 
- When a process is "completely" terminated, OS removes the process from the process table and frees all of the process's resources. 

- OS also maintains a **ready list** and a **blocked list** that store references to processes not currently running 

**ps**
- ps command can be used to list the processes' details 
- ps [option] -> some useful options include : 
    > -e : select all processes
    > -f : show in full format
    > w : wide output
    > f : ASCII-art process hierarchy

**pstree**
- pstree command is used to show the relations between processes 
- Since every process has a parent process, this command can be used to view the relationships 

**Process Creation**
- **Parent Process** : the process that creates a new process 
- **Child Process** : the newly created process 
- Steps taken to create a process : 
1. Assign a unique process ID, allocate memory for the process (+PCB)
2. Initialize the PCB by setting the PC and stack pointer to appropriate values
3. Set links so it is in the appropriate queue
4. Create other data strucutres : memory, files, accounting
5. Set the process state to Ready state and put it in the Ready queue. 

- Creating a process in Unix : 
    - fork() : A system call that creates a new process by duplicating the calling process
    - exec() : OS replaces the current program image with a new program image

**Process Termination**
- Process executes last statement and asks the operating system to delete itself 
- A process may terminate involuntarily - when the parent sends a termination signal to terminate the child process / a number of error and fault conditions
- Return termination status from child to parent 
    - parent can obtain this info by calling **wait()**, **waitid()** or **waitpid()**
    - process' resources are de-allocated by OS afterward
- **Zombine Process** : A process in terminated state , before the parent collects information 

**Process Suspension**
- Temporarily deactivating a process due to external activity: i) user request , ii) request by parent , iii) OS decision to suspend a blocked process to free up memory for another ready process
- A suspended process **must be** resumed by another process
- Different from *blocked*, as blocking is triggered by internal activity 

**Signals**
- Used in UNIX systems to notify a process that a particular event has occurred - Software Interrupt 
- Basically implemented as system calls (kill(), signal(), sigaction(), raise(), pause(), sigsuspend(), etc)
- Each signal is represented by a value / symbolic name : 
    > SIGINT : value = 2,  Ctrl + C
    > SIGCHLD : value = 17, when chid process finishes execution or is terminated
- A signal is generated by one software entity (in the occurrence of an event) to a target software entity
    - **Synchronous signal** : triggered by the current instruction of the current running process itself and is delivered to that process by the OS immediately 
    ![diagram](/assets/img/SynchronousSignal.png)
    - **Asynchronous signal** : generated by external events / activities, which are not triggered by the current activity / action of the target process at the time of receiving the signal 
    ![diagram](/assets/img/AsynchronousSignal.png)
- A process can decide whether it wants to catch, ignore or mask a signal 
    - Catching a signal involves specifying a routine (signal handler) in advance so that the OS will invoke that handler when the process receives that signal 
    - **Catching** : Using OS's default action to handle the signal 
    - **Ignore** : Inform OS that it does not want to handle the signal
    - **Masking** : Instruct the OS not to deliver signals of that type until the process clears the signal mask 
    * SIGKILL and SIGSTOp cannot be caught, blocked, or ignored
- A process's PCB contains a pointer to a vector of signal handlers - a child process inherits the setting from its parent 
