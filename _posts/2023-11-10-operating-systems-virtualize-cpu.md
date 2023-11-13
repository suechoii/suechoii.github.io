---
layout: post
title: Operating Systems - Virtualize CPU
category: [Operating Systems]
date: 2023-11-10 17:19 +0800
---
**Process Control**
- Restricted Operations : *System Calls*
- Virtualize the CPU : *Context Switching*
- Voluntary Release of CPU : *System Calls*
- Involuntary Release of CPU : *Interrupt*

**Requesting OS Services**
- System calls allow the kernel to carefully expose certain key services to applications  > Restricted operations in user mode
- Mostly accessed by programs via a high-level API rather than directly invoking the system call 
    - Caller does not need to know how the system call is implemented or invoked

**System Call Implementation**
- Typically, the application calls *getpid()*, causing a "trap", resulting in transfer control to the kernel
- Before "trap" into the kernel, either the hardware or system call function must save enough process's register context in order to be able to return correctly. 
- Each system call is associated with a number, and the number is passed by the function call to the kernel.
- Within the kernel the system-call implementation maintains a table indexed by these numbers. 
- Each entry consists of an address that points to the corresponding system function in the kernel. 
- When finished, call a special return-from-trap instruction to return into the calling user program

**Virtualizing the CPU**
- The mechanism behind CPU Virtualization is *time-sharing* the CPU - temporarily stop a process, run another process, resume the stopped process, ...
- To stop a process, one just needs to remove it from running in CPU (to ready state)
    - OS needs to save the register context of current running process before stopping it
    - To resume, OS needs to restore its register context back to CPU registers beore resuming it
- Context Switch is the switching of the register context of one process to another. 

**Context Switch**
- Save the "current" process's register context to the process's kernel stack
    - save the general purpose registers, PC, kernel stack pointer of the currently-running process, and then restore the registers, PC, and switch to the kernel stack for the soon-to-be-resumed process. 
- OS must minimize context-switching time to reduce overheads 
- Difference with Mode Switch : 
    - Mode Switch switches from user mode to kernel mode, and the kernel is said to be "executing on behalf of the process" 
    - For Mode Switch, upon exiting kernel, the process may resume and return to execute in user space 
    - In Context Switching, the kernel must suspend the progression of current process and store its context, retrieve the context of another process + restore it to CPU , resume execution of another process.

**Regaining control of CPU**
- There are two methods: Involuntary(Passive) and Voluntary(Active)
- Passive Approach : OS just waits for a process to make system calls, in worst case, a process can get into an infinite loop
- Active Approach : Timer Interrupt is raised, and OS will be called in to handle it
    - Interrupts may be triggered by a running process (exeception), which is synchronous with the operation of the current process + within processor, e.g. dividing by zero
    - If not related to the running process, it is asynchronous with the operation of the current process + outside processor, e.g. key is pressed

**Interrupt Handling**
- The processor responds to interrupts and exceptions in essentially the same way.
- CPU has to save some register contents when interrupt occurs.
- After receiving an interrupt, the processor completes execution of the current instruction.
- Interrupt handlers are stored in an array of function pointers called the interrupt vector, which the OS sets up at boot time.
- CPU reads from the system bus the interrupt vector number provided by the interrupt controller, which is then used as an index to locate the interrupt handler.
- OS has to save remainder of the process state somewhere, hence jumps to interrupt handler for it to process the interrupt
- After completion of interrupt handling, a decision has to be made by the Scheduler - restore the interrupted process or switch to a different one -> if switch, then comes *context switching*