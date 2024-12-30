# Array Implementation

> [!info]- Queue Parameters
> <!-- Multiline -->
> **~={purple}<u>Parameters</u>=~**
> ````col
>```col-md
> ```cpp
> class Queue {
> private:
> 	int* data;
> 	int front;
> 	int rear;
> 	int cap;
> 	int size;
> }
>```
>```col-md
>* **~={purple}data=~**: Pointer to the queue
>* **~={purple}front=~**: Indicates where the front of the queue is
>* **~={purple}rear=~**: Indicates where the end of the queue is
>* ~={purple}**cap**=~: Total capacity of the array (max)
>* **~={purple}size=~**: Amount of elements in the array
>```
>````
> **~={green}<u>Constructor</u>=~**
> * Note that you cannot use an initialiser list to initialise dynamically allocated memory
> ````col
>```col-md
> ```cpp
> public:
> Queue(int initialCap) :
> 	front{0};
> 	rear{-11};
> 	cap{initialCap},
> 	size{0} {
> 	this->data = new int[cap];
> }
>```
>```col-md
>* **~={purple}initialCap=~**: User passes in the initial capacity of the queue
>1. Set the front to be at index 0
>2. Given there are no elements yet, set rear to -1
>3. Set the initial size to be 0
>4. Initialise an array to be the of size `cap`
>```
>````
> **~={red}<u>Destructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
>~Queue() {
>	delete[] data;
>}
>```
>```col-md
>1. When you allocate memory using `new[]`, you must deallocate using `delete[]`. This frees up the memory where data was pointing.
>```
>````

> [!note]- Add to Back `(q.push(value))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Adding a value to the end of the queue:
> 1. Increment `rear`
> 2. Add the value to the data array, `data[rear] = val`
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void push(int val) {
>	if (this->size == this->capacity) {
>		throw std::overflow_error("Queue is full");
>	}
>	this->rear = (this->rear + 1) % this->capacity;
>	this->data[rear] = val;
>	++size;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We are incrementing one value, so $O(1)$.
>

> [!note]- Remove Front `(q.pop())`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> We are removing the front of the queue, by shifting where the "front" variable is.
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void pop_back() {
>	if (size == 0) {
>		throw std::underflow_error("Queue is empty");
>	}
>	this->front = (this->front + 1) % this->capacity;
>	--size;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(1)$=~**
> 
> We are incrementing one value, so $O(1)$

> [!info]- Circular Queue
> <!-- Multiline -->
> When we pushed elements, then popped them, those elements remain in our array, as we have only superficially "removed" them. Thus, by ensuring:
> * (front + 1) % capacity
> * (rear + 1) % capacity
>
> We will always have variable pointers that support "overlap"
> 
> ![[Drawing 2024-12-27 17.23.20.excalidraw | center | 700]]

#flashcards/dsa/ds/queue