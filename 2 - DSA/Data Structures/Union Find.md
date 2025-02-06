> [!quote]+ Overview
>````col 
>```col-md 
> ![[ufds 1.gif]]
>``` 
>```col-md 
>The **Union-Find** data structure supports efficient merging of sets and finding the representative (leader) of a set.
>  
**~={purple}Optimisations=~:**
> 1. ~={blue}**Path Compression**=~: Flattens the structure to improve future lookups.
> 2. ~={blue}**Union by Rank**=~: Merges smaller trees into larger trees to keep depth minimal.  
>``` 
>```` 

# Union Find Complexity

> [!info]- Union Find Parameters
> <!-- Multiline -->
> **~={purple}<u>Parameters</u>=~**
> ````col
>```col-md
> ```cpp
> class UnionFind {
> private:
> 	vector<int​> parent;
> 	vector<int​> rank;
> }
>```
>```col-md
>* **~={purple}parent=~**: Stores the parent of each element (or itself if it is the leader).
>* **~={purple}rank=~**: Height of disjoint set.
>```
>````
> **~={green}<u>Constructor</u>=~**
> ````col
>```col-md
> ```cpp
> public:
> UnionFind(int n) {
> 	parent.resize(n, 0);
>	rank.resize(n, 1);
> 	for (int i{0}; i < n; ++i) {
>		parent[i] = i;
>	 }
> }
>```
>```col-md
>* Each element starts as its **own parent** (a separate set).
>* Size is **initialised to 1** for all elements.
>```
>````

> [!note]- Find Representative `(uf.find(x))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Returns the **leader (representative)** of the set containing `x`. The recursive call will set the representative's parent (which is itself) as all the childrens parent, when path compression is used.
> 
> ![[Drawing 2025-02-06 12.31.59.excalidraw | center | 550]]
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>int find(int x) {
>	if (parent[x] != x) {
>		parent[x] = find(parent[x]);
>	}
>	return parent[x];
>}
>```
>
> **~={purple}<u>Complexity</u>: $O(log(n))$=~**
> 
> The complexity is the inverse Ackermann function. Without path compression (sometimes path compression may not happen), it is $O(log(n))$.
>
> ~={purple}<u>**Amortised Complexity**</u>: $O(1)^A$=~
> 
>Inverse Ackermann is approximately $O(1)$.
>

> [!note]- Same `(u.same(x, y))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Checks whether elements $x$ and $y$ belong to the same set.
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>bool same(int x, int y) {
>	return find(x) == find(y);
>}
>```
>

> [!note]- Unite `(u.unite(x, y))`
> <!-- Multiline -->
> **~={purple}<u>Description</u>=~**
> 
> Joins the sets that contain elements $x$ and $y$ (they have to be in different sets).
> 2. We first find the representatives of $x$ and $y$
> 3. Then connect the smaller set to the larger set (ideally, this is done by rank instead of size to achieve a height of $log(n)$)
> 4. When calling this function, make sure you check if the representative of $x$ and $y$ are different
>
>**~={purple}<u>Code</u>=~**
>
>```cpp
>bool unite(int x, int y) {
>	int repX = find(x);
>	int repY = find(y);
>	
>	if (repX == repY) return false;
>	
>	// Let us tree the X Bag as the largest
>	if (rank[repX] < rank[repY]) swap(repX, repY);
>	
>	// Merge repY into repX
>	parent[repY] = repX;
>	
>	// If we had 2 bags of the same size merging, this will
>	// cause the overall height to increase by 1
>	// Had one been smaller, the overall height would have been the 
>	// same
>	if (rank[repX] == rank[repY]) rank[repX]++;
>	return true;
>}
>```

#flashcards/dsa/ds/union-find

