> [!quote]+ Overview
> <!-- Multiline -->
>````col 
>```col-md 
> ![[list.gif]]
>``` 
>```col-md 
>* Doubly Linked List
>* Each node has a pointer to it's previous and next node
>
> ![[Drawing 2024-12-24 14.33.06.excalidraw | center]]
>``` 
>```` 
>

# List Fundamentals

> [!info]- Linked List vs Dynamic Array
> <!-- Multiline -->
> * **~={green}Advantage=~** of Linked List:
> 	* Not fixed size
> 	* Insertion, and removal can be O(1) if you have a direct node reference (i.e. the node before the one you want to insert/remove)
> * **~={red}Disadvantage=~** of Linked List:
> 	* No random access
> 	* Cache Locality:
> 		* **~={red}Linked List=~**: Elements are typically allocated separately and linked via pointers, so they are scattered in memory, resulting in _poor cache locality_.
> 		* ~={green}**Dynamic Array**=~: Stores elements contiguously, which leads to _better cache locality_ (fewer cache misses and faster iteration).

# List Complexity

> [!info]- List Parameters
> <!-- Multiline -->
> **~={purple}<u>Parameters</u>=~**
> ````col
>```col-md
> ```cpp
> class Vector {
> private:
> 	struct Node {
> 		int data;
> 		Node* prev;
> 		Node* next;
> 		// Default values for pointers is null
> 		Node(int value, Node* p = nullptr, Node* n = nullptr) :
> 			data{valeu}, 
> 			prev{p},
> 			next{n} {}	
> 	};
> 	Node* head;
> 	Node* tail;
> 	size_t size;
> }
>```
>```col-md
>* **~={purple}Node Struct=~**: Each node contains a value, and stores a reference to both the next node and prior node.
>* **~={purple}head=~**: Pointer to the start of the list
>* ~={purple}tail=~: Pointer to the end of the list
>* **~={purple}size=~**: Amount of elements the list
>```
>````
> **~={green}<u>Constructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
> List() :
> 	head(nullptr),
> 	taill(nullptr) {
> 	this->size{0};
> }
>```
>```col-md
>Create a head and tail pointer to keep track of the first and last node. Initially, when the list has not been created, set head and tail to null, and size to O.
>```
>````
> **~={red}<u>Destructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
>~List() {
>	Node* current = head;
>	while (current != nullptr) {
>		Node* toDelete = current;
>		current = current->next;
>		delete toDelete;
>	}
>	this->head = nullptr;
>	this->tail = nullptr;
>	this->size = 0;
>}
>```
>```col-md
>1. Iterate through the entire linked list and deallocate each node
>2. Reset the `head` and `tail` pointer.
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

> [!note]- Remove `(v.pop_back())`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Removing the last element from the vector. If we remove an element but size is less than half the size of the current array:
> 1. We will create an array half the size 
> 2. Copy the existing values over, but the last
> 3. Reallocate memory
> 4. Remove the value, and update `size`
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void pop_back() {
>	--size;
>	if (size == this->cap / 2) {
>		// (1) Double the Capacity
>		int newCap = this->cap / 2;
>		int* newData = new int[newCap];
>		
>		// (2) Copy Exisiting Elements to new Array, but the last
>		for (size_t i{0}; i < size; ++i) {
>			newData[i] = this->data[i];
>		}
>		
>		// (3) Clean up Old Memory and Reallocate
>		delete[] this->data;
>		this->data = newData;
>		this->cap = newCap;
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We have an array of $n$ length. If it's only half full, and we copy $n/2$ elements to a new array, $O(n/2)$ = $O(n)$.
>

> [!note]- Get `(v[i])`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Retrieve element at index $i$:
> 1. Note that we use `int&` as our return type, meaning we are returning a reference rather than a copy of the value. This allows for efficient access and modification.
> 2. We use operator[] to provide a concise and intuitive way to access elements of the array, mimicking the behaviour of standard arrays
> 3. Check if the index is valid or not.
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>//(1, 2) Method Signature
>int& operator[](size_t index) {
>	--size;
>	//(3) Check if index is valid
>	if (index >= this->size) {
>		throw std::out_of_range("Index out of range.");
>	}
>	return this->data[index];
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(1)$=~**
> 
> We can directly access the element via it's index.

> [!note]- Set `(v[i] = x)`
> <!-- Multiline -->
 **~={purple}<u>Description</u>=~**
> 
> Set element at index $i$:
> 1. Check if the index is valid or not.
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void set(size_t index, int value) {
>	--size;
>	//(1) Check if index is valid
>	if (index >= this->size) {
>		throw std::out_of_range("Index out of range.");
>	}
>	this->data[index] = value;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(1)$=~**
> 
> We can directly access the element via it's index.

#flashcards/dsa/ds/list