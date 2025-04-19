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

### Practical Parallelisation Approach

>[!Note]- Communication Costs
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

>[!Note]- When can we sap loop order
> <!-- Multiline -->
> When there is no flow dependence.

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
>Where we can flow inter loop flow dependencies from:
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
> * **~={green}Pros=~**: Good for irregular workloads when execution times vary strongly. Also helps with load balancing among threads.
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
> We treat the loop body as a task graph. 

>[!Note]- Loop Pipelining
> <!-- Multiline -->
> We treat the loop body as a task graph. 

# OpenMP



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
> * **~={green}If it is true=~**: The monitor is owned/locked by a thread, meaning another thread trying to synchronized on the object will be blocked.
> * **~={red}If it is false=~**: The monitor is unlocked, and any thread can acquire it via a synchronized block.

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
> * **~={purple}Lock=~**: ReentrantLock is a standalone object. Internally just an atomic counter and a queue. ~={green}It's just a simple counter=~ → lightweight and cache-friendly.
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


#flashcards/os/highperformancecomputing