# Virtualising the CPU

>[!Note]- States of a Process/Thread
> <!-- Multiline -->
> ![[Drawing 2025-02-23 08.24.30.excalidraw | center | 450]]
> * **~={purple}New=~**: This state represents a newly created process that hasn’t started running yet. It has not been loaded into the main memory, but its process control block (PCB) has been created, which holds important information about the process.
> * **~={purple}Ready=~**: A process in this state is ready to run as soon as the CPU becomes available. It is waiting for the operating system to give it a chance to execute.
> * **~={purple}Running=~**: This state means the process is currently being executed by the CPU.
> 	* **~={blue}Running <-> Ready=~**: This is done via **~={green}Context Switch=~**
> 	* A **~={red}runnable process=~** means it is either **~={purple}running=~** or ~={purple}ready=~ to run. We usually have many runnable processes, a but a CPU core can only run one process at a time. By context switching between different processes fast, we can achieve multi tasking (i.e. Swapping between different apps)
> * **~={purple}Blocked/Waiting=~**: This state means the process cannot continue executing right now. It is waiting for some event to happen, like the completion of an input/output operation (for example, reading data from a disk).
> * **~={purple}Exit/Terminate=~**: A process in this state has finished its execution or has been stopped by the user for some reason. At this point, it is released by the operating system and removed from memory.

>[!Note]- Hypervisor, vCPU, and Scheduler/Dispatcher
> <!-- Multiline -->
> ![[Drawing 2025-02-23 09.04.42.excalidraw | center | 450]]
> 
> * **~={purple}Hypervisor=~**: If you have an OS with multiple virtual machines, a hypervisor is a software that allocates vCPUs, memory, and other resources to each VM. For example, if a VM is allocated 2 vCPUs, then the processes in this VM have to share those 2 vCPUS.
> * **~={red}vCPU=~**: This is a data structure stored in the kernel space. When a context switch occurs, the PCB and TCB register values are stored into the vCPU's registers, which can be loaded into the actual CPU core.
> 	* **~={red}Stores=~**: Register values + What core the vCPU is currently assigned to
> 	* **~={red}Amount=~**: Infinite. This is how the CPU is virtualised. 
> 	* **~={red}Limit=~**: However, the number of active vCPUs is limited by the number of cores the CPU has
> * **~={blue}Scheduler=~**:
> 	* **~={blue}Purpose=~**: Decides which process or thread should run next based on scheduling policies. Basically is a queue (or multiple queues) of ready processes/threads.
> * **~={green}Dispatcher=~**:
> 	* **~={green}Purpose=~**: Performs the actual context switching, moving control from one process or thread to another.

>[!Note]- Scheduling Process
> <!-- Multiline -->
> ![[Drawing 2025-02-23 10.50.53.excalidraw | center | 700]]
> 1. **~={purple}Process Creation=~**: If a new process is created and requests to run, it is marked as Ready in the PCB and put in the Ready Queue which is in kernel space
> 2. **~={pink}Scheduler Picks a Process and vCPU=~**: The **CPU Scheduler** selects one process from the Ready Queue based on its scheduling algorithm. It will also pick an available vCPU and map it to an available core.
> 3. **~={orange}Dispatcher performs Context Switch=~**: 
> 	1. The **Dispatcher** saves the current process’s state (registers, program counter, etc.) back into its PCB (if another process was running).
> 	2. The Dispatcher then **loads the new process’s state** from its PCB (or TCB) into the CPU registers (first vCPU, then actual CPU).
> 4. **~={pink}Process Executes on Core=~**: The chosen process (now mapped to a physical core or a vCPU) **runs** until it gets blocked (moves to Blocked queue), or it's allocated time on the core is finished (Completes, or gets put back into the Ready Queue if not finished).

> [!Note]- Preemptive vs Cooperative Multitasking
> <!-- Multiline -->
>````col 
>```col-md 
>**~={purple}Preemptive (Modern)=~** 
>* Each process is assigned a specific amount of CPU time.
>* Upon reaching the time limit, the CPU halts the current process and work on other processes.
>* **~={green}Pros=~**: Every process will be executed more or less
>``` 
>```col-md 
>**~={purple}Cooperative=~**
>* Involves processes voluntarily yielding control of the CPU. Requires processes to be considerate
>* **~={red}Cons=~**: A selfish process can make the computer unresponsive.
>``` 
>```` 
>

# Scheduling Strategies

> [!Note]- Policies
> <!-- Multiline -->
> 1. First Come First Serve
> 2. Shortest Job First
> 3. Preemptive Shortest Job First
> 4. Round Robin Scheduling
> 5. Priority Scheduling
> 6. Multilevel Queue Scheduling

#flashcards/os/cpuscheduling