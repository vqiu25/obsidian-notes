> [!QUOTE] Quick Notes
> Searching a sorted array

# Overview
## Recipe

>[!Note]- Binary Search Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Binary Search**=~: When we need to search a sorted array.

## Time Complexity

>[!Note]- Time Complexity of Binary Search
> <!-- Multiline -->
> With Binary Search, what we're effectively asking is, how many times can we divide our search space by 2?
> ````col
>```col-md
> ![[Drawing 2025-01-04 16.10.40.excalidraw | center]]
>```
>```col-md
>For an array of size $n$, there will be $\log_{2}(n)$ levels, which means at most $\log_{2}(n)$ operations, giving $O(\log_{2}(n))$
>```
>````

## Points of Interest

>[!Note]- Avoid Overflow
> <!-- Multiline -->
>````col
>```col-md
>**~={red}No Overflow=~**
>$$ left + (right - left) / 2$$
>Cannot overflow
>```
>```col-md
>**~={green}Overflow=~**
>$$ (left + right) / 2$$
>This is because $left + right$ could exceed the capacity of their type (i.e. `int`)
>```
>````

>[!Note]- Searching a Matrix
> <!-- Multiline -->
> How do we convert a 2d array, into a 1d array with appropriate mapping:
> * Lets denote $n$ as the **~={green}number of columns=~**
> * Here $n=3$
> ![[Drawing 2025-01-04 17.35.41.excalidraw | center | 350]]
>
>Where we observe:
>* **~={purple}Row 1=~**: Starts at Index 0
>* **~={purple}Row 2=~**: Starts at Index n
>* **~={purple}Row 3=~**: Starts at Index 2n
>
>Hence for any index $i$, $i=row_{num} \cdot n+col_{num}$
> 
>````col
>```col-md
>**~={blue}Rows=~**
>$$ row_{num} = i / n $$
>Dividing the 1D array index ($i$) by the number of columns ($n$) gives the row number because each complete row spans $n$ elements, and integer division discards the remainder, leaving the row count.
>```
>```col-md
>**~={blue}Columns=~**
>$$ col_{num} = i \% n$$
>Taking the remainder of the index ($i$) divided by $n$ gives the column number because the remainder represents the position within the current row after dividing by $n$.
>```
>````

>[!Note]- Insert Position
> <!-- Multiline -->
> If we had the following array, [-1, 2 ,3]
>````col
>```col-md
>**~={red}Insert at Beginning=~**
>* If we try look for `-3`
>* => Index 0
>```
>```col-md
>**~={green}Insert at End=~**
>* If we try look for `5`
>* => Index 3
>```
>````

# Templates

>[!Info]- Binary Search (Left Most Occurrence / Insertion Point)
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Define Search Space**=~: Set left and right pointers to include all possible elements
>* `left`: Index 0
>* `right`: One larger than the final index
>2. ~={purple}**Exit Condition**=~: `while(left < right)`, as the moment both pointers point on the same index, we conclude our search.
>3. ~={purple}**Narrow Search Space**=~: We first compute the middle, and find it's value:
>* **~={green}If >= Target=~**: `right  = mid`, as if the value at the middle were equal to the target, we would want to set our capacity (right) here
>* **~={red}If < Target=~**: `left = mid + 1`, as if our target is greater than the value at left, left and below are no longer in the search space, so we increment 1 to create a tighter search space.
>4. **~={purple}Return Value=~**: left is the minimal value which satisfies the condition in the if statement
>
><u>**Notable Properties**</u>
>* **~={green}First Occurence of Target=~**: Search for Target. If not found, we obtain the Insertion Point.
>* **~={green}Last Occurence of Target=~**: Search for (Target + 1). Obtain that index, and subtract 1.
>* **~={red}Insertion Point=~**: Point 1
>* **~={red}Check for Existence=~**: Search for target. If the index returned does not have that value, then that value does not exist. We check for existence at the end of the binary search, performing:
> 
>```cpp
>// If the array was only size 1, and our target is not in the array
>// nums[left] will cause an exception, as left will be out of bounds
>if (left < arr.size() && arr[left] == target) {
>	return true;
>} else {
>	return false;
>}
>```
>
><u>**C++ Code**</u>
>```cpp
>int binarySearch(vector<int​>& arr, int target) {
>	// (1) Define Search Space
>	int left = 0;
>	int right = arr.size();
>	
>	// (2) Exit Condition
>	while (left < right) {
>		int mid = left + floor((right - left) / 2);
>		
>		// We could also instead add a if (arr[mid] == target) {
>		// return mid }, then else if (arr[mid] >= target)...
>		
>		// (3) Narrow Search Space Based on a Condition
>		// If descending order array, then adjust to < target
>		if (arr[mid] >= target) {
>			right = mid;
>		} else {
>			left = mid + 1;
>		}
>	}
>	return left;
>}
>```
><u>**Visual Explanation**</u>
>````col
>```col-md
>**First Occurence of Target**
>
> ![[Drawing 2025-01-04 15.30.31.excalidraw | center]]
>```
>```col-md
>Insertion Point
>
> ![[Drawing 2025-01-04 15.47.38.excalidraw | center]]
>```
>````

