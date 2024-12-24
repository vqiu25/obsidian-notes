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

> [!info]- Iterating through Linked List
> <!-- Multiline -->
> * Create a `Node* current = this->head`
> * And iterate $n$ times via a for loop, performing `current = current->next`

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
> 			data{value}, 
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

> [!note]- Add `(l.push_back(x))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Inserting element at the end of the list. General Process:
> 1. Create brand new node
> 2. If there does not exist any node, create a node, and set both the head and tail to point at it.
> 3. Otherwise:
> 	1. Prev of new node to be old tail
> 	2. Next of old tail to be new node
> 	3. Tail to point at new node
>
>**~={purple}<u>Code</u>=~**
>
> ![[Drawing 2024-12-24 15.02.48.excalidraw | center | 700]]
>```cpp
>void push_back(int value) {
>	// (1) Create new node
>	Node* newNode = new Node{value};
>	
>	// (2) If the linked list is empty
>	if (this->head == nullptr) {
>		this->head = newNode;
>		this->tail = newNode;
>	} else {
>		// (3) Otherwise, from the tail pointer, add to end
>		// Point previous of new node to the old tail
>		newNode->prev = this->tail
>		// Point the old tail to the new node
>		this->tail->next = newNode;
>		// Point the new tail to the new node
>		this->tail = newNode;
>	}
>	this->size++;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> $O(1)$ if we have a tail pointer. Otherwise, $O(n)$. 
>

> [!note]- Remove `(l.erase(pos))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> There are 3 cases. 
> 1. **~={blue}Removing head=~**: If we want to remove the head node, we set the head pointer to the following node. Note that we set the value before the head to be nullptr.
> 3. **~={blue}Removing tail=~**: If we want to remove the tail node, we set the tail to the be the current tails previous node. Note that we set the value after tail to be nullptr.
> 4. **~={blue}Removing arbitrary location=~**: Traverse to index of deletion:
> 	1. The next of the prev = currents next
> 	2. The prev of the next = current prev
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void erase(size_t index) {
>	// (1) Remove Head
>	if (index == 0) {
>		Node* toDelete = this->head;
>		this->head = this->head->next;
>		if (this->head != nullptr) {
>			// If the linked list is not entirely empty
>			head->prev = nullptr;
>		} else {
>			// If the linked list is entirely empty
>			tail = nullptr;
>		}
>		delete toDelete;
>		
>	// (2) Remove Tail (If index != 0, there is more than 1 element in
>	// the linked list)
>	} else if (index = this->size - 1) {
>		Node* toDelete = this->tail;
>		this->tail = this->tail->prev;
>		this->tail->next = nullptr;
>		delete toDelete;
>		
>	// (3) Delete at Arbitrary Location
>	} else {
>		Node* current = head;
>		// At the location of what we want to delete
>		for (size_t i{0}; i < index; ++i) {
>			current = current->next;
>		}
>		Node* toDelete = current;
>		current->prev->next = current->next;
>		current->next->prev = current->prev;
>		delete toDelete;
>	}
>	this->size--;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We may have to traverse the entire list, hence $O(n)$
>

> [!note]- Get `(N/A)`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Retrieve element at index $i$:
> 1. Traverse linked list until index from head
> 2. Return value
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>int get(size_t index) const {
>	Node* current = this->head;
>	for (size_t i{0}; i < index; ++i) {
>		current = current->next;
>	}
>	return current->data;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(1)$=~**
> 
> We may have to traverse entire list, hence $O(n)$

> [!note]- Set `(N/A)`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Retrieve element at index $i$:
> 1. Traverse linked list until index from head
> 2. Return value
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void get(size_t index, int value) const {
>	Node* current = this->head;
>	for (size_t i{0}; i < index; ++i) {
>		current = current->next;
>	}
>	current->data = value;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(1)$=~**
> 
> We may have to traverse entire list, hence $O(n)$

#flashcards/dsa/ds/list