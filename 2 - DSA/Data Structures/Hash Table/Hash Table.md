> [!quote]+ Overview
>````col 
>```col-md 
> ![[hashtable.gif]]
>``` 
>```col-md 
>* A hash table stores key value pairs
>* It is an unordered data structure where insertion order is not "remembered"
>``` 
>```` 
>

# Hash Table Fundamentals

> [!info]- Hash function example
> <!-- Multiline -->
> Let's say we have the following hash function:
> $$h_{i+1} = (31 \cdot h_{i} + s[i])\mod 2^{32}$$
> 
> **~={purple}Where we multiply the current hash value with 31, then adding the add character’s numeric value.=~**
> 
> In code, this would look like:
> ```cpp
> int hashFunction(string s) {
> 	int h{0};
> 	int length = s.size();
> 	for (int i{0}; i < length; ++i) {
> 		// Overflow wraps automatically
> 		h = 31 * h + s[i];
> 	}
> 	return h
> }
>```
>Note that the math equation does not result in negative hash values, but the code block can, as a result of using signed integers $(-2^{31}$ to  $2^{31} - 1)$.
>````col
>```col-md
>~={purple}**Signed Overflow=~**
>* **~={red}Signed int before overflow=~**: 2147483647
>* **~={green}Signed int after overflow=~**: -2147483648
>
>Not defined in modern cpp, but most systems use 2's compliment for storing **~={blue}signed integers=~**.
>```
>```col-md
>~={purple}**Unsigned Overflow=~**
>* **~={red}Unsigned int before overflow=~**: 4294967295
>* **~={green}Unsigned int after overflow=~**: 0
>
>Stored in binary.
>```
>````
>
> ![[Drawing 2024-12-22 10.26.02.excalidraw | center | 600]]

> [!info]- What makes a good hash function?
> <!-- Multiline -->
> ````col
>```col-md
>**~={green}Good=~**
>```
>```col-md
>**~={red}Bad=~**
>```
>````
> 31 is good because you don't lose data, whereas something like $32 = 2^5 = (i << 5)$, which is a bit shift of 5, means we would be crushing our old data, causing bits of $h_i$ to be discarded.

> [!info]- What is load factor?
> <!-- Multiline -->
> * The more elements you have in a table, the more likely you'll have a collision
> * If table is size = m, we add m + 1 elements, there will be a collision (pigeon hole principle)
> $$Load Factor = \frac{Number of Elements in Table}{Size of Table}$$
>

# Preventing Collisions
## Chaining

> [!info]- What is chaining?
> <!-- Multiline -->

## Open Addressing

### Linear Probing

> [!info]- What is linear probing?
> <!-- Multiline -->

### Quadratic Probing

> [!info]- What is quadratic probing?
> <!-- Multiline -->

# Hash Table Complexity
## Chaining

