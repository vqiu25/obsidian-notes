# Introduction

>[!Note]- Parallel Computing vs Distributed Computing vs Multitasking
> <!-- Multiline -->
>* **~={green}Parallel Computing=~**:
>	* ~={blue}Multiple=~ processing elements (**~={blue}processors=~**)
>	* Working simultaneously on a **~={blue}single task=~**, dividing the work to solve a problem faster.
>	* **~={red}Motivation=~**: Faster execution than with a single processor system.
>* **~={green}Distributed Computing=~**:
>	* ~={blue}Multiple=~ independent **~={blue}systems=~** communicating **over a network**.
>	* Collaborating on a **~={blue}single task=~** or handling **separate subtasks** in a larger problem.
>	* Systems may have their own **memory and resources** (loosely coupled).
>* **~={green}Multitasking=~**:
>	* A **~={blue}single=~ system** runs **~={blue}multiple=~ independent tasks** concurrently.
>	* Uses **task switching** (time-sharing) rather than true parallel execution.
>	* **~={red}Motivation=~**: Efficient CPU utilisation by overlapping execution of different tasks.

## How to Measure Parallel Performance

>[!Note]- Speed Up
> <!-- Multiline -->
> ~={green}**Definition**=~: The execution time of a program when it is written in sequential divided by the parallel version
> 
> ~={blue}**Equation**=~:
> * ~={purple}Sequential Time=~ $T_S$ : execution time on single processor
> * ~={purple}Parallelisation Time=~ $T_P$ : execution time on $p$ processors
> $$ S = \frac{T_S}{T_P} $$
>
>**~={red}Notes=~**
>* Ideally $S=p$, but in practice, $S<p$. Which is simply to say, the impact each additional processor has is not $\times p$. $S=p$ happens when **~={green}Super Linear Speedup=~** occurs.
>* Note that $T_S + T_P \neq T_{total}$, as they both individual are the total time, simply under different circumstances

>[!Note]- Efficiency
> <!-- Multiline -->
> ~={green}**Definition**=~: The speedup per processor
> 
> ~={blue}**Equation**=~:
> * ~={purple}Sequential Time=~ $T_S$ : execution time on single processor
> * ~={purple}Parallelisation Time=~ $T_P$ : execution time on $p$ processors
> * ~={purple}Speedup=~: $T_S / T_P$
> $$ E = \frac{S}{p} = \frac{T_S}{p\cdot T_P} $$
>
>**~={red}Notes=~**
>* Efficiency is a value between 0 and 1 (0% and 100%)
>	* ~={green}Best Case=~: $S = p$, where the speed up is $\times p$, hence this ratio would be 100%

>[!Note]- Amdahl's Law (Maximum Speedup) (Assume $T_S$ Constant)
> <!-- Multiline -->
> **~={green}Definition=~**: The speedup of a parallel program is limited by the fraction of the workload that remains sequential, regardless of the number of processors.
>
> **~={blue}Equation=~**:
> * ~={purple}Fraction=~ $0 \leq f \leq 1$ of a sequential program that cannot be parallelised. Hence, $(1 - f)$ can be perfectly parallelised.
> * ~={purple}Parallel Time (Total Execution on $p$ Processors)=~: Of our total sequential time, $f$ percent can't be parallelised, and $(1-f)$ can be perfectly parallelised, meaning that the remaining time can be reduced by dividing $p$ processors
> $$T_P = f \cdot T_S + (1 - f) \cdot \frac{T_S}{p}$$
> * Hence the **~={red}total speedup=~**:
>
>$$ S = \frac{T_S}{T_P} = \frac{T_S}{f \cdot T_S + (1 - f) \cdot \frac{T_S}{p}} = \frac{p}{1+(p-1) \cdot f}$$
>
>**~={red}Notes=~**
> * ~={purple}**Diminishing Returns with More Processor=~s:** Even if you keep increasing the number of processors, the performance gain is limited by the **sequential portion** of the program.
> 	* **~={red}For example=~**, if **50% of the program cannot be parallelised**, then the maximum achievable speedup is **at most 2×**, no matter how many processors you add.
> 	* $S_{P \to \infty} = \frac{1}{f}$ Defines the max speedup

> [!Note]- Gustafson-Barsis' Law (Maximum Speedup) (Assume $T_P$ Constant)
> 
> **~={green}Definition=~**: Unlike Amdahl’s Law, which assumes a fixed problem size, Gustafson-Barsis’ Law argues that **as we increase the number of processors, we can also scale the problem size**, leading to **greater speedup** than Amdahl's prediction. Because we have $T_P$ being constant even when more processors are added - this is achieved by "increasing" the problem size.
> 
> **~={blue}Equation=~**:
> 
> - **~={purple}Fraction=~**: Let $seq$ be the sequential part of $T_P$ (where only 1 processor is active)
> - **~={purple}Total execution time on $p$ processors=~**: Hence, $T_P - seq$ is the time amount that can be parallelised, but in a sequential environment, it would take $p$ times as long, hence $(T_P - seq) \cdot p$
>     $$T_S=seq + (T_P - seq) \cdot p$$
> - Hence the total **~={red}speedup=~**:  
> 	- $f = \frac{seq}{T_P}$, which is the percentage that can't be parallelised
>     $$S = \frac{T_S}{T_P} = \frac{seq + (T_P - seq) \cdot p}{T_P} = p - (p - 1) \cdot f$$
> 
> **~={red}Notes=~**
> - As more processors are added, the more work is being done. This isn't the case with Amdahl's Law, where even with more processors, the work being done stays the same.
> - Here the max speed up grows linearly. As we increase the number of processors, the speedup increases linearlyish too.

