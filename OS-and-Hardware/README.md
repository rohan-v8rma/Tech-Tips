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
- [CPU vs. GPU](#cpu-vs-gpu)
  - [What is unique in a GPU, that makes it different from a CPU?](#what-is-unique-in-a-gpu-that-makes-it-different-from-a-cpu)
  - [Why CPUs are still needed](#why-cpus-are-still-needed)
- [Deepdive on Cores](#deepdive-on-cores)
  - [What is meant by the Clock Speed of a core within a CPU?](#what-is-meant-by-the-clock-speed-of-a-core-within-a-cpu)
  - [How can smaller transistors help a core achieve higher clockspeeds?](#how-can-smaller-transistors-help-a-core-achieve-higher-clockspeeds)
  - [What is the impact of die size on a CPU?](#what-is-the-impact-of-die-size-on-a-cpu)
  - [Tensor cores and how they are different from CUDA cores](#tensor-cores-and-how-they-are-different-from-cuda-cores)
    - [What tensor cores do better](#what-tensor-cores-do-better)
    - [What tensor cores lose out on](#what-tensor-cores-lose-out-on)
  - [Architectural difference between Regular, CUDA and Tensor cores](#architectural-difference-between-regular-cuda-and-tensor-cores)
    - [Regular CPU Cores](#regular-cpu-cores)
    - [CUDA Cores (GPU)](#cuda-cores-gpu)
    - [Tensor Cores (GPU)](#tensor-cores-gpu)
- [Parallelism and Concurrency](#parallelism-and-concurrency)
  - [Simultaneous Multithreading](#simultaneous-multithreading)
    - [What is meant by CPU having 4 cores and 8 threads?](#what-is-meant-by-cpu-having-4-cores-and-8-threads)
    - [How is SMT different from threads created and scheduled by OS?](#how-is-smt-different-from-threads-created-and-scheduled-by-os)
    - [How is SMT implemented? (Difference in hardware from Non-SMT)](#how-is-smt-implemented-difference-in-hardware-from-non-smt)
      - [Register File](#register-file)
      - [Program Counter](#program-counter)
      - [Execution Pipelines](#execution-pipelines)
  - [Why can't we have an unlimited number of cores?](#why-cant-we-have-an-unlimited-number-of-cores)
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

# CPU vs. GPU

## What is unique in a GPU, that makes it different from a CPU?

GPU contains a large number of microprocessors, which are designed to handle parallel processing tasks. 

These microprocessors work in unison to perform computations on multiple data points simultaneously, making GPUs highly efficient for tasks that can be parallelized.

## Why CPUs are still needed

CPUs are better suited for certain tasks compared to GPUs due to their architectural design and characteristics. Here are some tasks that CPUs excel at:

1. **Single-Threaded Performance**: CPUs are optimized for single-threaded performance and are well-suited for tasks that require sequential processing and complex control flow. Tasks that involve frequent branching, conditional statements, and serial computations tend to perform better on CPUs.

2. **Serial Processing**: Applications that are inherently serial in nature, where one task depends on the completion of another, benefit from CPUs. This includes operating system tasks, system management, and certain types of database operations.

3. **Branch-Intensive Algorithms**: Algorithms with unpredictable or frequent branching, such as certain types of encryption and decryption algorithms, often perform better on CPUs due to their advanced branch prediction mechanisms.

4. **Latency-Sensitive Applications**: CPUs have lower latency compared to GPUs, making them suitable for tasks that require low response times. Real-time applications, such as certain types of networking and communication protocols, benefit from the CPU's ability to quickly handle events and interrupts.

5. **Complex Control Logic**: Tasks that involve complex control logic, dynamic task scheduling, and task prioritization are better handled by CPUs. This includes managing I/O operations, multitasking, and resource allocation.

6. **General-Purpose Computing**: CPUs are versatile and can handle a wide range of tasks, making them suitable for general-purpose computing. They are capable of running diverse applications without requiring specialized optimization.

7. **Legacy Software**: Older software or software that is not optimized for parallel processing may perform better on CPUs. Many legacy applications and software libraries are designed with single-threaded execution in mind.

8. **Small Data Sets**: CPUs are efficient for processing small data sets that do not benefit significantly from parallelization. Tasks that do not involve large-scale data processing or matrix computations can be handled efficiently by CPUs.

---

# Deepdive on Cores

## What is meant by the Clock Speed of a core within a CPU?

- The clock speed of a core within a CPU refers to the rate at which the core can execute instructions and perform calculations. 
  
  It is measured in Hertz (Hz) and indicates how many cycles the core can complete in one second. 
  
- A higher clock speed generally means that the core can process instructions more quickly, leading to faster overall performance.

- In modern CPUs, each core typically has its own clock generator that controls its internal operations. 
  
  The clock speed ***determines how quickly the core's logic gates can switch between 0 and 1 states***, which directly affects the execution of instructions and data processing.

## How can smaller transistors help a core achieve higher clockspeeds?

Smaller transistors can attain higher clock speeds primarily due to several key factors related to their size, structure, and electrical properties. 

These factors contribute to improved performance and faster switching times, enabling higher clock frequencies. Here's why smaller transistors can achieve higher clock speeds:

1. **Reduced Capacitance:** Smaller transistors have lower capacitance between their components. Capacitance is a measure of the ability to store electrical charge, and smaller capacitance allows for faster charging and discharging of the transistor. This leads to shorter signal propagation delays and faster switching times, enabling higher clock speeds.

2. **Shorter Interconnects:** Smaller transistors require shorter interconnects between components, such as gates and sources. Shorter interconnects lead to lower resistance and lower parasitic capacitance, which in turn reduces signal propagation delays and improves overall performance.

3. **Lower Inertia:** The smaller size of transistors reduces the inertia of the electrons moving through them. This means that electrons can change direction and speed more quickly, resulting in faster switching times and higher clock speeds.

4. **Reduced Gate Length:** The gate length of a transistor is a critical factor in determining its switching speed. Smaller transistors have shorter gate lengths, allowing the gate voltage to control the flow of current more rapidly. This enables faster on-off transitions and higher clock speeds.

5. **Improved Materials and Manufacturing:** Advances in semiconductor manufacturing techniques and materials have allowed for the creation of smaller transistors with better performance characteristics. These advancements include new materials with higher electron mobility and more precise manufacturing processes, which contribute to faster transistor switching speeds.

6. **Heat Dissipation:** Smaller transistors generate less heat during operation due to their reduced size and lower power consumption. This improved heat dissipation allows for more efficient cooling and thermal management, which can support higher clock speeds without overheating.

7. **Reduced Electron Transit Time:** Electrons can move shorter distances in smaller transistors, resulting in reduced electron transit time between different parts of the transistor. This further contributes to faster switching and higher clock speeds.

8. **Quantum Effects:** In smaller transistors, quantum effects can come into play, allowing for faster electron tunneling and other quantum phenomena. While these effects can be challenging to control, they can also be harnessed to achieve faster switching times.

It's important to note that while smaller transistors can achieve higher clock speeds, there are also limitations and challenges associated with shrinking transistor sizes, such as increased leakage current and heat generation. 

As a result, the pursuit of higher clock speeds is often balanced with considerations of power efficiency, thermal management, and overall system design.

## What is the impact of die size on a CPU?

The die size of a CPU, which refers to the physical size of the silicon wafer on which the processor is manufactured, has significant impacts on various aspects of the CPU's performance, power consumption, and capabilities. 

Here are some of the key impacts of die size on a CPU:

1. **Performance:** A larger die size allows for more transistors to be integrated onto the chip. This can lead to more complex and powerful processor designs, enabling higher levels of performance. More transistors can be allocated to features like cache memory, execution units, and specialized accelerators, which can enhance the CPU's ability to process instructions and data more efficiently.

2. **Transistor Count:** A larger die size directly translates to a higher transistor count. More transistors provide the opportunity for more advanced features and functionalities to be included in the CPU design. This includes additional cores, larger caches, better branch prediction, and improved instruction pipelines.

3. **Thermal Design Power (TDP):** A larger die size generally leads to higher power consumption and heat generation. This can potentially impact the CPU's thermal design power (TDP), which is the maximum amount of heat the cooling system needs to dissipate to keep the CPU within its operational temperature range. CPUs with larger die sizes may require more sophisticated cooling solutions to manage heat.

4. **Manufacturing Cost:** Larger die sizes require more silicon wafers and are therefore more expensive to manufacture. The cost of producing CPUs with larger die sizes can impact the overall pricing strategy of a product. Manufacturers often aim for a balance between die size, manufacturing cost, and market demand.

5. **Integration of Components:** A larger die size provides more space for integrating various components onto the chip. This can include not only CPU cores but also integrated graphics, memory controllers, and other specialized units. Integrated components can improve overall system efficiency and reduce the need for separate chips.

6. **Manufacturability and Yield:** While a larger die size offers more room for features, it can also present challenges during the manufacturing process. Larger dies may have a higher likelihood of defects, which can affect yield (the percentage of functional chips from a wafer). Manufacturers need to carefully balance die size with yield considerations.

7. **Upgradability:** In some cases, larger die sizes may limit the potential for CPU upgradability. Smaller CPUs with smaller die sizes might be easier to fit into existing socket designs and motherboard layouts, allowing for smoother upgrades without requiring major hardware changes.

8. **Form Factor and Power Efficiency:** The size of the die can impact the overall form factor of the CPU package. Smaller die sizes can lead to more compact and power-efficient designs suitable for mobile devices and laptops.


## Tensor cores and how they are different from CUDA cores

### What tensor cores do better

- Microprocessors (unique to GPUs) are inherently designed to perform a wide range of arithmetic and logical operations. 

  Among these operations, matrix multiplication stands out as a crucial computation. For instance, the multiplication of two 4x4 matrices necessitates a staggering 64 multiplications and 48 additions.

- The significance of tensor cores becomes particularly evident in tasks like convolution and multiplication. These computational operations are central to various fields, including Machine Learning, Deep Learning, and Ray Tracing.

- As the dimensions and size of matrices (or tensors) expand, the computational complexity increases significantly. This surge in complexity is especially pronounced in domains such as Machine Learning, where intricate mathematical operations are common.

- In essence, tensor cores are specialized processing units tailored to excel in scenarios involving extensive matrix computations. Their enhanced capabilities are particularly valuable in applications demanding large-scale multiplications, as seen in Machine Learning algorithms and graphics rendering processes like Ray Tracing.

### What tensor cores lose out on



## Architectural difference between Regular, CUDA and Tensor cores

### Regular CPU Cores

1. **Architecture**: Regular CPU cores are designed for general-purpose computing and are based on complex instruction set architecture (CISC) or a combination of CISC and reduced instruction set architecture (RISC) principles.
2. **Instruction Set**: They support a wide range of instructions for executing various tasks, including arithmetic, logic, memory access, and control operations.
3. **Single-Threaded Performance**: CPU cores are optimized for single-threaded performance, aiming to execute complex instructions quickly and efficiently.
4. **Multithreading**: Many modern CPUs support simultaneous multithreading (SMT), allowing multiple threads to run on a single core.
5. **Cache Hierarchy**: CPUs have a hierarchical cache design with L1, L2, and sometimes L3 caches to reduce memory access latency.
6. **Power Efficiency**: Regular CPU cores are designed to balance performance and power efficiency, making them suitable for a wide range of tasks.

### CUDA Cores (GPU)

1. **Architecture**: CUDA cores are specific to NVIDIA GPUs and are designed for parallel processing tasks. They are based on the streaming multiprocessor (SM) architecture.
2. **Parallel Processing**: CUDA cores are designed to execute multiple threads simultaneously, making them well-suited for parallel workloads like graphics rendering, scientific simulations, and data parallel computations.
3. **SIMD Execution**: They use single instruction, multiple data (SIMD) execution, where each core can execute the same instruction on multiple data elements in parallel.
4. **Thread Blocks and Grids**: CUDA cores are organized into thread blocks and grids, which allow for fine-grained parallelism and task distribution.
5. **Memory Hierarchy**: GPUs have specialized memory hierarchies, including global memory, shared memory, and texture memory, optimized for parallel access patterns.
6. **Floating-Point Performance**: CUDA cores excel at floating-point computations, making them suitable for graphics rendering and high-performance computing (HPC) applications.

### Tensor Cores (GPU)

1. **Architecture**: Tensor cores are a specialized component found in some NVIDIA GPUs, designed specifically for deep learning and tensor-based computations.
2. **Matrix Operations**: Tensor cores accelerate matrix-matrix multiplications and mixed-precision operations, common in neural network training and inference.
3. **Mixed-Precision**: Tensor cores perform computations in mixed precision (e.g., FP16), which improves performance without significantly sacrificing accuracy.
4. **AI Workloads**: Tensor cores greatly accelerate AI workloads by performing multiple operations per clock cycle.
5. **Support for DL Libraries**: They are designed to work seamlessly with popular deep learning frameworks and libraries like TensorFlow and PyTorch.

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

## Why can't we have an unlimited number of cores?

See this video to get core info: https://www.youtube.com/watch?v=dMejriBC_Kc&t=382s

1. **IO Operations and Memory Speed:** As the number of cores increases, the demand for IO (Input/Output) operations also increases. IO operations involve communication with peripherals, storage devices, network interfaces, and other external resources. 

   These operations often require waiting for data transfers or responses, which can lead to core idle time. 
   
   Additionally, the speed of RAM (Random Access Memory) is generally slower compared to the speed of CPU cores. 
   
   As a result, when there are a large number of cores, they can be frequently stalled waiting for data from memory, limiting their overall performance.

2. **Heat Dissipation and Throttling:** Increasing the number of cores on a single chip leads to a higher density of components, resulting in concentrated heat generation. 
   
   This concentration of heat can cause overheating issues if not properly managed. To prevent overheating, processors employ thermal management techniques, including throttling, which reduces the operating frequency or performance of the cores when they reach certain temperature thresholds. 
   
   Throttling is necessary to protect the chip from damage, but it also reduces the throughput per core, impacting the overall performance.

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