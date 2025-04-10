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
> Is a data structure maintained by the operating system to store all the information about a particular process. Both PCB and TCB are stored in the kernel space, in a data structure called a process list.
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
>1. **~={green}New Data Structure (PCB & TCB)=~**: When you start a new process from i.e. the shell (`./app`), the shell's PCB and TCB gets duplicated for this new process (COW), and are stored in **~={red}kernel space=~** of the ram.
>2. **~={green}Memory Allocation=~**: The child is assigned its own virtual address space.
>3. **~={green}CPU Register Setup=~**: The kernel sets up the child’s registers (including the program counter) so it can begin execution right after `fork()` returns in the child.
>4. **~={green}Mark as Runnable=~**: The child’s process state is then set to **~={red}runnable=~** so that it can be scheduled for the CPU to act on.
>
>![[Drawing 2025-02-22 20.52.55.excalidraw | center | 450]]
>``` 
>```col-md 
>**~={purple}Exec=~**
>
>1. **~={green}Replace=~**: Once it has ran, the old program will run `exec()`, replacing the old code with the code of the new program. The stack and heap are reinitalised to start from the beginning for the new program.
>
> ![[Drawing 2025-02-22 21.05.14.excalidraw | center | 700]]
>``` 
>```` 

> [!note]+ How is a Thread Created in Unix
> <!-- Multiline -->
>1. ~={green}Create new TCB=~: Create brand new TCB and store it in kernel space
>2. ~={green}Thread Register Values=~: Program Counter, Stack Pointer, etc are initialised in TCB
>3. ~={green}Mark as Runnable=~: Set thread’s state to runnable, making it eligible for scheduling.
>
>Additionally, the thread inherits the process’s resources (code, data, heap, and open files). This isn't making a copy, it simply share's the resource.

>[!Note]- What is a Context Switch?
> <!-- Multiline -->
> ![[Drawing 2025-02-22 22.10.40.excalidraw | center | 500]]
> 
> **~={purple}Context Switch=~**
> * **~={green}Definition=~**: A context switch is the process where the CPU saves the current process's state and loads the state of another process or thread, allowing multitasking.
> * We load the values of the PCB into a VCPU (a data structure), where the virtual CPU is mapped to an actual core
> * **~={green}Steps=~**:
> 	1. **~={blue}Save Current State=~**: CPU registers (program counter, stack pointer, general-purpose registers) are saved to the current process's PCB/TCB. (So that if we go back to this program, we can reload this info)
> 	2. ~={blue}**Update Schedular**=~: The current process's state is updated (e.g., marked as ready or waiting) in the scheduler.
> 	3. **~={blue}Select New Process=~**: The scheduler picks the next process or thread from the ready queue.
> 	4. **~={blue}Restore New State=~**: The saved register values from the selected process’s PCB/TCB are loaded into the CPU. This is enough to switch to a different program.
> 	5. **~={blue}Resume Execution=~**: The CPU continues running from the new process’s instruction pointer.

# Inter-Process Communication

>[!Note]- Common IPC Methods
> <!-- Multiline -->
> **~={purple}What is IPC?=~**
> Processes are independent. If we want 2 processes to communicate with each other, we do this via IPC.
> 
> **~={blue}Shared Memory (Fastest)=~**
> * **~={purple}What it is=~**: A region of memory both processes have access to. Processes directly read/write to the same physical memory. Synchronisation/locks are necessary.
> * **~={purple}How it works=~**: The PCB's for both processes have access to the same pages
> * **~={green}Speed=~**: Fastest—minimal overhead since no copying or kernel mediation is required.
> ![[Drawing 2025-02-22 22.45.29.excalidraw | center | 250]]
> 
> **~={blue}Pipes (Second)=~**
> * **~={purple}What it is=~**: A unidirectional channel for inter-process communication that allows one process to write data and another to read it. There are no boundaries, we are writing bytes, not discrete units. Thus the application must define what parts of the file to read.
> * **~={purple}How it works=~**: Through `Fork()`, parent and child have access to the same file, so they can communicate via that.
> * **~={orange}Speed=~**: Fast, though slightly slower than shared memory due to the overhead of copying data into and out of the kernel buffer.
> 
>  ![[Drawing 2025-02-22 22.59.08.excalidraw | center | 250]]
>  
> **~={blue}Message Queues (Third)=~**
> * **~={purple}What it is=~**: A kernel-managed mechanism for exchanging discrete messages between processes, where each message is kept as a distinct unit.
> * **~={purple}How it works=~**: The kernel maintains a message queue in kernel space where processes can post messages using system calls (such as `mq_send()` or `msgsnd()`). Other processes then retrieve these complete messages with corresponding system calls (like `mq_receive()` or `msgrcv()`). The kernel manages the queue—maintaining message boundaries and order (or priority)—so that each message remains a distinct unit that other processes can access when needed.
> * **~={red}Speed=~**: Moderately fast, but generally slower than pipes and shared memory because of additional overhead from copying messages and managing the message queue.
> 
> ![[Drawing 2025-02-22 22.48.23.excalidraw | center | 250]]
> 

#flashcards/os/processandthreads