# Architectures of Parallel Systems

## Streams

> [!QUOTE] Quick Notes
> Most modern systems have both SIMD (GPU) and MIMD (Multi Core CPU)

>[!Note]- Flynn's Taxonomy (Types of Computer Architecture)
> <!-- Multiline -->
> * **~={green}Instruction=~**: What to do (`ADD A, B`)
> * **~={green}Data=~**: What to do it on (`A = 3`, `B = 2`)
> 
> **~={purple}<u>Categories</u>=~**
> * **~={blue}SISD (Single Instruction, Single Data)=~** (Single Core CPU): 
> 	* Executes one instruction at a time on one set of data.
> 	* **~={red}Example=~**: `ADD A, B -> DONE`. How a Standard Single Core CPU works.
> * **~={blue}MISD (Multi Instruction, Single Data)=~**:
> 	* Multiple instructions operate on **the same piece of data.
> 	* **~={red}Example=~**: Used in fault tolerant systems. Running multiple safety checks on the same sensor data. (Rare)
> * **~={orange}SIMD (Single Instruction, Multi Data)=~** (GPU):
> 	* A single instruction is applied to many pieces of data at the same time.
> 	* **~={red}Example=~**: Apply blur filter on every pixel in an image
> * **~={orange}MIMD (Multi Instruction, Multi Data)=~** (Multi Core CPU):
> 	* Different **CPUs/cores** running different **programs** on different **data**.
> 	* **~={red}Example=~**: **PC** running a video game while also **playing music** and **downloading a file**.

>[!Note]- Types of Parallelism
> <!-- Multiline -->
> * **~={green}Task Parallelism=~** → Different tasks running at the same time (multiple threads running, all doing different tasks (i.e. different programs running))
> * ~={green}**Data Parallelism**=~ → The same task is applied to multiple data elements ( (multiple threads all doing the same task (i.e. each thread blurring pixels on an image))

>[!Note]- SIMD Architecture
> <!-- Multiline -->
> **~={purple}Properties=~**
> * Single **~={green}Control Unit=~** (Sends Instructions to Processing Elements)
> * Multiple **~={green}Processing Elements=~** (Processing Elements all Execute the Same Instruction)
> * Multiple **~={green}Memories=~** (Stores data that PEs process.)
> * **~={green}Interconnection Network=~** (Transfers data between components.)
> 
> ![[Drawing 2025-03-08 21.04.42.excalidraw | center | 350]]

>[!Note]- MIMD Architecture
> <!-- Multiline -->
> **~={purple}Properties=~**
> * Multiple **~={green}Control Units=~**
> * Multiple **~={green}Processing Elements=~**
> * Multiple **~={green}Memories=~**
> * **~={green}Interconnection Network=~**
> 
> ![[Drawing 2025-03-08 21.17.08.excalidraw | center | 500]]

## Memory and Parallel Systems

### Memory Location and Access

>[!Note]- Memory Locations (Where is the memory located)
> <!-- Multiline -->
> * **~={green}Centralised Memory=~**: All cores share the same memory/RAM (memory on 1 system)
> * **~={green}Distributed Memory=~**: Each processor has it's own private memory (memory on multiple systems)

>[!Note]- Memory Access Paradigms (Which processor are allowed to access which memory)
> <!-- Multiline -->
> * **~={green}Shared Memory=~**: Multiple processors **access the same memory/can access all memory**.
> * **~={green}Message Passing=~**: Processors **do not share memory**; instead, they **communicate via messages** (network calls, RPC).

### System Categories

>[!Note]- Centralised Shared Memory
> <!-- Multiline -->
> Each processor/core can access the centralised memory
> 
> ![[Drawing 2025-03-08 21.45.00.excalidraw | center | 350]]

>[!Note]- Distributed Shared Memory
> <!-- Multiline -->
> Each processor has some memory associated with it. A processor can access each other's memory
> 
> ![[Drawing 2025-03-08 21.48.49.excalidraw | center | 450]]

>[!Note]- Distributed Memory, Message Passing
> <!-- Multiline -->
> If a processor wants to access another a processor's memory, it needs to involve the other processor
> 
> ![[Drawing 2025-03-08 21.53.27.excalidraw | center | 350]]


# Performance of Parallel Systems


# Parallelisation Process

## Decomposition

## Dependence Analysis

## Scheduling


# Concurrency

## Thready Safety & Synchronisation


# Concurrency and GUI


# Programming (Shared Memory)

## OpenMP

## Object Oriented Parallelisation

## Heterogenous System Programming

#flashcards/os/highperformancecomputing