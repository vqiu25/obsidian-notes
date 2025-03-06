> [!quote]+ Overview
> <!-- Multiline -->
>````col 
>```col-md 
> ![[fenwicktree.gif]]
>``` 
>```col-md 
>Fenwick tree is effectively a prefix sum that supports point updates. 
>``` 
>```` 
>

# Fenwick Tree Complexity

> [!info]- Fenwick Tree Parameters
> <!-- Multiline -->
> **~={purple}<u>Parameters</u>=~**
> ````col
>```col-md
> ```cpp
> class FenwickTree {
> private:
> 	vector<int​> tree;
> 	int size;
> }
>```
>```col-md
>* **~={purple}tree=~**: Holds partial sums to quickly compute prefix sums.
>* **~={purple}size=~**: The size of the input array (using 1-based indexing internally).
>```
>````
> **~={green}<u>Naive Constructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
> 	FenwickTree(int size) 
> 		: size{size} {
> 	tree.resize(size + 1, 0); 
> }
>```
>```col-md
>* Initialises the Fenwick Tree for `n` elements.
>* Uses 1-based indexing for simplicity in bit manipulation.
>```
>````
> **~={green}<u>Array Based Constructor</u>=~**
>* Each node in the Fenwick Tree adds the values of its direct children to its own, effectively propagating the cumulative sums upward through the tree structure.
>
> ```cpp
> public:
> 	FenwickTree(vector<int​>& nums) { 
> 		size = nums.size();
> 		tree.resize(size + 1, 0);
> 		// Copy array values using 1-based indexing
> 		for (int i{1}; i <= size; ++i) {
> 			tree[i] = nums[i - 1];
> 		}
> 		// Propagate sums to build the tree in O(n)
> 		for (int i{1}; i <= size; ++i) {
> 			int j = i + (i & -i);
> 			if (j <= size) tree[j] += tree[i];
> 		}
> }
>```
>

> [!note]- Prefix Sum Query `(f.query(i))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> * Computes the cumulative sum (inclusive) from index $1$ to index $i$ in the original array. We can use this function to compute range sums by subtracting 2 prefix queries. Thus a range query from $l$ to $r$ would be `query(r) - query(l - 1)`
> * The way we jump from index to index is to look at the:
> 	1. Binary number of the current index
> 	2. Find what position the LSB is from the right (1 indexed)
> 	3. Current Index += 2 ^ (LSB Position - 1)
> 
> ![[Drawing 2025-02-25 16.42.33.excalidraw | center | 450]]
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>int query(int i) {
>	int sum{0};
>	
>	if (; i >= 1; i -= i & -i) {
>		sum += tree[i];
>	}
>	return sum;
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(log(n))$=~**
> 
> Bitwise traversal is $O(log(n))$.
>

> [!note]- Point Update `(f.update(i, delta))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Increments the value at index `i` and it's parents above, in the original array by `delta`, which updates the tree accordingly.
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>void update(int i, int delta) {
>	for (; i <= size; i += i & -i) {
>		tree[i] += delta;
>	}
>}
>```
> **~={purple}<u>Complexity</u>: $O(log(n))$=~**
> 
> Bitwise traversal is $O(log(n))$.

#flashcards/dsa/ds/fenwick-tree
