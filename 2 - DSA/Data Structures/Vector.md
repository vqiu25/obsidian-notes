> [!quote]+ Overview
>````col 
>```col-md 
> ![[vector.png]]
>``` 
>```col-md 
>We double the size of the array if we add over the current size. And we half the size of the array, if the amount of elements in the array fall below half the size of the current array.
>``` 
>```` 

# Vector Complexity

> [!info]- Vector Parameters
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
> ~={purple}<u>**Amortised Complexity**</u>: $O(1)^A$=~
> 
>On average, the cost of resizing gets distributed over multiple remove operations, resulting in an average time complexity of $O(1)$.

> [!note]- Get `(v[i])`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Retrieve element at index $i$:
> 1. Note that we use `int&` as our return type, meaning we are returning a reference rather than a copy of the value. This allows us modify a stored value in the array by `v[i] = x`
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

> [!note]- Size `(v.size())`
> <!-- Multiline -->
> **~={purple}<u>Code</u>=~**
> 
> ```cpp
> // We mark the method with const, so it cannot modify any non-mutable 
> // member variables (by default, everything is non-mutable)
> 
> size_t getSize() const {
> 	return this->size;
> }
>```
> **~={purple}<u>Complexity</u>: $O(1)$=~**
> 
> Typically, you would maintain a variable to keep track of the size, so accessing it is a constant time operation.

> [!info]- Add & Remove
> <!-- Multiline -->

#flashcards/dsa/ds/vector