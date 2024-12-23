> [!quote]+ Overview
>````col 
>```col-md 
> ![[hashtable.gif]]
>``` 
>```col-md 
>* A hash table stores key value pairs (unordered map) or just values (unordered set)
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
> 	// Note that int is a signed integer, -2^32 to 2^32 -1
> 	int h{0};
> 	int length = s.size();
> 	for (int i{0}; i < length; ++i) {
> 		// Overflow wraps automatically, but to be explicit:
> 		// Bit shift of 2^5=32 = 2^32
> 		h = (31 * h + s[i]) % (1 << 32);
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
> 
> <!-- Multiline -->
> ````col
>```col-md
>**~={green}Good=~**
>Primes such as 31 are good. They help distribute hash values more uniformly and reduces systematic collisions. They combine bit‑shifting and addition/subtraction in a way that better preserves—or at least _mixes_—the high bits. This reduces the chance of an entire leading character’s contribution vanishing from the final hash.
>```
>```col-md
>**~={red}Bad=~**
>Powers of 2 are generally bad. This is because they cause large shifts that can wipe out high‐order bits once you $2^{32}$, leading to collisions where the first character “falls off.”
>
>**~={red}Note that powers of 2 are just bit shifts=~** So every time you multiply by the chosen value (i.e. 32 instead of 31), it may be lossy if enough bits have been used.
>```
>````

> [!info]- What is load factor?
> <!-- Multiline -->
> * The more elements you have in a table, the higher the load factor, the more likely you'll have a collision.
> * If table is size = m, we add m + 1 elements, there will be a collision (pigeon hole principle)
> $$Load \space Factor = \frac{Number \space of \space Elements \space in \space Table}{Size \space of \space Table}$$
>

# Preventing Collisions
## Chaining

> [!info]- What is chaining?
> <!-- Multiline -->
> **~={purple}Idea=~**: For keys that have the same hash value, at each position in the hash table, we store a Linked List or Dynamic Array of keys that have the same has value.
> 
> **~={purple}Complexity=~**: $O(n)$, where $n$ = size of hash table (all elements in 1 bucket)

## Open Addressing

### Linear Probing

> [!info]- What is linear probing?
> <!-- Multiline -->
> **~={purple}Idea=~**: If we add 2 elements with the same hash value, we go down the hash table until there is an empty space, and we place the value there instead.
> 
> **~={purple}Terminology=~**:
> * **~={green}"Always Empty"=~**: Initially all buckets in the table are marked as this. This goes away when something is added into this bucket.
> * **~={red}"Been Deleted"=~**: When a bucket had a value, and it was removed, it is marked this.
>
>**~={purple}Complexity=~**: $O(n)$, where $n$ is the size of the table. $O(1)$ if low load factor.
>
> ![[Drawing 2024-12-22 11.51.30.excalidraw | center | 700]]

### Quadratic Probing

> [!info]- What is quadratic probing?
> <!-- Multiline -->
> **~={purple}Idea=~**: If an element collides at its hash position, instead of moving **linearly** (i.e., +1, +2, …) to find an open slot, we move according to the squares of the step count. In other words, from the hash position $h$ we check:
>
>$$h+1^2,\space h+2^2,\space h+3^2,...$$
>
> **~={purple}Terminology=~**:
> * **~={green}"Always Empty"=~**: Initially all buckets in the table are marked as this. This goes away when something is added into this bucket.
> * **~={red}"Been Deleted"=~**: When a bucket had a value, and it was removed, it is marked this.
>
>**~={purple}Complexity=~**: $O(n)$, where $n$ is the size of the table. $O(1)$ if low load factor.

> [!info]- Chaining vs Linear vs Quadratic Probing
> <!-- Multiline -->
> * **~={purple}Chaining=~**:
> 	* **~={green}Pros=~**: Easy to handle collisions. Doesn't suffer from clustering as much, as clustering is localised to buckets with many collisions, but it doesn't affect unrelated buckets.
> 	* **~={red}Cons=~**: More memory heavy.
> * **~={purple}Linear=~**:
> 	* **~={green}Pros=~**: More cache friendly as it accesses memory sequentially. This means adjacent slots are likely to be loaded into the CPU cache together.
> 	* **~={red}Cons=~**: Suffers from clustering. Searches sequentially for the next slot, causing collisions to form contiguous clusters that grow, impacting unrelated insertions.
> * **~={purple}Quadratic=~**:
> 	* **~={green}Pros=~**: Reduces some clustering by jumping around.
> 	* **~={red}Cons=~**: More computationally intensive compared to Linear. Not as cache friendly. Keys with the same value still cause clustering.

# Hash Table Complexity
## Chaining

> [!info]- Hash Table Parameters
> <!-- Multiline -->
> **~={purple}<u>Parameters</u>=~**
> ````col
>```col-md
> ```cpp
> class HashTable {
> private:
> 	struct Node {
> 		int key;
> 		int value;
> 		Node(int k, int v) :
> 			key{k},
> 			value{v} {}
> 	}
> 	size_t cap;
> 	size_t size;
> 	vector<list<Node​>​> table;
> 	const double loadFac{0.75};
> }
>```
>```col-md
>* ~={purple}**cap**=~: Total capacity of the hash table
>* **~={purple}size=~**: How many elements exist in the has table
>* **~={purple}table=~**: A vector of linked lists. This is the hash table.
>* **~={purple}loadFac=~**: What the load factor needs to be for the table to resize
>```
>````
>**~={purple}<u>Methods</u>=~**
>
>**~={blue}Resize Method=~**
>1. Update capacity to be 2x the current capacity
>2. Create a brand new vector of linked lists
>3. Copy all exisiting values over
>```cpp
>private:
>	int hashFunction(int key) const {
>		return key % (this->cap);
>	}
>	
>	void resize() {
>		int newCap = 2 * this->cap;
>		vector<list<Node​>​> newTable(newCap);
>		
> 		// Within every linked list, iterate through its nodes
> 		for (const auto& bucket : table) {
> 			for (const auto& node : bucket) {
> 				// Apply hash function with updated capacity for
> 				// new index position
> 				int newIndex = node.key % newCap;
> 				// Add to end of linked list
> 				newTable[newIndex].push_back(Node(node.key, node.value));
> 			}
> 		}
> 		// Copy the new table over (using move is more efficient)
> 		this->table = move(newTable);
> 		this->cap = newCap;
> 	}
>	
>```
>
> **~={green}<u>Constructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
> HashTable(int initialCap) :
> 	cap{initialCap},
> 	size{0} {
> 	this->table.resize(initialCap);
> }
>```
>```col-md
>* **~={purple}initialCap=~**: User passes in the initial capacity of the hash table
>1. Set the initial size to be 0
>2. Resize the hash table to the initial capacity (well double)
>```
>````
> **~={red}<u>Destructor</u>=~**
> 
>Not necessary here as the `vector` and `list` already handles it.

> [!note]- Add `(h.insert(key, value))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Inserting element in a hash table:
> 1. If the key already exists, return (we can't store duplicates)
> 2. Find the index of where to insert the key (hash function)
> 3. Check load factor and resize the table if exceeded
>
> ![[Drawing 2024-12-22 14.53.20.excalidraw | center | 350]]
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void insert(int key, int value) {
>	// (1) If the key already exists
>	if (contains(key)) return;
>	
>	// (2) Find which bucket to place key
>	int index = this->hashFunction(key);
>	this->table[index].push_back(Node(key, value);
>	this->size++;
>	// (3) If we have breached our load factor, resize the table
>	if ((float) this->size / this->cap > this->loadFac) {
>		this->resize();
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> We have to copy over the entire hash table.
>
> ~={purple}<u>**Average Complexity**</u>: $O(1)$=~
> 
>Happens infrequently, which averages out to constant time.
>

> [!note]- Remove `(h.erase(key))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Remove the key value pair from the hash map. 
> 1. Find the index position/bucket the key value pair object is stored
> 2. Create a brand new bucket to replace the exisiting bucket
> 3. Iterate through the bucket, and if the key is not found, add it to the new bucket
> 4. Copy over the new bucket to existing table
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void remove(int key) {
>	// (1) Find index position/bucket where the key value pair object
>	// is stored
>	int index = this->hashFunction(key);
>	// (2) Create a new bucket to replace the existing bucket
>	list<Node​> newBucket;
>	// (3) Iterate through the linked list add to the new bucket
>	for (const Node& node : table[index]) {
>		if (node.key != key) {
>			newBucket.push_back(node);
>		}
>	}
>	// (4) Copy over the new bucket to existing table
>	this->table[index] = move(newBucket);
>	this->size--;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> If all the elements are in the same bucket, we may have to iterate through every element.
>
> ~={purple}<u>**Average Complexity**</u>: $O(1)$=~
> 
>Generally elements are well spread out.
>

> [!note]- Get `(h.a(key) / h[key])`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Retrieve value at key.
> 1. Find the index position/bucket the key value pair object is stored
> 2. Iterate through the bucket, and return what is found
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>int get(int key) const {
>	// (1) Find the bucket where the key is stored
>	int index = this->hashFunction(key);
>	// (2) Iterate through every element in the bucket
>	for (const Node& node : this->table[index]) {
>		if (node.key == key) {
>			return node.value;
>		}
>	}
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> If all the elements are in the same bucket, we may have to iterate through every element.
>
> ~={purple}<u>**Average Complexity**</u>: $O(1)$
> 
>Generally elements are well spread out.

> [!note]- Contains `(h.find(key))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Retrieve value at key.
> 1. Find the index position/bucket the key value pair object is stored
> 2. Iterate through the bucket, and return true if found
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>bool find(int key) const {
>	// (1) Find the bucket where the key is stored
>	int index = this->hashFunction(key);
>	// (2) Iterate through every element in the bucket
>	for (const Node& node : this->table[index]) {
>		if (node.key == key) {
>			return true;
>		}
>	}
>	return false
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(n)$=~**
> 
> If all the elements are in the same bucket, we may have to iterate through every element.
>
> ~={purple}<u>**Average Complexity**</u>: $O(1)$=~
> 
>Generally elements are well spread out.

#flashcards/dsa/ds/hashtable