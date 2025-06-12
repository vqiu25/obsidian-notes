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
> * `seq` = time of the parallel portion which is sequential. `T_s` is the total sequential time if there was no parallelism.
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
> * **~={green}Task Parallelism (MIMID)=~** → Different tasks running at the same time (multiple threads running, all doing different tasks (i.e. different programs running)). We have threads execute different tasks (functions/methods) concurrently, where both computation and I/O operations are called (which can be blocking). **~={red}Can be modelled with graphs=~**
> * ~={green}**Data Parallelism (SIMD/SPMD)**=~ → The same task is applied to multiple data elements ( (multiple threads all doing the same task (i.e. each thread blurring pixels on an image))

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
> * **~={green}Centralised Memory=~**: All cores share the same memory/RAM (memory on 1 system). 
> * **~={green}Distributed Memory=~**: Each processor has it's own memory (memory on multiple systems). Or, if you have a system with i.e. 4 processors, where each processor has DIMM slots to add ram, then the memory is distributed across the motherboard.

>[!Note]- Memory Access Paradigms (Which processor are allowed to access which memory)
> <!-- Multiline -->
> * **~={green}Shared Memory=~**: Multiple processors **access the same memory/can access all memory**. This is achieved via threads and communication through shared variables
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
> * If a processor wants to access another a processor's memory, it needs to involve the other processor
> * In distributed memory systems, a processor sends a message to request data, and the processor owning that data responds with it.
> 
> ![[Drawing 2025-03-08 21.53.27.excalidraw | center | 350]]

### Typical Large Parallel Systems

>[!Note]- Bottom Up Organisation of Large Parallel System
> <!-- Multiline -->
> **~={purple}Bottom Up Organisation=~**
> ![[Drawing 2025-03-09 07.26.59.excalidraw | center | 700]]

## Shared Memory Multiprocessors

>[!Note]- Shared Address Space: Transparent vs Explicit
> <!-- Multiline -->
> * **~={green}Transparent=~**: Memory is accessed normally using standard load/store operations.
> 	* **~={red}Example=~**: `x = 10` } The memory address of `x` is assigned the value `10`. The location of `x` in memory **does not matter** because all processors see the same shared address space.
> * **~={green}Explicit=~**: Writing into memory of other processor is a explicit instruction. Hence if we want to access memory of another processor, this has to be done explicitly through `mmap()` in C++.

>[!Note]- UMA vs NUMA
> <!-- Multiline -->
> Access times to memory locations can vary:
> 
> **~={purple}Uniform Memory Access UMA=~**
> * **All processors access memory at the same speed**, regardless of the memory location.
> * Uses a **single, centralised memory pool** shared among all processors.
> 
> **~={purple}Non-Uniform Memory Access NUMA=~**
> * Memory access time depends on the processor-memory location pair.
> * Memory is **physically distributed** across multiple processors.
> * Each processor has **fast access to its local memory**, but **slower access to remote memory** from another processor.
> 
> ![[Drawing 2025-03-09 07.59.45.excalidraw | center | 700]]

>[!Note]- Dangers of NUMA Architecture
> <!-- Multiline -->
> From the perspective of the programmer, when a CPU requires data from remote memory, that is something they will be unaware of. This is not great as remote memory usage is much slower. To be more explicit with remote memory usage, Message passing could be used.

>[!Note]- What are Memory Hierarchies, and why are they used?
> <!-- Multiline -->
> They are used so that system can be modular, and thus scalable. The idea is that we have nodes of of cpus, with a shared centralised memory that is fast to access. These nodes are connected with one another, where the caveat of communication between these nodes being slower.

>[!Note]- What is Thread Pinning?
> <!-- Multiline -->
> **~={green}Definition=~**: is the technique of binding a thread to a specific CPU core so that it always runs on the same processor.
> 
> **~={red}Without Thread Pinning=~**
> * The operating system (OS) freely moves threads between available CPU cores* when performing CPU scheduling & context switches
> * This movement can cause NUMA performance issues because a thread might get moved to a core far from its original memory allocation. Thus, it may **lose access to its cached data**, leading to **~={red}higher memory latency=~** and more ~={red}cache misses=~.
> 
> **~={green}With Thread Pinning=~**
> * A thread is locked (pinned) to a specific core, ensuring it always runs on the same processor.
> 

# Parallelisation Process

## Parallelisation Challenges & Processes

>[!Note]- Sequential vs Parallel Programming
> <!-- Multiline -->
> ![[Pasted image 20250312102116.png | center | 500]]

>[!Note]- Parallelisation Process
> <!-- Multiline -->
> 1. **~={purple}Task Decomposition=~**: Break the problem into independent subtasks.
> 2. **~={purple}Dependence Analysis=~**: Identify data dependencies and execution constraints. 
> 3. **~={purple}Scheduling=~**: Assign tasks to processors efficiently.
> 

## Task Decomposition

>[!Note]- Metrics for Parallelisation: Concurrency and Granularity 
> <!-- Multiline -->
> 1. **~={green}Degree of Concurrency=~**: The number of sub-tasks that can be executed simultaneously (Maximum Degree)
> 2. **~={green}Granularity=~**: Relation of number of tasks to their sizes
> 	* **~={red}Coarse Grain=~**: Few large tasks
> 	* ~={red}**Fine Grain**=~: Many small tasks

### Decomposition Techniques

>[!Note]- Recursive Decomposition
> <!-- Multiline -->
> Think parallelising merge sort and quick sort. The max concurrency is the number of leaves of the recursive tree.

>[!Note]- Exploratory Decomposition
> <!-- Multiline -->
> Cousin to recursive decomposition. DFS on a tree. Often the tree is unbalanced.

### Performance

>[!Note]- Overhead
> <!-- Multiline -->
> Parallelisation introduces overheads. This is as a result of thread management, bookkeeping, locking and synchronisation. The finer the granularity, the more threads, the more overhead.

>[!Note]- Load Balancing
> <!-- Multiline -->
> However, good speed ups only happen with good load balancing, where the finer the granularity, the better the potential balancing. As such, note that there is a conflicting aim:
> * **~={purple}Minimise overhead=~**: **~={red}Decrease=~** granularity
> * **~={purple}Maximise load balancing=~**: **~={green}Increase=~** granularity

>[!Note]- Communication Costs
> <!-- Multiline -->
> Communication between processors is slower. But higher parallelism requires more communication. As such, we want to avoid inter-processor communication as much as possible.
> 
> **~={purple}Message Passing vs Shared Memory=~**
> * **~={green}Message Passing=~**
> 	* Communication is explicit, which enforces rigorous communication planning
> * **~={green}Shared Memory=~**
> 	* Easier and more elegant than message passing. It's also quicker to develop, however it's often difficult to get good performance and correct correctness.
> 	* A few reasons why shared memory is difficult:
> 		* Data moves between caches
> 		* Data is accessed from remote memory locations (if NUMA systems)
> 		* False sharing of cache lines. There are caches in each processor. The caches are buffer for main memory, so processes don't always have to go back to main memory. Cache are organised in cache lines, so when 1 byte of the cache line is loaded in, the remaining bytes in the cache line are also loaded in. In multithreaded programs, 2 threads could be working on 2 items, but these items are in the same cache line. However, one processor has to have ownership of the cache line, requiring all other processors to delete what they have loaded in for consistency purposes.
> 		* Data is concurrently modified (race condition)

>[!Note]- Shared Memory Architecture vs Message Passing Architecture and Share Memory Programming Model vs Message Passing Programming Model
> <!-- Multiline -->
> ![[Drawing 2025-04-26 20.07.13.excalidraw | center | 700]]

### Practical Parallelisation Approach

>[!Note]- Parallelisation Approach
> <!-- Multiline -->
> 1. Consider target platform
> 2. Choose parallelisable algorithm
> 3. Choose data representation
> 4. Parallelisation technique

# Dependence Analysis

### Data Dependence

>[!Note]- Data Dependence Types
> <!-- Multiline -->
> * Flow Dependence
> * Anti Dependence
> * Output Dependence

>[!Note]- Flow Dependence
> <!-- Multiline -->
> **~={green}Flow Dependence=~**: Read after write
> 
> If we have $x \cdot 7 + (a + 2)$, and it's been decomposed into 4 tasks:
> 1. $a=2$
> 2. $u=a+2$
> 3. $v=a\cdot7$
> 4. $x=u+v$
> 
> In order for lines 2 and 3 to read $a$, it needs to after line 1 has written to $a$. Hence **~={red}lines 2 and 3 are flow dependent on line 1=~**.

