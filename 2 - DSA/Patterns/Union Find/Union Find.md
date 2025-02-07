# Example Problems

> [!Question]- Number of Unique Categories  
> <!-- Multiline -->  
> ~={red}**Question**=~:  
> * You are given `n` items, each belonging to a **category**.  
> * You have a function `haveSameCategory(a, b)`, which returns `true` if `a` and `b` share the same category.  
> * Find the **number of unique categories** in the dataset.  
>  
> ~={red}**Solution (Union-Find with Path Compression)**=~:  
> 1. ~={purple}**Use Union-Find (Disjoint Set)**=~:  
>    - Each item is initially in its own category.  
>    - If `haveSameCategory(i, j)` is `true`, **merge** their sets.  
> 2. ~={purple}**Path Compression**=~:  
>    - The `find(x)` operation updates **parent references** to ensure queries are fast.  
> 3. ~={purple}**Count Unique Categories**=~:  
>    - After merging, count the **number of distinct representatives** in the parent array.  
>  
> ~={green}**Code (Using Union-Find)**=~:  
> ```cpp  
> class Solution {  
> public:  
>     int numberOfCategories(int n, CategoryHandler* categoryHandler) {  
>         UnionFind uf{n};  
>  
>         // (1) Group items into categories  
>         for (int i{0}; i < n; ++i) {  
>             for (int j{i + 1}; j < n; ++j) {  
>                 if (categoryHandler->haveSameCategory(i, j)) {  
>                     uf.unite(i, j);  
>                 }  
>             }  
>         }  
>  
>         // (2) Count unique parent representatives  
>         unordered_set<int​> uniqueGroups;  
>         for (int i{0}; i < n; ++i) {  
>             uniqueGroups.insert(uf.parentIndexValue(i));  
>         }  
>  
>         return uniqueGroups.size();  
>     }  
> };  
> ```  
>  
> ~={green}**Key Points**=~:  
> * **Time Complexity**:  
>   - **$O(n^2 * Ackermann)$** (each union-find operation is nearly constant due to path compression).  
> * **Space Complexity**:  
>   - **$O(n)$** for parent and rank arrays.  
