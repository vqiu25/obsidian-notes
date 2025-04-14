# Memory Hierarchy



# Memory Types



# Virtual Memory

>[!Note]- What does Virtual Memory try to resolve?
> <!-- Multiline -->
> Without virtual memory, all processes have access to the same physical memory space, which could cause the following:
> * Segmented memory is basically the second one. Segments of memory are swapped between the disc and main memory, which can cause fragmentation (this is because a segment in memory can only be replaced by a segment of the same size or smaller)
> 
> ![[Drawing 2025-04-13 16.38.58.excalidraw | center | 600]]

>[!Note]- What is Virtual Memory?
> <!-- Multiline -->
> **~={purple}What is Virtual Memory?=~**
> Virtual memory is a memory management technique where each process has its own isolated virtual address space, which is mapped to physical RAM (and disk if needed), giving the illusion of a large, continuous memory space even if the actual RAM is limited.
> 
> To be more concrete. If we are on a 32-bit architecture chip, each virtual address is 32 bits long, which means, there are 2^32 addresses, where each address refers to 1 byte. This means each process has it's own page table that spans 2^32 virtual addresses.
> 
> ![[Pasted image 20250413165848.png | center | 450]]
> 
> **~={purple}What if there isn't enough RAM/Physical Memory?=~**
> The disk can be used. In that case, the mapping for some values will point to disk instead. This is called ~={green}**swap memory**=~. If we have to go to the disk though, it is slow, we call this a **~={red}page fault=~**.

>[!Note]- How does Virtual Memory solve `Not enough memory`
> <!-- Multiline -->
> **~={purple}What if there isn't enough RAM/Physical Memory?=~**
> The disk can be used. In that case, the mapping for some values will point to disk instead. This is called ~={green}**swap memory**=~. If we have to go to the disk though, it is slow, we call this a **~={red}page fault=~**.
> 
> ![[Drawing 2025-04-13 17.07.53.excalidraw | center | 500]]

>[!Note]- How does Virtual Memory solve `Memory fragmentation`
> <!-- Multiline -->
> **~={purple}What if we want to use non-contiguous physical memory?=~**
> From the perspective of each program, it has an infinite amount of contiguous memory, and thus even if its get mapped to the physical memory which may not be contiguous, that's fine!
> 
> ![[Pasted image 20250413172538.png | center | 450]]

>[!Note]- How does Virtual Memory solve `Security`
> <!-- Multiline -->
> **~={purple}What if 2 processes write to the same memory address?=~**
> The same virtual memory address doesn't map to the same physical address. They can still communicate via shared memory via `mmap`, but do note that processes don't share a stack or heap

>[!Note]- Pages & Page Tables
> <!-- Multiline -->
> **~={purple}What is a Page Table?=~**
> A page table maps virtual memory addresses to the physical memory. Every process has it's own page table.
> 
> **~={purple}Why do we not have a page entry for every possible virtual address?=~**
> Assuming we are working in a 32-bit architecture, that would mean each program has a 2^32 virtual addresses. If each page entry takes up 32 bits = 4 bytes, that would be GB's worth of space for each page table. Thus we split the total number of addresses into chunks called pages.
> 
> **~={purple}What are pages?=~**
> Pages are sections of virtual memory addresses, addressing the concern above. Typically a page is 4KB/covers 4K virtual memory addresses. In this case, there are only 2^32/4KB (2^12) = 2^20 pages. And if each entry is 4 bytes, then 2^20 * 4 = 4 MB for each page table.
> 
> **~={red}Example=~**:
> * If our virtual memory address is $4096 + 104 = 4200$, within page 1
> * The actual physical page is in page 2
> * Then the actual physical memory address starts from page 2 at the same offset which would be $8192 + 104 = 8296$
> 
> ![[Pasted image 20250413200440.png | center | 600]]

>[!Note]- How does a virtual page number get translated to a physical page number?
> <!-- Multiline -->
> * The page table will contain a mapping for the first 20 bits of the virtual memory address to the first 20 bits of the physical memory address. The remaining 12 bits is the offset.
> * Since addresses are 32 bits. The last 12 bits of the virtual and physical address are the same, as they have the same offset. Only the page number is different (the first 20 bits).
> 
> ![[Pasted image 20250413204257.png | center | 550]]

>[!Note]- What is the Translation Lookaside Buffer (TLB)
> <!-- Multiline -->
> When we want to access a particular memory address, we have to find it in the page table, and then access it. This can be expensive as we need to load in the page table into memory. Thus we introduce the TLB! The TLB is a cache, which caches both instructions and data. When the CPU wants to access the physical memory, it'll first check the TLB, if it isn't present, it'll load it from the page table and store it in the TLB.

>[!Note]- What is the Memory Management Unit (MMU)
> <!-- Multiline -->
> Is a hardware component, closely connected to the CPU. The OS is responsible for creating and managing page tables, but it is the MMU that is responsible for translating the virtual memory addresses to the physical addresses from the TLB. We use an MMU as doing it via software is too slow.

>[!Note]- Paging vs Segmentation
> <!-- Multiline -->
> Segmentation memory makes an entire block of code available to the processor which allows for fast access. This can be good if you don't need to use much ram. Otherwise, go for paging.

# Allocating Memory & Smart Pointers


# Cache and Memory Locality



#flashcards/os/memory