>[!Note]- Antidependence
> <!-- Multiline -->
> **~={green}Antidependence=~**: Write after read
> 
> If we have $x \cdot 7 + (a \cdot 5 + 2)$:
> 1. $a=2$
> 2. $v=a\cdot5$
> 3. $u=v+2$
> 4. $v=a\cdot7$
> 5. $x=u+v$
> 
> Line 4 is antidependent on line 3. This is because line 3 reads $v$, and line 4 then writes to $v$. Thus we have a read then a write on the same variable.

>[!Note]- Output Dependence
> <!-- Multiline -->
> **~={green}Output Dependence=~**: Write after write
> ...
> 2. $o=10$
> ...
> 3. $o = 7$
> 
> * The order of the writes have influence on the final result.
> * Parallel execution should produce the same result as sequential

>[!Note]- How to Eliminate Dependence
> <!-- Multiline -->
> * **~={green}Flow Dependence=~**: Is inherent to the problem and can't be eliminated
> * **~={red}Anti and Output=~**: Are a product of variable utilisation and can be eliminated
> 
> Consider:
> 1. $a=2$
> 2. $v=a\cdot5$
> 3. $u=v+2$
> 4. $v=a\cdot7$
> 5. $x=u+v$
> 
> Line 4 is anti dependent on line 3. We can change the $v$ to a $w$ on lines 4 and 5. 

### Dependence in Loops

>[!Note]- Under what condition does Flow and Antidependence occur?
> <!-- Multiline -->
> ![[Pasted image 20250418231724.png | center | 500]]

>[!Note]- What is a Dependence Test?
> <!-- Multiline -->
> In parallel programming, a dependence test is a method used to determine whether **~={blue}two instructions=~** (or iterations of a loop) access the **~={green}same memory location=~** and whether one of those **~={red}accesses is a write=~**. 

>[!Note]- What is the GCD Test?
> <!-- Multiline -->
> If $a_1x_1+a_2x_2=c$, where $a_1, a_2, c$ are constant integers, it shall have an integer solution for $x_1$ and $x_2$ **~={blue}if and only if=~** $gcd(a_1, a_2)$ is a factor of $c$.
> 
> This is used to check if 2 arrays accesses can point to the same memory location across different loop iterations.
> 
> ```fortran
> for i=1 to n do
> 	A(a * i + k) = ...
> 	... = A(b * i + m)
> end for
>```
>We want to see if the:
>* Write to `A(a * i + k)`
>* Read from `A(b * i + m)`
>
>Could access the same memory location. Thus we change one of the $i$ to a $j$ to indicate a different iteration:
>```fortran
>a * i + k = b * j + m
>```
>Which can rearrange to:
>```fortran
>ai + (-b)j = m - k
>```
>Which is in the format of the diophantine equation. Hence if 
>* $gcd(a, b)$ is **~={red}not=~** a factor of $m-k$, there is **~={red}no dependence=~**
>* $gcd(a, b)$ **~={green}is=~** a factor of $m-k$, there **~={green}might be a dependence=~**. There is a solution, but it might not be in the iteration range.

>[!Info]- Identify Dependencies
> <!-- Multiline -->
> ![[Drawing 2025-04-19 12.11.12.excalidraw | center | 700]]

