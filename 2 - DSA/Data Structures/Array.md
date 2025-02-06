> [!quote]+ Overview
> <!-- Multiline -->
>````col 
>```col-md 
> ![[array.png]]
>``` 
>```col-md 
>An **array** is a **contiguous block of memory** that stores multiple elements of the same type. Each element in the array is stored sequentially in memory, one after another, without gaps.
>``` 
>```` 
>

# What is an Array?

An array is a **contiguous block of memory** used to store multiple elements of the same type. Each element is stored sequentially without gaps.

**~={red}Example=~**:

```cpp
int arr[3] = {10, 20, 30};
```

````col
```col-md
![[Drawing 2024-12-19 21.41.28.excalidraw | center | 150]]
``` 
```col-md
* The base address is 1000
* Each element is placed consecutively in memory.
```
````
# Where Does the Array Reside?

The location of an array depends on how it is allocated:

````col
```col-md
**~={blue}Stack=~**

Statically allocated arrays are stored in the stack. Memory is automatically managed.
```cpp
int arr[3] = {110, 20, 307};
```
```col-md
**~={blue}Heap=~**

Dynamically allocated arrays are stored in the heap and require manual memory management.
```cpp
int* arr = new int[3]; // Heap 
delete[] arr; // Manual cleanup
```
````
Both segments reside in RAM, which the CPU accesses during program execution.

# How the CPU Accesses Arrays

The CPU uses the base address of the array and calculates the address of an element using its index:

$$
\text{Address of } \text{arr}[i] = \text{Base Address} + (i \cdot \text{Size of Element})

$$

The calculated address is then used to retrieve or modify the element in RAM.