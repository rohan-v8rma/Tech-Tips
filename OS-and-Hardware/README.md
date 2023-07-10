# INDEX

Refer OS notes.

- [INDEX](#index)
- [What happens when a program runs?](#what-happens-when-a-program-runs)
- [What is an Operating System?](#what-is-an-operating-system)
  - [How OS achieves the task of making programs easier to run (*Virtualization*)](#how-os-achieves-the-task-of-making-programs-easier-to-run-virtualization)
  - [How users interface with the OS to perform general operations (*System Calls* \& *Standard Library*)](#how-users-interface-with-the-os-to-perform-general-operations-system-calls--standard-library)
  - [How OS functions as a *Resource Manager*](#how-os-functions-as-a-resource-manager)
- [Different ways to represent dates in Software](#different-ways-to-represent-dates-in-software)
  - [Epoch time](#epoch-time)
  - [ISO 8601](#iso-8601)
  - [Calendar date](#calendar-date)
  - [Julian date](#julian-date)
  - [`.NET` date time](#net-date-time)
  - [UNIX timestamp](#unix-timestamp)
- [Parallelism and Concurrency](#parallelism-and-concurrency)
  - [Simultaneous Multithreading](#simultaneous-multithreading)
    - [What is meant by CPU having 4 cores and 8 threads?](#what-is-meant-by-cpu-having-4-cores-and-8-threads)
    - [How is SMT different from threads created and scheduled by OS?](#how-is-smt-different-from-threads-created-and-scheduled-by-os)
    - [How is SMT implemented? (Difference in hardware from Non-SMT)](#how-is-smt-implemented-difference-in-hardware-from-non-smt)
      - [Register File](#register-file)
      - [Program Counter](#program-counter)
      - [Execution Pipelines](#execution-pipelines)
- [TODO](#todo)
  - [A deep dive into Virtualization](#a-deep-dive-into-virtualization)
  - [Concurrency](#concurrency)
  - [Persistence](#persistence)
  - [OS-level virtualization](#os-level-virtualization)
  - [Hypervisor](#hypervisor)
- [More Operating System Concepts](#more-operating-system-concepts)
  - [UUID (`Universally Unique Identifier`)](#uuid-universally-unique-identifier)
  - [File Offset](#file-offset)
  - [What exactly, is a Swap Partition?](#what-exactly-is-a-swap-partition)
      - [Changing the size of the swap partition using the terminal](#changing-the-size-of-the-swap-partition-using-the-terminal)
  - [Environment Variables](#environment-variables)
  - [Binary Files](#binary-files)
  - [Executable Files](#executable-files)
  - [Binary Package](#binary-package)
- [TODO](#todo-1)
  - [Containers](#containers)
  - [Loop Device](#loop-device)
  - [Block Device](#block-device)


# What happens when a program runs?

Well, a running program does one very simple thing: *it executes instructions*. 

Many millions (and these days, even billions) of times every second, the processor:
1. Fetches an instruction from memory.
   
2. Decodes it (i.e., figures out which instruction this is).
   
3. And, executes it (i.e., it does the thing that it is supposed to do, like *add two numbers together*, *access memory*, *check a condition*, *jump to a function*, and so forth). 
   
4. After it is done with this instruction, the processor moves on to the next instruction, and so on, and so on, until the program finally completes.

> **Note** : Of course, modern processors perform a series of optimizations underneath the hood to make programs run faster, which include but are NOT limited to: 
> - Executing multiple instructions at once.
> - Issuing​ and completing instructions out of order.
> 
> But that is not our concern here; we are just concerned with the simple model most programs assume: that instructions seemingly execute one at a time, in an orderly and sequential fashion. 
> 
> This is known as **Von Neumann model of computing**.

# What is an Operating System?

There is a body of software, in fact, that is responsible for making it *easy to run programs* (even allowing you to seemingly run many at the same time):
- allowing programs to share a memory.
- enabling programs to interact with devices like disks and I/O; etcetera. 

That body of software is called the **Operating System (OS)**, as it is in charge of making sure the system *operates* correctly and efficiently in an easy-to-use manner.

## How OS achieves the task of making programs easier to run (*Virtualization*)

The concept of ***Virtualization*** consists of the OS taking a physical resource such as: the *processor*, or *memory*, or a *disk*) and transforms in into a more general, powerful, and easy-to-use **virtual** form of itself.

> ***Note***: The generality of the resource refers to having a uniform way of interfacing with the resource irrespective of the machine's architecture. 
> 
> So, an OS installed on two different machines will allow for same procedures for interfacing with the virtual form of a particular resource.

Thus, we sometimes refer to the OS as a **virtual machine**, because of it having *virtual* counterparts of the components present in a *physical machine*.

---

## How users interface with the OS to perform general operations (*System Calls* & *Standard Library*)

In order to allow users to tell the OS what to do and thus make use of the features of the *virtual machine*, such as:
- running a program
- allocating a memory
- accessing a file

, the OS provides some interfaces ([API](../APIs/README.md)s) that one can call.

A typical OS, in fact, exports a few hundred ***System Calls*** (similar to library functions provided in certain languages to avoid re-building of fundamental logic repeatedly); that are available to applications.

> ***Note***: Because the OS provides these calls to *run programs*, *access memory* and *interface with connected devices*, along with other related actions; we sometimes say that the OS provides a ***Standard Library*** to applications.

---

## How OS functions as a *Resource Manager*

We know that *virtualization* allows: 
- Many programs to run; thus *sharing the CPU*, by each program executing its instruction turn-wise.
- Many programs to concurrently access their corresponding instructions and data; thus *sharing the memory*.
- Many programs to interface with devices; thus *sharing disks, I/O and so forth*

Because of this, the OS is sometimes known as as ***Resource Manager***

The CPU, memory and disk are ***resources*** of the system, which the OS transforms into *virtual* resources. It then needs to ***manage*** these, doing so efficiently or fairly or indeed with many other possible goals in mind.

# Different ways to represent dates in Software

## Epoch time

This is the number of seconds that have elapsed since January 1, 1970 (UTC). This is a commonly used representation in computer systems and programming languages.

## ISO 8601

This is an international standard for representing date and time in a machine-readable format. It uses the format `YYYY-MM-DDTHH:MM:SS`.

## Calendar date

This is the traditional way of representing a date, using the format `MM/DD/YYYY` or `DD/MM/YYYY`. This format is often used in human-readable interfaces and user input.

## Julian date

A Julian date is the number of days that have elapsed since January 1, 4713 BC (Julian calendar). It is widely used in astronomical applications.

## `.NET` date time

`.NET` Framework provides a set of date and time classes in the System namespace that are designed for both local and global use. It is widely used in Microsoft applications.

## UNIX timestamp

similar to Epoch time but the reference point is January 1, 1970 (UTC) but not limited to seconds but also includes milliseconds, microseconds and nanoseconds.

---

# Parallelism and Concurrency

## Simultaneous Multithreading

### What is meant by CPU having 4 cores and 8 threads?

This is known as simultaneous multi-threading, where the CPU has hardware level support for concurrent execution (read ahead to know what is different in the hardware).

With SMT, each physical core appears as multiple logical cores to the operating system. 

For example, a dual-core processor with SMT would appear as four logical cores to the OS. Each logical core can execute its own thread independently, allowing for concurrent execution of multiple threads on a single physical core.

### How is SMT different from threads created and scheduled by OS?

- OS level threads need to be created and managed by OS. 

  They need to be scheduled according to loads on various cores, by the OS, using some type of scheduling algorithm.

  Special code needs to be written for handling these type of threads.

- In the case of SMT, the threads in a CPU are available as distinct logical cores to the OS,
  which can be used as a physical core would be.

  No special code is required for enabling this concurrent behavior, as it is already looked after by the CPU manufacturer.

### How is SMT implemented? (Difference in hardware from Non-SMT)

It involves duplicating certain components within a physical core to support simultaneous execution of multiple threads. 

Each logical core within a physical core has its own set of architectural registers, program counters, and execution pipelines, **allowing independent instruction streams to be executed concurrently**.

> ***Note***: These independent instruction streams can include instructions from various OS level threads that are executing concurrently, on the OS level.
>
> So, a 4 core 8 thread processor could be running more than 8 processes at a time due to **OS levl thread creating and scheduling**.

By duplicating these components, SMT enables the processor to schedule and execute instructions from different threads in parallel, effectively utilizing idle resources within a physical core. This improves overall performance and throughput, especially in situations where there are multiple threads with independent instruction streams.

From the perspective of the operating system, SMT appears as if there are more cores available than the actual physical cores. The OS can schedule threads onto these logical cores, and **the processor's hardware takes care of interleaving and executing the instructions from different threads concurrently**.

The primary components that are duplicated in an SMT implementation include:

#### Register File

Each logical core has its own set of architectural registers, allowing independent register state for each thread. This allows simultaneous execution of multiple threads without interfering with each other's register state.

#### Program Counter

Each logical core maintains its own program counter, which keeps track of the currently executing instruction for that thread. This enables each thread to independently fetch and execute instructions without relying on a shared program counter.

#### Execution Pipelines

SMT typically includes multiple execution pipelines within a physical core, allowing simultaneous execution of instructions from different threads. These pipelines may be duplicated or augmented to handle the increased instruction throughput.

---

# TODO

## A deep dive into Virtualization

## Concurrency 

## Persistence

## OS-level virtualization

https://en.wikipedia.org/wiki/OS-level_virtualization

## Hypervisor

https://en.wikipedia.org/wiki/Hypervisor


# More Operating System Concepts

## UUID (`Universally Unique Identifier`)

UUID is a unique identifier used in partitions to uniquely identify partitions in Linux operating systems. 

UUID is a property of the disk partition itself. So, if you install the hard drive containing the partitions on another Linux computer, the partitions will have the same UUID as before which is a good thing.

The UUID of a partition is required mainly for mounting the partitions correctly in a computer system where hundreds of hard drives are installed. 

If you mount the hard drives or SSDs using UUIDs, there is almost zero chances of the wrong hard drive getting mounted and causing serious data loss.

Our usual computers and laptops where mostly 1 or 2 hard drives are installed and we need limited number of partitions won’t benefit much from UUIDs.

Refer [this](https://linuxhint.com/uuid_storage_devices_linux/) for obtaining the UUID of a partition. 

## File Offset

An offset into a file is simply the character location within that file, usually starting with 0; thus “offset 240” is actually the 241st byte in the file. 

## What exactly, is a Swap Partition?

TODO

[`dd`](#dd-command-for-making-bootable-usbs-and-swap-partitions) command is used for making swap partitions.

#### [Changing the size of the swap partition using the terminal](https://askubuntu.com/a/1177939)

## Environment Variables

Environment variables are data holder variables stored within a system like unix, linux or windows etc. These variables are not accessible outside that environment. So only the programs executing inside these environments can access them and use them. They have a name and value.

They are like shortcuts which are used to avoid remembering paths for execution. 

For example, if we wish to execute python in an ubuntu installation in which the environment variables are set,
```console
python filename.py 
```

If I didn't set environment variable or define it,
```console
/usr/bin/python filename.py 
```
In both the methods python will execute and the first one is easy to use than the second one.

## Binary Files

In a strict sense a binary file is one which is not character encoded as human readable text. 

But colloquially, a "binary" refers to a file that is compiled i.e., contains **executable code**. 

Although, the file itself may not be executable in terms of its capacity to be run **ALONE**, even after having the required permissions.

## Executable Files

A file which is able to run standalone is an referred to as an "executable". 

Note that all executable files are NOT [binaries](#binary-files).

Executable text files which [invoke an interpreter via a shebang]() such as `#!/bin/sh` are executables too.

<!-- TODO: Complete this hyperlink -->

## Binary Package

A binary package in a linux context is an application package which contains (pre-built) executables, as opposed to source code.

Note that this does not mean a package file is itself an [executable](#executable-files). The package must be unpacked in order for you to access these files. 

- A package file is an archive (sort of like a `.zip`) which contains other files.
  
- But, a "**binary**" package file is one which specifically contains [executables](#executable-files) which contain binary code, since we know that ALL executables are not necessarily truly binaries.
  
  In fact, binary packages may be used for compiled libraries which are binary code, but not [executable](#executable-files) in nature. 

  
# TODO

## Containers
## Loop Device
## Block Device