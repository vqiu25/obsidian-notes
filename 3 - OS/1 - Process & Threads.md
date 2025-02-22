# Operating Systems

>[!Note]- How does an OS communicate with hardware
> <!-- Multiline -->
> ![[Drawing 2025-01-05 09.16.43.excalidraw | center | 500]]

>[!Note]- What is a Program?
> <!-- Multiline -->
>* ~={green}**Program**=~: A program is a sequence of instructions written in a programming language. It is then interpreted or compiled into binary code.
>
> ![[Drawing 2025-01-05 11.15.14.excalidraw | center | 700]]

>[!Note]- What is the purpose of an OS?
> <!-- Multiline -->
> The primary purpose of an operating system (OS) is to **virtualise the hardware** for software to use. In other words, it allows software to assume that there are **infinite CPU cores, memory, and other resources** available.
> * **~={green}Simplifies software development=~**: Don't need to manually allocate memory, CPU scheduling...
> * **~={green}Supports concurrency=~**: Allows multiple applications to run simultaneously by managing process scheduling.
> * ~={green}Maximises hardware use=~: If programs assumed limited resources, they would be under-optimised or conflict over resources.

# Program, Processes and Threads

>[!Note]- Process and Thread Relationship
> <!-- Multiline -->
>* **~={green}Process=~**: A process is simply an instance of a program that is being executed
>* **~={green}Thread=~**: A thread is a unit of execution within a process
> 
>````col
>```col-md
>**~={purple}Process Control Block PCB=~**
> Is a data structure maintained by the operating system to store all the information about a particular process. Both PCB and TCB are stored in the kernel space.
> 
> ![[Drawing 2025-01-05 12.21.09.excalidraw | center | 150]]
>```
>```col-md
>* ~={blue}Process ID=~: A unique identifier assigned by the OS
>* ~={blue}Process State=~: Indicates whether the process is new, ready, running, waiting/blocked, or terminated.
>* ~={blue}Memory Management Information=~:
>	* ~={red}Base/Limit Registers=~: Base holds the starting physical memory address, and limit indicates the maximum offset from the base.
>	* ~={red}Page Table or Segment Table Info=~: The PCB holds a pointer to the page table, which maps the process's virtual addresses to physical memory frames. The page table entries indicate where each virtual page is stored—whether it's in RAM or needs to be loaded from disk (for example, for code, data, or the heap).
>* ~={blue}Scheduling Information=~: Indicates the scheduling priority level of the process, which helps determine its order in the scheduling queue by the CPU
>````
> ​
>````col
>```col-md
>**~={purple}Thread Control Block TCB=~**
>Threads exist in a process and share many of the resources at the process level. Each process has at least one thread.
>
> ![[Drawing 2025-01-05 12.09.33.excalidraw | center | 150]]
>```
>```col-md
>* ~={blue}Thread ID=~: A unique identifier for the thread
>* ~={blue}Thread State=~: Indicates whether the process is new, ready, running, waiting/blocked, or terminated.
>* ~={green}Registers=~: This stores the value which would be loaded into a register, such that it can be loaded into the register later. This allow's a CPU's registers to be used by multiple threads through context switches.
>* ~={green}Program Counter=~: The address of the next instruction to be executed by the thread. (What line in code do we run next)
>* ~={green}Stack Pointer=~: Points to the top of the thread’s own stack. Each thread typically has its own stack space which stores temporary data like local variables.
>* ~={blue}Pointer to PCB=~: Threads belong to a process, so the TCB often has a reference to the parent process’s PCB.
>````
>​
>````col
>```col-md
>**~={purple}Conceptual Memory Space of a Process=~**
>* Each process's memory space is independent of one another
>
> ![[Drawing 2025-01-05 10.56.03.excalidraw]]
>```
>```col-md
>**~={purple}Accessed via Page Table pointer from PCB=~**:
>* ~={blue}Code Segment=~: Contains the compiled code.
>* ~={blue}Data Segment=~: Stores global variables, static variables, and static data allocated at compile time.
>* ~={blue}Heap=~: The region from which memory is dynamically allocated at runtime.
>
>* ~={green}Stack=~: Each thread has its own stack that stores local variables, function parameters...
>
>~={purple}Not accessed via Page Table pointer=~:
>* ~={green}Registers=~: Temporary store of register values.
>* ~={green}Thread=~: A process may contain more than 1 thread.
>````

> [!note]+ How is a Process Created in Unix
> <!-- Multiline -->
>````col 
>```col-md 
>**~={purple}Fork=~**
>
>1. **~={green}New Data Structure (PCB & TCB)=~**: When you start a new process from i.e. the shell (`./app`), the shell's PCB and TCB gets duplicated for this new process, and are stored in **~={red}kernel space=~** of the ram.
>2. **~={green}Memory Allocation=~**: The child is assigned its own virtual address space.
>3. **~={green}CPU Register Setup=~**: The kernel sets up the child’s registers (including the program counter) so it can begin execution right after `fork()` returns in the child.
>4. **~={green}Mark as Runnable=~**: The child’s process state is then set to **~={red}runnable=~** so that it can be scheduled for the CPU to act on.
>``` 
>```col-md 
>**~={purple}Exec=~**
>
>1. **~={green}Replace=~**: Once it has ran, the old program will run `exec()`, replacing the old code with the code of the new program. The stack and heap are reinitalised to start from the beginning for the new program.
>``` 
>```` 
>

> [!note]+ How is a Thread Created in Unix
> <!-- Multiline -->
>

>[!Note]- Difference between Forking and Creating new Threads?
> <!-- Multiline -->

>[!Note]- What is a Context Switch?
> <!-- Multiline -->

# Thread Lifecycle


# Inter-Process Communication


#flashcards/os/processandthreads