> [!info]- Hash Table Parameters
> <!-- Multiline -->
> **~={purple}<u>Parameters</u>=~**
> ````col
>```col-md
> ```cpp
> class Vector {
> private:
> 	int* data;
> 	size_t cap;
> 	size_t size;
> }
>```
>```col-md
>* **~={purple}data=~**: Pointer to the dynamic array
>* ~={purple}**cap**=~: Total capacity of the array (max)
>* **~={purple}size=~**: Amount of elements in the array
>```
>````
> **~={green}<u>Constructor</u>=~**
> * Note that you cannot use an initialiser list to initialise dynamically allocated memory
> * To be explicit, we can use `this->`
> ````col
>```col-md
> ```cpp
> public:
> Vector(size_t initialCap) :
> 	cap{initialCap},
> 	size{0} {
> 	this->data = new int[cap];
> }
>```
>```col-md
>* **~={purple}initialCap=~**: User passes in the initial capacity of the vector
>1. Set the initial size to be 0
>2. Initialise an array to be the of size `cap`
>```
>````
> **~={red}<u>Destructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
>~Vector() {
>	delete[] data;
>}
>```
>```col-md
>1. When you allocate memory using `new[]`, you must deallocate using `delete[]`. This frees up the memory where data was pointing.
>```
>````

> [!note]- Add `(v.push_back(x))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Inserting element at the end of the vector. If the array does not have enough space:
> 1. We will create an array 2x the size 
> 2. Copy the existing values over
> 3. Reallocate memory
> 4. Insert new value, and update `size`
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void push_back(int value) {
>	if (size == this->cap) {
>		// (1) Double the Capacity
>		int newCap = 2 * this->cap;
>		int* newData = new int[newCap];
>		
>		// (2) Copy Exisiting Elements to new Array
>		for (size_t i{0}; i < size; ++i) {
>			newData[i] = this->data[i];
>		}
>		
>		// (3) Clean up Old Memory and Reallocate
>		delete[] this->data;
>		this->data = newData;
>		this->cap = newCap;
>		
>		// (4) Insert new Value
>		this->data[size++] = value;
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We have an array of $n$ length. If it's full and we add an element, we create a new array that is double the length, and copy the $n$ elements over.
>
> ~={purple}<u>**Amortised Complexity**</u>: $O(1)^A$=~
> 
>On average, the cost of resizing gets distributed over multiple append operations, resulting in an average time complexity of $O(1)$.
>

> [!note]- Remove `(v.push_back(x))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Inserting element at the end of the vector. If the array does not have enough space:
> 1. We will create an array 2x the size 
> 2. Copy the existing values over
> 3. Reallocate memory
> 4. Insert new value, and update `size`
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void push_back(int value) {
>	if (size == this->cap) {
>		// (1) Double the Capacity
>		int newCap = 2 * this->cap;
>		int* newData = new int[newCap];
>		
>		// (2) Copy Exisiting Elements to new Array
>		for (size_t i{0}; i < size; ++i) {
>			newData[i] = this->data[i];
>		}
>		
>		// (3) Clean up Old Memory and Reallocate
>		delete[] this->data;
>		this->data = newData;
>		this->cap = newCap;
>		
>		// (4) Insert new Value
>		this->data[size++] = value;
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We have an array of $n$ length. If it's full and we add an element, we create a new array that is double the length, and copy the $n$ elements over.
>
> ~={purple}<u>**Amortised Complexity**</u>: $O(1)^A$=~
> 
>On average, the cost of resizing gets distributed over multiple append operations, resulting in an average time complexity of $O(1)$.
>

> [!note]- Get/Access `(v.push_back(x))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Inserting element at the end of the vector. If the array does not have enough space:
> 1. We will create an array 2x the size 
> 2. Copy the existing values over
> 3. Reallocate memory
> 4. Insert new value, and update `size`
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void push_back(int value) {
>	if (size == this->cap) {
>		// (1) Double the Capacity
>		int newCap = 2 * this->cap;
>		int* newData = new int[newCap];
>		
>		// (2) Copy Exisiting Elements to new Array
>		for (size_t i{0}; i < size; ++i) {
>			newData[i] = this->data[i];
>		}
>		
>		// (3) Clean up Old Memory and Reallocate
>		delete[] this->data;
>		this->data = newData;
>		this->cap = newCap;
>		
>		// (4) Insert new Value
>		this->data[size++] = value;
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We have an array of $n$ length. If it's full and we add an element, we create a new array that is double the length, and copy the $n$ elements over.
>
> ~={purple}<u>**Amortised Complexity**</u>: $O(1)^A$=~
> 
>On average, the cost of resizing gets distributed over multiple append operations, resulting in an average time complexity of $O(1)$.
>

> [!note]- Contains `(v.push_back(x))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Inserting element at the end of the vector. If the array does not have enough space:
> 1. We will create an array 2x the size 
> 2. Copy the existing values over
> 3. Reallocate memory
> 4. Insert new value, and update `size`
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void push_back(int value) {
>	if (size == this->cap) {
>		// (1) Double the Capacity
>		int newCap = 2 * this->cap;
>		int* newData = new int[newCap];
>		
>		// (2) Copy Exisiting Elements to new Array
>		for (size_t i{0}; i < size; ++i) {
>			newData[i] = this->data[i];
>		}
>		
>		// (3) Clean up Old Memory and Reallocate
>		delete[] this->data;
>		this->data = newData;
>		this->cap = newCap;
>		
>		// (4) Insert new Value
>		this->data[size++] = value;
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We have an array of $n$ length. If it's full and we add an element, we create a new array that is double the length, and copy the $n$ elements over.
>
> ~={purple}<u>**Amortised Complexity**</u>: $O(1)^A$=~
> 
>On average, the cost of resizing gets distributed over multiple append operations, resulting in an average time complexity of $O(1)$.
>

#flashcards/dsa/ds/hashtable