>[!Info]- Binary Search (New Approach :O)
><!-- Multiline -->
><u>**Explanation**</u>
>* **~={purple}What Left and Right Mean=~**:
>	* **~={red}Left=~**: Last value of "before"
>	* **~={green}Right=~**: First value of "after"
>* **~={purple}Virtual Boundaries=~**: We define `left = -1` and `right = arr.size()`. This is so that the invariant can be maintained.
>* **~={purple}isBefore and isAfter=~**:
>	* **~={green}isBefore=~**: T T F F
>	* **~={red}isAfter=~**: F F T T
>		* If we establish our invariant to this, then we'll need to swap where `left = mid` with `right = mid`
>
><u>**C++ Code**</u>
>```cpp
>int binarySearch(vector<int​>& arr, int target) {
>	// (4) isBefore Lambda
>	isBefore = [&](int mid) -> bool {
>		// If in the before region, return true
>		// else return false
>	};
>	
>	// (1) Define Search Space
>	int left = -1;
>	int right = arr.size();
>	
>	// (2) Exit Condition
>	while (right - left > 1) {
>		int mid = left + (right - left) / 2;
>		
>		// (3) Narrow Search Space Based on a Condition
>		if (isBefore(mid)) {
>			left = mid;
>		} else {
>			right = mid;
>		}
>	}
>	return left or right;
>}
>```
><u>**Visual Explanation**</u>
>
>**~={green}In Range=~**
> ![[Drawing 2025-06-02 10.04.24.excalidraw | center | 500]]
> 
> **~={orange}Border=~**
> ![[Drawing 2025-06-02 10.31.38.excalidraw | center | 500]]
> 
> **~={red}Out of Range (If values may not exist)=~**
> ![[Drawing 2025-06-02 10.44.48.excalidraw | center | 500]]
> 
> **~={purple}On Solution Space=~**
> ![[Drawing 2025-06-06 14.42.37.excalidraw | center | 500]]
> 

# Example Problems

