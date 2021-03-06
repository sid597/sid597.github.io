---
layout: post
category: distributed systems
title: Threads from the ground up
data: 2021-01-12
---

How are threads defined ?
What is a process ?
How are threads implemented ?

To understand threads I think it is important to understand the environment in which they are implemented. And to do so we need understand what a process is
and when we are discussing processes then operating systems will come up. With operating systems comes the hardware on which it is implemented, if we look around here then we will find nowadays processors also have threads. So where do we start ? Let's start from the basics, I will describe a hardware on which we will build the operating system, process abstraction and threds inside those processes.

This hardware is based on the [NandToTetris][n2t] course, I think this will get the point I am trying to make while being simple to understand and not deviating from the current implementation of threads. This hardware is going to be pretty simple and I will try to build upon it based on where we need it.


### KERNAL

Kernal is a virtual abstraction of the underlying hardware. 

 It is the "portion of the operating system code that is always resident in memory", and facilitates interactions between hardware and software components.

The critical code of the kernel is usually loaded into a separate area of memory, which is protected from access by application programs or other, less critical parts of the operating system. The kernel performs its tasks, such as running processes, managing hardware devices such as the hard disk, and handling interrupts, in this protected kernel space.

In contrast, application programs like browsers, word processors, or audio or video players use a separate area of memory, user space.

This separation prevents user data and kernel data from interfering with each other and causing instability and slowness, as well as preventing malfunctioning application programs from crashing the entire operating system.

Kernel, is not a process. It is a framework for running processes. In practice, a process is more than just a "program in execution". On a system with an MMU, a process is usually run in its own virtual address space. The kernel however, is usually mapped into all processes. It's always there.





###  The Process Abstraction
The word “process", like many terms in computer science, has evolved over time. The evolution of words
can sometimes trip up the unwary — systems built at different times will use the same word in significantly
different ways.
A “process" was originally coined to mean what is now called a “thread" — a logical sequence of instructions
that executes either operating system or application code. The concept of a process was developed as a
way of simplifying the correct construction of early operating systems that provided no protection between
application programs.
Organizing the operating system as a cooperating set of processes proved immensely successful, and soon
almost every new operating system was built this way, including systems that also provided protection
against malicious or buggy user programs. At the time, almost all user programs were simple, singlethreaded programs with only one program counter and one stack, so there was no confusion. A process was
needed to run a program, that is, a single sequential execution stream with a protection boundary.
As parallel computers became more popular, though, we once again needed a word for a logical sequence
of instructions. A multiprocessor program can have multiple instruction sequences running in parallel, each
with its own program counter, but all cooperating within a single protection boundary. For a time, these were
called “lightweight processes" (each a sequence of instructions cooperating inside a protection boundary),
but eventually the word “thread” became more widely used.
This leads to the current naming convention used in almost all modern operating systems: a process
executes a program, consisting of one or more threads running inside a protection boundary.


why do we need a process, what problem are we trying to solve ?

A process is the execution of an application program with restricted rights; the process is the abstraction for protected execution provided by the operating system kernel. A process needs permission from the operating system kernel before accessing the memory of any other process, before reading or writing to the disk, before changing hardware settings, and so forth. In other words, the operating system kernel mediates and checks each process’s access to hardware.

Lets say we are able to run multiple programs inside our OS but there is nothing stopping one program to write into memory address of other program, if this happens then it is possible to mess with other programs. So we have this abstraction of process where we give each process an illusion that they have all the memory to themselves and they can do whatever they want to do with it.



lock implementation:

int value = FREE;

Acquire() {
    disable interrupts;
    if (value == BUSY) {
        put thread on wait queue;
        Go to sleep();
        // Enable interrupts ?
    } else {
        value = BUSY;
    }
    enable interrupts;
}

Release() {
    disable interrupts;
    if (anyone on wait queue) {
        take thread off wait queue
        Place on ready queue;
    } else {
        value = FREE;
    }
    enable interrupts;
}

The anove soltuion does not work in multiprocessor so we need another solution
Use hardware instructions : Test&Set instruction


test&set(&address) {
    result = Memory[addreess];
    Memory[address] = 1;
    return result;
}

We can use the above instruction provided by the hardware to malke amy type of lock. This is inefficient solution

GUNICORN: Web Server threads 