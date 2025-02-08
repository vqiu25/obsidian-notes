# Program, Processes and Threads

>[!Note]- How does an OS communicate with hardware
> <!-- Multiline -->
> ![[Drawing 2025-01-05 09.16.43.excalidraw | center | 500]]

>[!Note]- What is a Program?
> <!-- Multiline -->
>* ~={green}**Program**=~: A program is a sequence of instructions written in a programming language. It is then interpreted or compiled into binary code.
>
> ![[Drawing 2025-01-05 11.15.14.excalidraw | center | 700]]

>[!Note]- Process and Thread Relationship
> <!-- Multiline -->
>* **~={green}Process=~**: A process is simply an instance of a program that is being executed
>* **~={green}Thread=~**: A thread is a unit of execution within a process
> 
>````col
>```col-md
>**~={purple}Process Control Block PCB=~**
>
> ![[Drawing 2025-01-05 12.21.09.excalidraw | center | 150]]
>```
>```col-md
>* Process ID:
>* Process State:
>* Memory Pages:
>* ...
>````
> ​
>````col
>```col-md
>**~={purple}Thread Control Block TCB=~**
>
> ![[Drawing 2025-01-05 12.09.33.excalidraw | center | 150]]
>```
>```col-md
>* Thread ID:
>* Thread State:
>* Registers:
>* Program Counter:
>* Stack Pointer:
>* Pointer to PCB:
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
>* Code Segment:
>* Data Segment:
>* Heap:
>* Stack:
>* Registers:
>* Thread:
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

Context Switch
# Thread Lifecycle


# Inter-Process Communication


#flashcards/os/processandthreads