> [!question]- Successful Pairs of Spells and Potions
> <!-- Multiline -->
> **~={red}Question=~**
> * You are given two arrays:
>   - `spells[i]`: Strength of the $(i^{th})$ spell.
>   - `potions[j]`: Strength of the $(j^{th})$ potion.
> * A spell and potion are considered a successful pair if their product is at least `success`.
> * Return _an array where each element represents the number of potions that form a successful pair with the corresponding spell._
>
> **~={red}Solution=~**
> 1. **~={purple}Sorting Step=~**:
>    - Sort the `potions` array in **ascending order**. This allows efficient binary search to find the minimum potion strength required for success.
> 2. **~={purple}Binary Search for Valid Potions=~**:
>    - For each spell, calculate the minimum potion strength required: $\text{ceil}(\text{success} / \text{spell})$
>    - Perform binary search in `potions` to find the first potion meeting this requirement.
> 3. **~={purple}Count Valid Pairs=~**:
>    - The number of valid potions for a spell is the number of potions from the binary search index to the end of the `potions` array.
>
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> class Solution {
> public:
>     vector<int​> successfulPairs(vector<int​>& spells, vector<int​>& potions, long long success) {
>         // Sort potions for binary search
>         sort(potions.begin(), potions.end());
>         
>         // Result array to store the number of valid potions for each spell
>         vector<int​> result;
>
>         // Iterate through each spell
>         for (int i{0}; i < spells.size(); ++i) {
>             // Calculate the minimum required potion strength for success
>             int spellValue = spells[i];
>             long long requiredPotionStrength = ceil(success 
> 								            / (double)spellValue);
>
>             // Find the number of valid potions using binary search
>             int numberOfValidPotions = potions.size() 
>             - binarySearch(potions, requiredPotionStrength);
>             
>             // Store the result for the current spell
>             result.push_back(numberOfValidPotions);
>         }
>         return result;
>     }
>
>     // Binary search to find the first potion meeting the minimum required strength
>     int binarySearch(vector<int​>& arr, long long target) {
>         int left{0};
>         int right = arr.size();
>
>         // Perform binary search
>         while (left < right) {
>             int mid = left + (right - left) / 2;
>             
>             // Narrow down the range based on the target
>             if (arr[mid] >= target) {
>                 right = mid;
>             } else {
>                 left = mid + 1;
>             }
>         }
>         return left; // Return the index of the first valid potion
>     }
> };
> ```
>
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:
>   - Sorting: $O(m \log m)$, where \(m\) is the size of `potions`.
>   - Binary search for each spell: $O(n \log m)$, where \(n\) is the size of `spells`.
>   - Total: $O(m \log m + n \log m)$.
> - **~={purple}Space Complexity=~**:
>   - $O(1)$ (in-place sorting and calculations).

> [!Question]- Kth Missing Positive Number
> <!-- Multiline -->
> **~={red}Question=~:**
> - Given a sorted array `arr` of distinct positive integers and an integer `k`, return the kth positive integer that is missing from this array.
> 
> **~={red}Solution=~:**
> 1. ~={blue}**Key Insight=~:**
>    - The number of missing integers before `arr[i]` can be computed as `arr[i] - (i + 1)`.
>    - This represents how many positive integers are missing up to the `i`-th position.
> 
> 1. ~={blue}**Binary Search Approach=~:**
>    - **Search Space:** The range of indices from `0` to `arr.size()`.
>    - **Condition:** Find the **leftmost index** where the missing count is **greater than or equal to** `k`.
>    - **Process:**
>      - Compute `mid = left + (right - left) / 2`.
>      - If `arr[mid] - (mid + 1) >= k`, shrink the right boundary (`right = mid`).
>      - Otherwise, move the left boundary up (`left = mid + 1`).
> 
> 1. ~={blue}**Post-Search Analysis=~:**
>    - **Case 1:** If `left == arr.size()`, the kth missing number is **beyond the last element**.  
>      - Return: `arr.back() + (k - (arr.back() - arr.size()))`.
>    - **Case 2:** The kth missing number occurs **before** `arr[left]`.  
>      - Return: `left + k`.
> 
>  ![[Drawing 2025-02-20 15.18.41.excalidraw | center | 700]]
> 
> **~={green}Code=~:**
> ```cpp
> class Solution {
> public:
>     int findKthPositive(vector<int​>& arr, int k) {
>         int left{0};
>         int right = arr.size();
>
>         while (left < right) {
>             int mid = left + (right - left) / 2;
>             if (arr[mid] - (mid + 1) >= k) {
>                 right = mid;
>             } else {
>                 left = mid + 1;
>             }
>         }
>
>         if (left == arr.size()) {
>             return arr.back() + (k - (arr.back() - arr.size()));
>         } else {
>             return left + k;
>         }
>     }
> };
> ```
> 
> ~={green}**Key Points=~:**
> - **Time Complexity:** $O(\log n)$ due to binary search.
> - **Space Complexity:** $O(1)$ as no extra space is used.
> - **Why `arr[mid] - (mid + 1)`?**  
>   - It measures how many numbers are missing before `arr[mid]`. For a perfectly continuous sequence `[1, 2, 3, ...]`, `arr[mid]` should equal `mid + 1`. Any difference indicates missing values.
> 
> **Example Walkthrough:**
> - Input: `arr = [2, 3, 4, 7, 11]`, `k = 5`
> - Missing numbers sequence: `[1, 5, 6, 8, 9, 10, ...]`
> - The 5th missing number is `9`.


#flashcards/dsa/patterns/binarysearch/array