>[!Note]- Cases when there is Flow and Antidependence in a loop
> <!-- Multiline -->
> * **~={purple}Flow=~**: Within or across iterations. We write to `A[x]`, and we use read from `A[x]` next. (Get's smaller)
> * **~={purple}Antidependence=~**: Within or across iterations. We read from `A[x + 1]`, and we then write to `A[x]` next. (Get's smaller)
> 
> ![[Drawing 2025-04-19 12.28.58.excalidraw | center | 500]]

>[!Info]- Identify Intra and Inter Dependencies in a Single Loop
> <!-- Multiline -->
> ![[Drawing 2025-04-19 12.25.04.excalidraw | center | 700]]

>[!Info]- Identify Intra and Inter Dependencies in a Nested Loop
> <!-- Multiline -->
> ![[Pasted image 20250419142747.png | center | 600]]

>[!Note]- When can we swap loop order
> <!-- Multiline -->
> When there are no flow dependence which have a distance vector that is both positive and negative.

### Loop Transformations

> [!QUOTE] Quick Notes
> How to parallelise loops with dependencies. We can transform them!

>[!Note]- Loop Fission
> <!-- Multiline -->
> Loop fission (also called loop distribution) is a compiler optimisation technique where a single loop that performs multiple independent operations is split into multiple loops, each performing a subset of the original operations
> 
> **~={purple}When can we perform Loop Fission?=~**
> * When there are no intra dependencies (within loop), but inter dependencies exist.
> 
> ```cpp
> for (int i{2}; i < n; ++i) {
> 	b[i] = a[i] + i;
> 	x[i] = (b[i - 2] + b[i - 1]) / 2;
> }
>```
>The flow dependencies occur across loop iterations, specifically from:
>1. `b[i]` -> `b[i - 2]`, and
>2. `b[i]` -> `b[i - 1]`
>
>But because there are no dependencies within a loop, we can split these 2 lines into their own for loop
>
>```cpp
>for (int i{2}; i < n; ++i) {
>	b[i] = a[i] + i;
>}
>
>for (int i{2}; i < n; ++i) {
>	x[i] = (b[i - 2] + b[i - 1]) / 2;
>}
>```
>
>In the second loop, even though x[i] depends on previous values of b, those values were computed in the first loop, which is already finished by the time the second loop runs.

>[!Note]- Loop Shifting
> <!-- Multiline -->
> Loop shifting is a transformation that repositions iteration boundaries to eliminate inter-iteration dependencies — specifically to enable parallelism. This is only potentially possible if the inter dependencies are uniform.
> 
> **~={red}Example=~**
> ```cpp
> for (int i{1}; i <= n; ++i) {
> 	A[i] = B[i - 1] + 7;
> 	B[i] = C[i] + 6;
> }
>```
>Which when unrolled, looks like:
>
> ![[Drawing 2025-04-19 15.41.49.excalidraw | center | 400]]
> 
> If we shift the bounds up by 1:
> 
> ```cpp
> A[1] = B[0] + 7;
> 
> for (int i{1}; i < n; ++i) {
> 	B[i] = C[i] + 6;
> 	A[i + 1] = B[i] + 7;
> }
> 
> B[n] = C[n] + 6;
>```

### Control Dependence

>[!Note]- Control Dependence
> <!-- Multiline -->
> ![[Pasted image 20250419160033.png | center | 300]]

# Scheduling

## Task Scheduling with and without Dependencies

* **~={purple}Critical Path=~**: Longest path from start to finish
* **~={purple}Bottom Level=~**: Longest path from a node (include the initial node)
* **~={purple}Task Bottom Level Order=~**: Order tasks by non-increasing bottom level ($w_i$ and $c_{i,j}$ length)

## Loop Scheduling Without Dependencies

>[!Note]- What is Loop Scheduling Without Dependencies
> <!-- Multiline -->
> * **~={green}Chunksize=~**: Number of consecutive iterations handled together - single task. Total number of iterations $n$ is known.
> 
> ![[Pasted image 20250419202118.png | center | 600]]

>[!Note]- How would you split the iterations across threads?
> <!-- Multiline -->
> Can distinguish between 2 types:
> * **~={green}Static=~**: all decided before loop execution
> * **~={green}Dynamic=~**: decisions taken during loop execution
> 
> Together with adjustment of chunk size this leads to four well known techniques:
> * Block
> * Cyclic
> * Dynamic
> * Guided

### Static Scheduling Policies

>[!Note]- Block Policy
> <!-- Multiline -->
> **~={purple}Properties=~**
> * Static
> * Chunk size = $\frac{n}{p}$ ($n$ iterations, $p$ threads)
> * Iteration space divided into equal blocks
> * Each thread gets one block
> 
> ![[Pasted image 20250419202822.png | center | 350]]

>[!Note]- Cyclic Policy
> <!-- Multiline -->
> **~={purple}Properties=~**
> * Static
> * Constant chunk size $< \frac{n}{p}$
> * Iteration space divided into (smaller) chunks
> * Each thread gets multiple chunks, allocated in round robin
> 
> ![[Pasted image 20250419205643.png | center | 350]]

>[!Note]- Block vs Cyclic Policy
> <!-- Multiline -->
> ![[Pasted image 20250419210035.png | center | 500]]

### Dynamic Scheduling Policy

> [!QUOTE] Quick Notes
> Work is distributed at runtime than statically at compile time

>[!Note]- Dynamic Scheduling Policy
> <!-- Multiline -->
> * Dynamic
> * Constant chunk size $< \frac{n}{p}$: Each thread processes a fixed-size chunk of iterations
> * Each thread gets chunk on demand (request nest when ready) / (threads are assigned chunks in the order they become available)
> * **~={green}Pros=~**: Good for irregular workloads when execution times vary strongly. Also helps with load balancing among threads.
> * **~={red}Cons=~**: Higher overhead due to locking/synchronisation for accessing the word queue
> 
> ![[Pasted image 20250419211016.png | center | 450]]

### Guided Scheduling Policy

>[!Note]- Guided Scheduling Policy
> <!-- Multiline -->
> * Dynamic
> * Constant chunk decreases during execution (min can be fixed)
> 	* Starts with large chunks to reduce overhead early once
> 	* Gradually shifts to smaller chunks for better load balancing
> * Each thread gets chunk on demand (request nest when ready) / (threads are assigned chunks in the order they become available)
> * **~={green}Pros=~**: Good for irregular workloads when execution times vary strongly. Also helps with load balancing among threads. Lower overhead compared to dynamic, but worse off load balancing.
> * **~={red}Cons=~**: Will still have overhead compared to static policies. However, when the blocks are larger size, there's less frequent access to the queue which holds the chunks of loop iterations
> 
> ![[Pasted image 20250419211354.png | center | 450]]

## Loop Scheduling With Dependencies

>[!Note]- How to schedule with dependencies between loop iterations?
> <!-- Multiline -->
> * **~={green}Best=~**: Get rid of inter-iteration dependencies via loop transformations
> * **~={yellow}Ok=~**:
> 	* Loop body scheduling
> 	* Unrolling
> 	* Software pipelining

>[!Note]- What is a Flow Graph
> <!-- Multiline -->
> A systematic way to model dependencies between statements in loops. It is a directed cyclic graph.
> 
> **~={purple}Properties=~**:
> * Very similar to a task graph
> * Can have cycles
> * Edge weights indicate dependence distance (sometimes called delay)
> 
> **~={purple}How to Construct a Flow Graph=~**
> * Only put flow dependencies
> 
> ![[Drawing 2025-04-19 21.49.37.excalidraw | center | 450]]

### Strategies

>[!Note]- Loop Body Scheduling
> <!-- Multiline -->
> We treat the loop body as a task graph (DAG). To do this we:
> 1. Get the Flow Graph
> 2. Transform it to a Task Graph by removing all edges with a delay > 0. Thus only intra-iteration dependencies are kept.
>
>This allows us to schedule the statements within 1 iteration using this DAG. Hence, we need to complete the entire iteration before moving to the next one. This means each iteration is sequential, but within an iteration, we can parallelise it.
>
>![[Pasted image 20250419221108.png | center | 550]]

>[!Note]- Loop Unrolling
> <!-- Multiline -->
> The idea here is we increase the size of the loop body by partially loop unrolling. This just means you do more work per iteration.
> 
> **~={red}Example=~**
> * Note that in 1 iteration, we're now doing 3 iterations worth of work
> 
> ![[Pasted image 20250419222636.png | center | 450]]
> 
> * This means we have less task graphs, so less inter loop dependencies
> 
> ![[Pasted image 20250419222758.png | center | 450]]

>[!Note]- Loop Pipelining
> <!-- Multiline -->
> **~={purple}What is Pipelining=~**
> Is a technique where multiple stages of a task are overlapped in execution, allowing the next iteration to start before the previous one finishes—similar to an assembly line.
> 
> ![[Drawing 2025-04-19 22.43.59.excalidraw | center | 600]]

# OpenMP

> [!QUOTE] Quick Notes
> Standard for shared memory programming.

>[!Note]- How to use the library
> <!-- Multiline -->
> First we put `#pragma omp parallel` to create a team of threads to execute a block of code in parallel. Then we do `pragma omp <directive_name>` to specify how the work should be divided or controlled among those threads.

>[!Note]- What is OpenMP's Thread Concept
> <!-- Multiline -->
> 1. `#pragma omp parallel` forks threads. Following structured block is executed by each thread.
> 2. At the end of the structured block, the threads join back together into a single thread
> 
> ![[Pasted image 20250419231513.png | center | 450]]

>[!Note]- How many threads in the parallel region?
> <!-- Multiline -->
> You can specify it in the directive. If not specified, it defaults to the number of processors/cores in your system.

## Work sharing constructs

>[!Note]- Implicit Barrier
> <!-- Multiline -->
> There is an implicit barrier at the end of for, sections, and single, but not master. This means that all threads will wait at the end of these blocks until all threads have completed their portion of work, so they can proceed together.  You can suppress this barrier using the nowait clause (e.g. `#pragma omp for nowait` or `#pragma omp sections nowait`), allowing threads to continue without waiting.

### For Construct

| ~={purple}Schedule Kind=~ | ~={purple}Schedule Kind=~                                                |
| ------------------------- | ------------------------------------------------------------------------ |
| static                    | Entire iteration space / num_threads                                     |
| dynamic                   | **1**                                                                    |
| guided                    | **Implementation-defined** (usually starts large, shrinks exponentially) |
| runtime                   | Based on OMP_SCHEDULE env variable                                       |

>[!Note]- `for` construct
> <!-- Multiline -->
> - Used in OpenMP to parallelise `for` loops
> - **~={blue}Syntax=~**: `#pragma omp for` followed by a standard `for(...)` loop
> - Must be placed inside a `parallel` region
> - Distributes loop iterations among threads
> - Iterations must be **independent**
>  
> **~={purple}Constraints=~**
> - Loop must follow the form: `for (var = a; var logical-op b; incr-exp)`
> - **~={blue}Allowed=~** `logical-op`: `<`, `<=`, `>`, `>=`
> - **~={blue}Allowed=~** `incr-exp`: `var = var +/- incr` or `var++`
> - Loop variable must **not** be modified inside the loop body
> 
> **~={purple}Scheduling=~**
> - Controls how iterations are split across threads
> - **~={blue}Clause format=~**: `schedule(kind[, chunk_size])`
> - **~={red}Example=~**: `#pragma omp for schedule(static, 2)`
> - **~={green}Types=~**: `static`, `dynamic`, `guided`, `runtime`
> 	- Chunk Size is an optional positive integer
> 		- Not specified → iteration space divided equally (block style)
> 		- Specified → split into chunks of `chunk_size` and **cycled across threads**

>[!Note]- `schedule(static)`
> <!-- Multiline -->
> - Iteration distribution is **decided before loop executes**
> - Procedure:
>   1. Determine `chunk_size`
>   2. Assign `chunk_size` iterations cyclically to threads (round-robin)

>[!Note]- `schedule(dynamic)`
> <!-- Multiline -->
> - Iteration distribution is **decided during loop execution**
> - Procedure:
>   1. Determine `chunk_size`
>   2. Repeat until all iterations are done:
>      - Each thread grabs a `chunk_size` batch
>      - Executes it, then repeats

>[!Note]- `schedule(runtime)`
> <!-- Multiline -->
> - Scheduling kind is provided at **runtime via environment variable**
> - No `chunk_size` parameter in the code
> - Useful for testing different scheduling kinds without recompiling
> - Example:
>   ```bash
>   export OMP_SCHEDULE="dynamic, 4"
>   ```
>   - kind: `dynamic`
>   - chunk size: `4`

## Sections Construct

>[!Note]- `sections` construct
> <!-- Multiline -->
> - Work sharing construct for MIMD programming
> - Each section is executed by a different thread. This allows each thread to execute different code.
> - Not suitable for many small tasks (**~={green}course grain=~**)
> ```cpp
> #pragma omp sections
> {
> 	#pragma omp section
> 	structured_block
> 	#pragma omp section
> 	structured_block
> 	...
> }
>```

## Single/Master Construct

>[!Note]- `single & master` construct
> <!-- Multiline -->
> `#pragma omp single` then `structured_block`
> * Structured block is executed by only one thread
> 	* all other threads wait until it is finished (**~={red}blocking=~**)
> 	* remember, by default everything within a parallel region is executed by all threads
> 	
> `#pragma omp master` then `structured_block`
> * Structured block is only executed by master thread
> 	* all other threads ignore it and continue without waiting

## Synchronisation

>[!Note]- How to Synchronise Between Threads
> <!-- Multiline -->
> Sometimes it is necessary to synchronise between the threads. Mainly to synchronise data access. We can use 2 constructs:
> * `critical`
> * `barrier`

### Critical Region

>[!Note]- `crtical` region
> <!-- Multiline -->
> It is an OpenMP directive used to prevent multiple threads from running a block of code at the same time.
> * Only 1 thread can be in the critical section
> * Other threads must wait their turn before entering
> 
> **~={purple}How to use it?=~**
>```cpp
>int total{0};
>#pragma omp parallel {
>	#pragma omp critical {
>		total += 1;
>	}
>}
>``` 

### Barrier Region

>[!Note]- `barrier` region
> <!-- Multiline -->
> It is an OpenMP directive that creates a synchronisation point. All threads must reach the barrier before any of them can move forward,
> 
> **~={purple}How to use it?=~**
> ```cpp
> #pragma omp parallel{
    // Each thread does some work
    #pragma omp barrier  // <- All threads wait here
    // No thread goes past this point until all threads have hit the barrier
}
>```

## Data Clauses

>[!Note]- Shared vs Private Variables
> <!-- Multiline -->
> In OpenMP, variables must be declared as either **shared** (accessible by all threads) or **private** (each thread has its own copy). Correct classification prevents race conditions and is essential for writing correct and efficient parallel code.
> * **~={purple}Private=~**: Best for thread-local computations (e.g., loop indices, scratch space). No interference between threads.
> * ~={purple}**Shared**=~: Used when threads must read/write common data (e.g., input arrays or result containers).

>[!Note]- Where to Place Data Clauses
> <!-- Multiline -->
> * `#pragma omp <directive> [clauses...]` 
> * **~={red}No Loop=~**: `#pragma omp parallel private(id) shared(data)`
> * **~={red}Loop=~**: `#pragma omp parallel for private(id) shared(data)`
> 

>[!Note]- Core Data Clauses
> <!-- Multiline -->
> * `private(var1, var2, ...)`
> 	* Each thread gets an uninitialised private copy
> 	* Avoids race conditions and cache-line contention (false sharing)
> 	* Use for per-thread temporary variables
> 	* **~={red}If possible set everything to private=~**
> * `shared(var1, var2, ...)`
> 	* All threads references the **~={green}same memory=~**
> 	* Needed for shared data structures or global communication
> * `default(shared | none)`
> 	* Sets the default for unspecified variables
> 		* `shared`: variables are assumed to be `shared` unless stated otherwise
> 		* `none`: forces explicit declaration of sharing status

>[!Note]- Reduction Syntax
> <!-- Multiline -->
> * **~={green}When does Reduction Work=~**: When you are **accumulating or combining values** into a single variable across threads (e.g., summing, counting, finding min/max).
> * **~={purple}Template=~**: `reduction(operator: var1, var2, ...)`
> * **~={purple}Supported Operations=~**: +, *, -, &, |, ^, &&, ||, min, max

>[!Note]- What and How does Reduction work?
> <!-- Multiline -->
> **~={purple}What is Reduction=~**?
> Gives each thread it's own local variable. Once it's done accumulating that local variable, all the local variables gets accumulated.
> 
> **~={purple}How does Reduction Work?=~**
> 
> ```cpp
> int a[4] = {1, 2, 3, 4};
> int sum = 0;
> #pragma omp parallel for reduction(+:sum)
> for (int i = 0; i < 4; i++) {
> 	sum += a[i];
> }
>```
>
>**~={red}Assuming 2 threads are used=~**:
>1. Thread 0 processes `i = 0` and `i = 1`
>* Adds `a[0] + a[1] = 1 + 2 = 3`
>* Stores `local_sum = 3`
>2. Thread 1 processes `i = 2` and `i = 3`
>* Adds `a[2] + a[3] = 3 + 4 = 7`
>* Stores `local_sum = 7`
>3. After the loop, OpenMP reduces the local results:
>* `sum = 3 (Thread 0) + 7 (Thread 1)` 

>[!Info]- How to implement Reduction using Critical
> <!-- Multiline -->
> Let's say we had the following code snippet:
> ```cpp
> int sum{0};
> for (int i{0}; i < n; ++i) {
> 	sum += a[i];
> }
>```
>Recall the idea of reduction is that each thread get's their own local variable, they accumulate (sum in this case), and at the end we accumulate every thread's local variable.
>
>```cpp
>int sum{0};
>#pragma omp parallel private(localSum) shared(a, sum) {
>	int localSum{0};
>	#pragma omp for
>	for (int i{0}; i < n; ++i) {
>		localSum += a[i];
>	}
>	#pragma omp critical
>	sum += localSum;
>}
>```

>[!Note]- Why Reduction is better than Critical/Locking
> <!-- Multiline -->
> **~={red}Problem=~**
> * Threads may simultaneously read/modify/write to `sum`
> ```cpp
> int sum{0};
> #pragma omp parallel for shared(sum)
> for (int i = 0; i < N; i++) {
> 	sum += a[i];  // Multiple threads writing to 'sum' causes races
> }
>```
>
>**~={green}Naive Solution=~**
>* Prevents races but kills performance — threads **serialize** at the critical section.
>* High overhead due to frequent locking.
>
> ```cpp
> int sum{0};
> #pragma omp parallel for shared(sum)
> for (int i = 0; i < N; i++) {
> 	#pragma omp critical
> 	sum += a[i];
> }

>[!Note]- `firstprivate` vs `lastprivate`
> <!-- Multiline -->
> `firstprivate`
> * Thread-private variable initialised with its original value before entering the parallel region.
> 
> ```cpp
> int t{5};
> #pragma omp parallel firstprivate(t) {
> 	t *= 2; // Each thread starts with t = 5
> }
>```
>
> `lastprivate`
> * Thread-private during execution, but after the region ends, the original variable is updated with the last logical iteration's value.

# Benchmarking



# Multithreading Thread Safety

> [!QUOTE] Quick Notes
> Task Parallelism and Shared Memory Architecture. Data parallelism is about doing the same thing on multiple pieces of data. Like for loops to apply for a filter on an image.

>[!Note]- Why is allocating to the Heap expensive?
> <!-- Multiline -->
> * **~={green}Stack Allocation=~**: The heap is a large pool of memory managed manually or by a memory allocator. Allocation involves searching for a suitable free block, which adds overhead. Over time, as blocks are allocated and freed, the heap becomes fragmented, making it harder to find contiguous space. This increases the cost of allocation and deallocation.
> * **~={green}Heap Allocation=~**: The heap is a large pool of memory managed manually or by a memory allocator. Allocation involves searching for a suitable free block, which adds overhead. Over time, as blocks are allocated and freed, the heap becomes fragmented, making it harder to find contiguous space. This increases the cost of allocation and deallocation.

>[!Note]- Bottom Up of Hardware, Processors, Threads and Tasks
> <!-- Multiline -->
> ![[Drawing 2025-03-31 12.38.42.excalidraw | center | 700]] 

>[!Note]- Why number of processors should = number of cores
> <!-- Multiline -->
> ![[Drawing 2025-04-10 14.42.57.excalidraw | center]]

>[!Note]- Equal Allocation Task Allocation when Tasks have no Dependencies and are Equal with no I/O Operations
> <!-- Multiline -->
> ![[Drawing 2025-03-31 13.01.15.excalidraw | center | 500]]

>[!Note]- Forking a Process
> <!-- Multiline -->
> * Forking a process is relatively expensive compared to creating a thread, as it involves setting up a new process context, including a new stack and heap.
> * Although **Copy-On-Write (COW)** is used — meaning the memory isn’t physically copied until either the parent or child modifies it — the system still needs to duplicate metadata like page tables and establish a new address space.
> * This is in contrast to threads, which share the same memory space and only require a new stack, making them more lightweight for concurrency.
> * As such, when a thread or process is created, reuse it rather than creating and killing it over and over!

>[!Note]- Simple multithreading in C++
> <!-- Multiline -->
> The following is a showcase on how to create threads. All it does is create 2 threads that each run a nonsense for loop:
> 
>```cpp
>void computeWork(int amount) {
>    int sum = 0;
>    for (int i = 0; i < amount * 1000; ++i) {
>        sum += i;
>    }
>}
>
>class PieceOfPaper {
>public:
>    PieceOfPaper(int amount) : amount(amount) {}
>	// This function automatically runs when this object
>	// is passed to a thread
>    void operator()() const {
>        computeWork(amount);
>    }
>
>private:
>    int amount;
>};
>
>int main() {
>    std::cout << "C++ Threading Demo" << std::endl;
>    std::cout << "Main thread ID: " << std::this_thread::get_id() << std::endl;
>
>    auto startTime = std::chrono::high_resolution_clock::now();
>
>    // -- do the tasks -- sequentially
>    computeWork(5000);
>    computeWork(5000);
>
>    // Now do the same using threads
>    PieceOfPaper paper1(5000);
>    PieceOfPaper paper2(5000);
>
>    std::thread worker1(paper1);
>    std::thread worker2(paper2);
>
>    // Wait for threads to complete
>    worker1.join();
>    worker2.join();
>
>    auto endTime = std::chrono::high_resolution_clock::now();
>    auto totalTime = std::chrono::duration_cast<std::chrono::milliseconds>(endTime - startTime).count();
>
>    std::cout << "Total time = " << totalTime << " milliseconds" << std::endl;
>
>    return 0;
>}
>```

# Concurrent Collections

>[!Note]- Race Condition Example in C++
> <!-- Multiline -->
>To prevent the race condition, we want it so that if we have 2 threads both incrementing a `counter`, 1 thread first increments it, then the second thread increments. We want it to be **~={green}serial=~**.
>
> ![[Drawing 2025-04-10 17.27.36.excalidraw | center | 450]]
>
>```cpp
>class Counter {
>public:
>    void increment() {
>        ++count; // <-- Race condition: this is not atomic!
>    }
>
>    int getCount() const {
>        return count;
>    }
>
>private:
>    int count = 0;
>};
>
>class Worker {
>public:
>    Worker(Counter& counter, int id) : counter(counter), id(id) {}
>
>    void operator()() {
>        counter.increment(); // Critical section (not synchronized)
>    }
>
>private:
>    Counter& counter;
>    int id;
>};
>
>int main() {
>    std::cout << "C++ Threading Race Condition Example" << std::endl;
>
>    Counter counter;
>
>    Worker w1(counter, 1);
>    Worker w2(counter, 2);
>
>    std::thread t1(w1);
>    std::thread t2(w2);
>
>    t1.join();
>    t2.join();
>
>    std::cout << "Final value of counter = " << counter.getCount() << std::endl;
>
>    return 0;
>}
>```

>[!Note]- What does `synchronized` do in Java and why do we not use it everywhere?
> <!-- Multiline -->
> ![[Drawing 2025-04-10 18.03.14.excalidraw | center | 350]]
> 
> * **~={green}Synchronized=~**: Only one thread can run this block/method at a time per object (or per class, if it’s static)
> * We could instead put all the non-shared variables in a different method (so the second thread can access these variables when the first thread is within the synchronised method), and call that method should we need to adjust it, however, that would require creating a lot more methods.
> * The smarter solution would be to put a mutex instead, which in Java is `synchronized(this) { ++c; }`

 >[!Note]- What happens to cache performance if we have more classes, methods etc?
> <!-- Multiline -->
> It worsens as the CPU only has finite space for **~={green}instruction cache=~** and **~={green}data cache=~**.
> 
> ![[Drawing 2025-04-10 20.40.35.excalidraw | center | 200]]
> 
> Because the cache is fixed, the more methods there are, the more unloading and reloading data into the cache will be done, resulting in more cache misses.

 >[!Note]- Why does every object in Java have a monitor? What is it?
> <!-- Multiline -->
> * **~={purple}What is a monitor=~**: A monitor is a synchronisation construct that allows threads to have mutual exclusion and the ability to wait (block) and be notified. It's effectively a **~={purple}flag=~**.
> * **~={green}If it is true=~**: The monitor is owned/locked by a thread, meaning another thread trying to enter a synchronised block on the object will be blocked.
> * **~={red}If it is false=~**: The monitor is unlocked, and any thread can acquire it via a synchronised block.

> [!Note]- `synchronized(this)` vs. Fine-Grained Locking
> <!-- Multiline -->
> **~={purple}Using this (Coarse-Grained Locking)=~**:
> * ~={blue}**What it does**=~: Locks the entire object instance.
> * **~={green}Pros=~**: Simple, ensures that _all_ synchronized methods/blocks using this are mutually exclusive. So if a thread is running a synchronised method, no other **~={purple}synchronised=~** method can run concurrently
> * **~={red}Cons=~**: May overly restrict concurrency because it blocks threads even if they access independent parts of the object.
>
>~={red}**Code Example**=~:
>```java
>public class MyClass {
>    public synchronized void doSomething() {
>        // Critical section: locks the entire instance
>    }
>}
>```
>
>**~={purple}Using a Dedicated Lock Object (Fine-Grained Locking)=~**:
>* **~={blue}What it does=~**: Locks a specific private final object, allowing for more selective control.
>* ~={green}Pros=~: Enables concurrent execution of independent operations if they protect different data or logic, improving performance in multi-threaded scenarios.
>* **~={red}Cons=~**: Requires careful management to ensure that the correct lock is used consistently.
>
>~={red}**Code Example**=~:
>```java
>public class MyClass {
>    private final Object lockA = new Object();
>    private int a = 0;
>    
>    // Or we could this.a (if a is Integer) and pass it in instead
>    public void doSomething() {
>        synchronized (lockA) {
>            // Critical section: only locks on the specific lock object
>            ++a;
>        }
>    }
>}
>```

> [!Note]- Atomic Variables
> <!-- Multiline -->
> Atomic variables in Java (from java.util.concurrent.atomic package) provide lock-free, thread-safe operations on single variables. They use low-level atomic CPU instructions to ensure correctness in concurrent environments.

> [!Note]- Lock vs Monitor
> <!-- Multiline -->
> * **~={purple}Monitor=~**: Built into every object. Using synchronized(obj) loads object metadata (like the object header: identity hash, GC info, lock state, and monitor pointers) into CPU caches. Also includes support for wait(), notify(), notifyAll() → heavier on instruction/data cache.
> * **~={purple}Lock=~**: `ReentrantLock` is a standalone object. Internally just an atomic counter and a queue. ~={green}It's just a simple counter=~ → lightweight and cache-friendly.
> * **~={purple}Lock vs Monitor=~**: Prefer Lock for high-performance or advanced features (timeouts, try-lock, fairness). Use Monitor (synchronized) for simplicity in low-contention cases.

> [!Note]- Concurrent Data Structures (TODO)
> <!-- Multiline -->

# Tasking

> [!Note]- Naive vs Static vs Dynamic Task Scheduling
> <!-- Multiline -->
> ![[Drawing 2025-04-10 23.06.45.excalidraw | center | 700]]

> [!Note]- Scheduling Compute and I/O Tasks in One Queue
> <!-- Multiline -->
> 1. **~={purple}Cannot bound exec time=~**: If we have I/O tasks, we now no longer know how long each thread will take, as some threads may get stuck waiting on I/O
> 2. **~={purple}I/O does nothing in the CPU=~**: Because I/O doesn't require CPU compute, if the thread does nothing, then it leads to underutilisation of cores. Thus, though it does cost, we should create a new thread (or use an existing thread) and perform a context switch, putting the I/O thread to sleep, only waking it up when the I/O work is finished.
> 
> ![[Drawing 2025-04-11 11.49.57.excalidraw | center | 200]]

# Executor Fork Join

> [!Note]- What is Fork Join?
> <!-- Multiline -->
> A Fork Join graph if present, can easily be parallelised!
> 
> ![[Drawing 2025-04-11 21.25.31.excalidraw | center | 200]]

> [!Note]- `Executors.newSingleThreadExecutor` vs `Executor.newFixedThreadPool(numProcesses)` vs `Executors.newCachedThreadPool`
> <!-- Multiline -->
> 
> ![[Drawing 2025-04-11 20.55.36.excalidraw | center | 400]]

> [!Note]- Parallelising Recursive Functions
> <!-- Multiline -->
> ![[Pasted image 20250411214700.png | center | 500]]

# Parallelisation of Graph Algorithms in Java

> [!Note]- Why are Graph Algorithms more difficult to Parallelise compared to Linear Algebra?
> <!-- Multiline -->
> They always have dependencies, requiring the results of the previous step. Whereas in some linear-algebra problems, memory access for matrices is predictable, and they can be divided into independent blocks without the need for synchronisation.

> [!Note]- Why is Djisktra hard to parallelise?
> <!-- Multiline -->
> * Dijkstra always expands the closest unvisited node. 
> * This step must be done sequentially to ensure correctness — you can't skip ahead.
> * Concurrent access leads to lock contention or complex lock-free data structures regarding the priority queue.

> [!Note]- Java Concepts
> <!-- Multiline -->
> * `ExecutorService pool = Executors.newFixedThreadPool(numThreads);` - Creates a pool of threads
> * `CyclicBarrier barrier = new CyclicBarrier (numThreads);` - Reusable barrier
> * `CountDownLatch latch = new CountDownLatch(numThreads);` - One time barrier

> [!Note]- SSSP How to Parallelise Bellman Ford $O(V \cdot E)$
> <!-- Multiline -->
> **~={purple}Sequential Bellmanford=~**
> * We have a single distances array
> * **~={green}Initialise=~**:
> 	* `dist[v] = ∞`, `dist[src] = 0`
> * For $V - 1$ iterations:
> 	* We go through each vertex, and check if we can reach every other node in a shorter distance. The equation for this is, `d[v] = min(d[v], d[u] + w(u, v))`
>
> ![[Drawing 2025-05-24 10.13.32.excalidraw | center | 300]]
> 
> **~={purple}Parallel Bellmanford=~** 
> * We have 2 distances arrays
> 	* `distOld[]` holds the snapshot of distances from the previous iteration (we read from this)
> 	* `distNew[]` for the current iteration (we write to this). This needs to be an `AtomicIntegerArray`. Java doesn't provide an atomic `min` operation, so you'd have to use `updateAndGet` in addition with `Math.min`
> * **~={green}Initialise=~**:
> 	* `dist[v] = ∞`, `dist[src] = 0`
> * For $V-1$ iterations:
> 	* We don't have to visit the vertices in the same order in every iteration, so we can just create threads, and split up the edges among threads for edge relaxation. Operations are atomic so no race conditions.
> 	* We block the next iteration with a barrier `countDownLatch`. Synchronisation does slow down these algorithms
> 	
> ![[Drawing 2025-05-24 10.34.26.excalidraw | center | 500]]

> [!Note]- APSP How to Parallelise Floyd Warhshalls $O(V^3)$
> <!-- Multiline -->
> **~={purple}Sequential Floyd Warshall=~**
> * We use a 2D distance matrix `dist[v][v]`
> * **~={green}Initliase=~**:
> 	* `dist[i][j] = weight (i → j )` if edge exists, otherwise `∞`
> 	* `dist[i][i] = 0`
> * For every node $k$ (intermediate node)
> 	* For every pair of nodes $(i, j)$
> 		* `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`
> 		* (Checks if the path from `i` to `j` is shorter if we do `i -> k -> j` instead)
> 
> **~={purple}Parallel Floyd Warshall=~**
> * **~={red}This line=~**: For every pair of nodes $(i, j)$
> 	* Is independent. I.e. within an iteration, there are non `write-write` or `read-write` conflicts. Therefore, it's safe to divide the (i, j ) matrix into submatrices (rows or blocks) and let each thread process part of the matrix concurrently.

> [!Note]- MST How to Parallelise Boruvkas $O(ElogV)$
> <!-- Multiline -->
> * You should know. Every iteration we go through all $E$ edges. There should be around $log(V)$ iterations, since in each round, components are merged, roughly halving the number of components.
> * Each thread processes a subset of edges. To avoid race conditions when identifying the cheapest edge per component, **each thread maintains its own local `minEdge[]` array**. When copying to the global array, we perform min operations.

# Synchronisation Methods and their use in thread-safe Priority Queue in Java

> [!Note]- What is Synchronisation?
> <!-- Multiline -->
> Enforces correct order of execution

> [!Note]- Types of Synchronisation
> <!-- Multiline -->
> * **~={red}Blocking=~**: Blocking methods are those which only allow one thread to access a section of code at a time. Threads that want to access that section are forcefully blocked until they are granted access to it. Thread goes to sleep and is context switched out potentially.
> 	* Mutex
> 	* Semaphores
> 	* Monitors
> 	* Barriers
> * **~={green}Non Blocking=~**: Non-blocking methods avoid forcing threads to wait; instead, threads keep making progress without being suspended, even when accessing shared resources. More efficient as it never sleeps or yields.
> 	* Wait-Free
> 	* Lock-Free
> 	* Obstruction-Free

> [!Note]- Obstruction Free (Weakest) vs Lock Free vs Wait Free (Strongest)
> <!-- Multiline -->
> ![[Pasted image 20250524114651.png]]

### Blocking Methods

> [!Note]- Mutex
> <!-- Multiline -->
> Allows only one thread to enter a critical section at once

> [!Note]- Semaphore
> <!-- Multiline -->
> Allows up to $x$ number of threads to enter a critical section at once

> [!Note]- Read & Write Locks
> <!-- Multiline -->
> * Multiple threads can acquire this as a read lock, but only 1 thread can acquire it as a write lock.
> * Write locks can be downgraded to read locks but not vice versa
> * Good to use when you might require lots of reads but few writes

 >[!Note]- Monitor
> <!-- Multiline -->
> * **~={purple}What is a monitor=~**: A monitor is a synchronisation construct that allows threads to have mutual exclusion and the ability to wait (block) and be notified. It's effectively a **~={purple}flag=~**.
> * **~={green}If it is true=~**: The monitor is owned/locked by a thread, meaning another thread trying to enter a synchronised block on the object will be blocked.
> * **~={red}If it is false=~**: The monitor is unlocked, and any thread can acquire it via a synchronised block.

 >[!Note]- Barriers
> <!-- Multiline -->
> Prevents threads from continuing until they have all reached a "barrier" in the execution. Threads will block at a barrier until all active threads have reached it before resuming execution.
> * **~={purple}Latches=~**: 1 time use
> * **~={purple}Phasers=~**: Can be used multiple times

### Non-Blocking Methods

> [!Note]- Compare and Swap
> <!-- Multiline -->
> * Seems to be good. Hard to say if it's better than a mutex or worse. Need to benchmark. May mean you have to re-check very often.
> 
> ![[Drawing 2025-05-24 12.36.41.excalidraw | center | 700]]

> [!Note]- Test and Set
> <!-- Multiline -->
> 1. Threads will check a memory location before accessing a shared resource. 
> 2. The thread will set the a flag in the location to 1 to acquire it if it is available (mutex like)
> 3. Other threads will keep polling/waiting the memory location to try enter the critical section
> 
> This is different to a mutex as, a mutex does this:
> * When a lock is already held, any new threads requesting to access is put to sleep by the OS and placed in a wait queue. When the lock is released, the OS wakes up the waiting thread, resulting in less wasteful CPU usage.

> [!Note]- Atomic Variables
> <!-- Multiline -->
> * Thread-safe objects (e.g., AtomicInteger, AtomicBoolean, AtomicReference)
> * Provide lock-free synchronisation for single variables
> * **~={purple}Under the hood=~**:
> 	* `incrementAndGet()`, `compareAndSet()`, `get()`, `set ()`
> 	* Actually backed by a hardware instruction using CAS
> 	* If it's not an integer, boolean or long, atomic is just a software lock, akin to a database transaction, where the entire thing goes through, or none of it

### Conditional Methods

> [!Note]- Futures and Promises
> <!-- Multiline -->
> * **~={green}Promise=~**: holds the completed result
> * **~={red}Future=~**: will wait for the completed result 
> 
> **~={purple}Process=~**:
> 1. Producer thread creates a `Promise` that is shared with consumer threads
> 2. Once successful, it fulfils a `Promise` and will notify the consumer threads
> 3. Consumer threads perform other work or is blocked until the promise is fulfilled
> 4. Consumer will access or receive the future object, but not modify

> [!Note]- Condition Variables
> <!-- Multiline -->
> Blocking synchronisation primitive allows threads to wait until a particular condition occurs. Effectively, once a condition is true, it'll allow the thread to enter the critical section blocked off by the mutex.

### Methods to make Priority Queue Thread Safe

> [!Note]- Atomic Variable Approach
> <!-- Multiline -->
> 1. **~={purple}AtomicReference for entire PQ=~**
> 	* **~={green}Thread safe=~**
> 	* **~={red}But as CAS is being used, threads would be making copies of the entire PQ, which is expensive (optimistic)=~**
> 2. **~={purple}AtomicReference for all elements=~**
> 	* **~={red}Not thread-safe, since heapify is a multi-step operation that must be atomic—this approach doesn’t ensure that=~**
> 3. **~={purple}AtomicReference for head element=~**
> 	* **~={red}Doesn't work for the same reason as number 2=~**

> [!Note]- Batch Processing Approach
> <!-- Multiline -->
> * So if $n$ threads each want to perform an operation on the PQ, they all queue their operation into the **operation queue** `ConcurrentLinkedQueue`. Then **one thread** calls `combine()` and runs all 10 of those operations (including its own), **one after another**, while holding the lock.
> 1. When `combine()` is called, that thread acquires a mutex lock on the PQ.
> 2. Processes all pending operations in the operation queue.
> 3. Fulfills each Future tied to those operations.
> ```
> Thread A: creates `CompletableFuture`, puts it in queue, and holds on to it
↓
Combiner thread: completes the future later
↓
Thread A: resumes via `.get()` or callback. It gets the value it request for (i.e. top value).
>```

* 1 lock vs multiple locks and unlocks

# Parallel Iterators in C++

> [!Note]- What is an Iterator?
> <!-- Multiline -->
> Iterators provide a standard way to access different types of data structures. For example, you can use the same iterator interface to loop through a simple array or a more complex tree without changing how you write your code.

### Java

> [!Note]- Parallel Iterators in Java
> <!-- Multiline -->
> When a thread in Java calls `it.hasNext()` on a data structure, it reserves the next element for it's own thread. Other threads are blocked off, and cannot proceed with their own `next()` call if they try access the same location. Thus this serves as a synchronisation point.

> [!Note]- Random Access Collections
> <!-- Multiline -->
> **~={blue}What are Random Access Collections=~**: Arrays
> 
> **~={purple}Static (Cyclic)=~**
> * Each thread is assigned a fixed stride.
> * Threads know in advance which indices to access
> * Just need a simple boundary check to avoid going out of bounds
> * **~={red}No locks needed, but less adaptive to workload imbalances=~**
> 
> **~={purple}Dynamic/Guided=~**
> * Threads pull work from a shared index. This index marks the next chunk that needs to be worked on.
> * Requires locking to ensure only one thread claims a chunk at a time
> * After taking a chunk, thread updates index and releases the lock.
> * **~={red}Better for uneven workloads, but slower due to locking overhead.=~**
> * The chunks are still processed in the order of indices. But total output order is not guaranteed.
> 

> [!Note]- Inherently Sequential Collections
> <!-- Multiline -->
> **~={blue}What are Random Access Collections=~**: Linked Lists, Trees, Hash Maps
> 
> **~={purple}Static (Cyclic)=~**
> * Each thread is assigned a fixed stride. Each thread maintains a private subarray of references to the elements it needs to access.
> 
> **~={purple}Dynamic/Guided=~**
> * Threads pull work from a shared iterator. This iterator marks the next chunk that needs to be worked on.
> * Requires locking to ensure only one thread claims a chunk at a time
> * After taking a chunk, thread updates the iterator and releases the lock.
> 

> [!Note]- Reductions
> <!-- Multiline -->
> Thread Local Storage: Reductions are implemented using thread local storage which gives each thread its own copy of a variable. 
> 1. A ThreadLocal variable is declared to hold each thread's intermediate result.
> 2. Each thread updates its local value during computation.
> 3. After all threads complete, the main thread combines the results

> [!Note]- Loop Breaks
> <!-- Multiline -->
> **~={purple}Global Breaks=~**
> * A global break stops the entire parallel loop for all threads.
> * Once the condition is met, all threads must stop processing, even if they haven't finished their chunk. (Their `.hasNext()` will stop working)
> 
> **~={purple}Local Breaks=~**
> * A local break only stops execution within the current thread's chunk or loop.
> * Other threads continue processing their own data.
> 

### C++

> [!Note]- Sequence Containers
> <!-- Multiline -->
> **~={blue}What are Sequence Containers=~**: Arrays, Vectors, List

> [!Note]- Associative Containers
> <!-- Multiline -->
> **~={blue}What are Sequence Containers=~**: Sets

> [!Note]- Parallel Iterator Implementation
> <!-- Multiline -->
> **~={purple}Properties=~**
> * **~={blue}Scheduling Policy=~**: Static (Block, Cyclic), Dynamic
> * **~={blue}Block Size=~**:
> * **~={blue}Container=~**: What container to take in
> * **~={blue}Number of Threads=~**:
> 
> **~={purple}General Implementation=~**
> 
> ![[Drawing 2025-05-25 12.38.29.excalidraw | center | 300]]
> 
> * Each thread will have it's own queue
> * Objects in the container will be allocated (as a reference) to the threads queue
> * Policy and block size determine allocation
> * Iterating complete when all objects have been allocated and queues are empty

> [!Note]- Static Block Policy
> <!-- Multiline -->
> ![[Drawing 2025-05-25 12.40.01.excalidraw | center | 300]]

> [!Note]- Static Cycle Policy
> <!-- Multiline -->
> ![[Drawing 2025-05-25 12.42.15.excalidraw | center | 300]]
> 
> **~={purple}Copy on Demand=~**
> * A copy on demand approach can also be used, you only perform the copying from the container when it's needed rather than doing it upfront

> [!Note]- Dynamic Policy
> <!-- Multiline -->
> There is a mutex on the shared container's iterator. When a thread wants to allocate a chunk to itself, it will enter the critical section, make a copy of references to its local queue, and move the iterator forward, leaving the critical section.

# Co-routines in C++

> [!Note]- Concurrency vs Parallelism
> <!-- Multiline -->
> * **~={blue}Concurrency=~**: Multiple tasks are in progress at the same time, but not necessarily running at the same instant.
> * **~={blue}Parallelsim=~**: Multiple tasks are executing simultaneously, usually on multiple cores.

> [!Note]- Cooperative vs Non-cooperative multitasking
> <!-- Multiline -->
> * **~={green}Cooperative=~**: Threads or co-routines voluntarily yield control of the CPU. Only 1 task runs at a time. Tasks yield control explicitly, so you know exactly when context switches can happen.
> * **~={red}Non-cooperative=~**: The system/OS decides which threads or co-routines pause and switch between them automatically. Requires synchronisation to prevent race conditions.

> [!Note]- What is a Sub-routine?
> <!-- Multiline -->
> Standard functions. A calls B. Needs to wait for B to return for A to resume.

> [!Note]- What is a Co-routine?
> <!-- Multiline -->
> Co-routines (a function that can pause and resume its execution.) allow for concurrency without parallelism. This is called cooperative multitasking. Basically async functions.
> 1. Coroutine A can start coroutine B. Coroutine B may yield, transferring control back to A. A must then explicitly resume B. This allows functions to pause and resume, enabling controlled switching between them.

> [!Note]- Structured vs Non-Structured Concurrency
> <!-- Multiline -->
> If nested co-routines conclude before the outer nested co-routine, then it is structured.
> 
> ![[Pasted image 20250525151445.png | center | 500]]

> [!Note]- Stackless Co-routines
> <!-- Multiline -->
> * C++ co-routines are stackless. This means a coroutine does not have the full call stack, only it's own local frame (variables, and instruction pointer). The programmer must explicitly manage relationships and control flow between coroutines.
> 	* You must explicitly pass values back up the coroutine chain. For example, if A → B → C, and C wants to yield a value to A, it must pass that value through B manually.
> 	* In Go, goroutines are stackful and independently scheduled. Any goroutine can communicate with any other (e.g., via channels), so C can effectively send a value directly to A without going through B.

> [!Note]- What are Go Channels?
> <!-- Multiline -->
> Don't communicate by sharing memory, share memory by communicating!
> * **~={purple}Unbuffered Channels=~**: No capacity to hold values. Effectively requires a handshake between two go routines to communicate a value.
> * **~={purple}Buffered Channels=~**: Has a finite queue to temporarily store values.
> 	* Sends blocks of data only if the buffer is full
> 	* Receives blocks only if the buffer is empty

> [!Note]- Go Scheduler
> <!-- Multiline -->
> The Go scheduler follows the GMP model, comprising three main components: Goroutines (G), OS Threads (M), and Processors (P). The scheduler maps goroutines onto available threads and processors, with support for work stealing to improve load balancing.
> * **~={purple}Queue of Goroutines=~**: Each processor in Go has it's own queue of gorotuines. When a queue is empty, it tries to steal from the queues of other processors
> 
> ![[Drawing 2025-05-25 17.07.47.excalidraw | center | 400]]

> [!Note]- `co_yield` (Suspend & Return Value)
> <!-- Multiline -->
> 2. When a coroutine is first made, it is initially suspended. Thus we need to first resume it.
> 3. When resumed, it will run it's function. If it runs `co_yield(5)` it'll return back to where it was called, returning 5 to the coroutine object
> 
> ![[Drawing 2025-05-25 15.43.36.excalidraw | center | 300]]

* Go: when a thing is blocked, scheduler handles it

> [!Note]- `co_await` (Suspend & Wait)
> <!-- Multiline -->
> `co_await doSomething()`: Waits here until the function returns

> [!Note]- `co_return` (Return Value & Complete)
> <!-- Multiline -->
> Returns a final value, and concludes the coroutine.

# Image Processing in Halide

> [!Note]- What is Halide
> <!-- Multiline -->
> Halide allows you to describe the algorithm separately from how it is executed, giving you fine-grained control over performance through a separate scheduling phase. Halide has 2 phases:
> 1. **~={green}Algorithm Definition=~**: How the algorithm works
> 2. **~={green}Scheduling Phase=~**: Whether to use tiling, vectorisation etc for performance enhancements

> [!Note]- JIT vs AOT
> <!-- Multiline -->
> **JIT (Just-In-Time) compiles code at runtime, offering flexibility but with a performance cost, while AOT (Ahead-Of-Time) compiles code before execution, leading to better performance and portability.**

> [!Note]- Why we don't parallelise across the X axis / columns
> <!-- Multiline -->
> We parallelise across rows rather than columns, as each row is stored in a different `vector`, so parallelising across columns will make us load and reload in `vectors` which is expensive. So each thread gets its own row.

> [!Note]- What is Convolution
> <!-- Multiline -->
> A moving sliding window stencil across an image

### Image Denoising

> [!Note]- Median Filter
> <!-- Multiline -->
> * **~={red}Difficulty to Parallelise=~**: Harder (Has a sorting step which has a sorting cost, but still independent)
> * **~={blue}Description=~**: Takes in neighbouring values, sorts it and take the middle value.

> [!Note]- Gaussian Filter
> <!-- Multiline -->
> * **~={green}Difficulty to Parallelise=~**: Easy/Medium (similar to mean filter)
> * **~={blue}Description=~**: Copy to new 2d matrix. Borders remain the same.
>
>![[Pasted image 20250524163049.png | center | 300]]

> [!Note]- Mean Filter
> <!-- Multiline -->
> * **~={green}Difficulty to Parallelise=~**: Easy
> * **~={blue}Description=~**: Loop through the image, for each pixel, compute the average of it's neighbour values and itself, and copy the pixel value to a new 2d matrix

3x3 Kernel -> 9 Operations
1x3 and 3x1 Kernels -> 6 Operations
#### Parallelisation Strategies

* **<font color="#b4befe">Parallelisation</font>**: Giving each thread a seperate row to perform the algorithm on
* **<font color="#b4befe">Tiling</font>**: Helps improve cache locality by processing sections all at once
* <font color="#b4befe">Vectorisation</font>: Give the compiler "Blocks" of pixels to process all at once using SIMD

> [!Note]- Tiling & Parallelising
> <!-- Multiline -->
> Divides the image into submatrics:
> 
> ![[tiling.png | center | 300]]
> 
> * **~={green}Tiling=~**: These tiles can be worked on independently, and doing this helps with data locality with faster cache access, because each thread will now repeatedly accesses nearby memory addresses within its tile **~={blue}(By using tiling, we improve the cache locality since a large image cannot fit into the cache, but a small segment of it can)=~**. 
> * **~={green}Parallel=~**: With the parallel directive, each tile can be assigned a thread, where multiple rows can be computed simultaneously using threads.
> 

> [!Note]- SIMD Vectorisation
> <!-- Multiline -->
> The Halide vectorise directive will allow Halide to process multiple pixels in a single instruction. Treats a vector as a single pixel.

### Deconvolution

> [!Note]- Richardson-Lucy
> <!-- Multiline -->
> Tries to undo the blur on an image.
> * **~={purple}Parallelism within Iteration=~**: All steps (convolutions and pixel-wise division operation) are parallelisable since they operate independently over pixels.
> * **~={purple}Parallelism across iterations=~**: Not parallelisable due to loop-carried dependencies — each new estimate (2d matrix) depends on the result of the previous iteration.

# Accelerating ML on Raspberry Pi with NPU

> [!Note]- What is an NPU
> <!-- Multiline -->
> Is a specialised processor to efficiently run AI inference tasks.
> 
> ![[Drawing 2025-05-25 17.55.48.excalidraw | center | 500]]

> [!Note]- What does MAC do?
> <!-- Multiline -->
> * **~={purple}GPU=~**: We compute a bunch of dot products for matrix multiplication, and store intermediate results back to the cache, and then retrieve from cache again to compute next set of dot products.
> * **~={purple}NPU=~**: Has MAC which can multiply, add and then accumulate in one step. This avoids the need to send intermediate results back to the cache. It is however only good at matrix multiplications.
> 
> ![[Pasted image 20250525180748.png | center | 400]]

> [!Note]- CPU vs GPU vs NPU
> <!-- Multiline -->
> **~={purple}CPU=~**
> * SIMD
> * Is good at multi-tasking
> 
> **~={purple}GPU=~**
> * MIMD
> * Is good for graphics and parallel processing. It has very high throughput and is ideal for training large AI models
> 
> **~={purple}NPU=~**
> * Good for machine learning inference (not training) on devices like phones, as it consumers little power. Capable of being real-time.

> [!Note]- Processing Pipieline (Pipeline Parallelism)
> <!-- Multiline -->
> 1. 🎥 **~={purple}Video Input=~**: **Video frames are captured** and transferred into the system. This is fetched by the DMA.
> 2. 🧹 **~={purple}Pre-process Frames=~**: Resize and normalise the frames where needed.
> 3. ⚙️ **~={purple}Run Inference=~**: Inference using MACs
> 4. 🛠️ **~={purple}Post Process Frames=~**: Any post processing
> 5. 🖥️ **~={purple}Overlay Results=~**: DMA write output back to RAM
> 
> ![[Drawing 2025-05-25 20.11.27.excalidraw | center | 250]]

> [!Note]- CPU vs NPU
> <!-- Multiline -->
> The CPU approach follows a Consumer and Producer paradigm, where we have an input queue and an output queue.
> 
> **~={green}Input Queue=~**:
> * **~={purple}Producer=~**: A preprocessing thread captures frames and normalises it, putting it into the queue
> * **~={purple}Consumer=~**: The inference thread pulls from the queue and runs the ML model on it
> 
> **~={red}Output Queue=~**:
> * **~={purple}Producer=~**: When the inference thread finishes, it passes the results to the output queue
> * **~={purple}Consumer=~**: A post processing thread will dequeue and draw the overaly
> 
> The math computation is more difficult for the CPU, and has more clear bottleneckes.

# Parallel Rust Implementation of Multi-Dimensional Quadrature

> [!Note]- What is Quadrature
> <!-- Multiline -->
> Integration using numerical methods

> [!Note]- What is the curse of dimensionality?
> <!-- Multiline -->
> As the number of dimensions increases, the number of points needed to adequately cover the space grows exponentially.
> 1. In 1d (0 to 1): 10 points
> 2. In 2D ((0, 0) to (1, 1)): 100 points

> [!Note]- Cuhre Algorithm
> <!-- Multiline -->
> 1. Integrate by sampling $x$ points, and effectively drawing a rectangle below it, weighting it according to weights
> 2. Divide the region in half
> 3. Integrate the two sub regions, which ever sub region is has the largest estimated error, we perform the above for it (continue until error is within acceptable tolerence)

> [!Note]- Vegas
> <!-- Multiline -->
> 1. Same as Cuhre, but initially you start with a uniform distribution. After sampling, area's that are more higher, you mark more important in your distribution, as you want to sample there more (Peaks) } Important sampling
> 	1. Getting these wrong near higher points means will be more off

> [!Note]- Const Generics
> <!-- Multiline -->
> Parameters that are fixed at compile time. Thus, this allows the compiler to aggressively optimise (loop unrolling, SIMD etc).
> * Fixed the number of dimensions & integrands

> [!Note]- Compile Time Polymorphism
> <!-- Multiline -->
> * If the full type of an object or the return type of a function is known at compile time, the compiler can inline the entire function body rather than performing a function call. This enables aggressive optimisations like loop unrolling, SIMD vectorisation, and removal of dynamic dispatch, resulting in fast, zero-cost abstractions.
> * It can know this cause Rust of Rust's type system, at least on generics.
> * This removes the overhead of calling functions a bunch of times

> [!Note]- Sequential vs Parallel Rust Implementation
> <!-- Multiline -->
> **~={blue}Sequential=~**
> * Loop evaluates each point one by one, sequentially on a single thread
> 
> **~={blue}Parallel=~**
> * Rayon can be used to create a parallel iterator over the input points. Each thread has it's own queue and does some work stealing.
> 
> ![[Drawing 2025-05-25 22.13.07.excalidraw | center | 700]]

> [!Note]- Ahmdahls Law
> <!-- Multiline -->
> Cost of parallelism becomes less significant as we make the integrands more complicated

> [!Note]- Vegas vs Cuhre
> <!-- Multiline -->
> VEGAS is better for high-dimensional integrals because it uses random sampling with adaptive weighting, which scales more gracefully and efficiently in high dimensions, whereas Cuhre’s grid-based method suffers from the curse of dimensionality.

Cuhre Parallelisation:
1. Heap on error values
2. Each thread will pull subregions from the heap
	1. Sample it, and compute integration and error
	2. If error high enough, bisect and put in queue

> [!Note]- What do the `Send` and `Sync` traits do?
> <!-- Multiline -->
> * **~={purple}Send=~**: If a type implements the `Send` trait, it can be moved to another thread.
> * **~={purple}Sync=~**: If we want multiple threads to be able to read and access the same bit of data via a reference

#flashcards/os/highperformancecomputing