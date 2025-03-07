> [!QUOTE] Quick Notes
> Determine the sum of subarrays efficiently

# Overview
## Recipe

>[!Note]- Prefix Sums Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Prefix Sums=~**: Whenever we have to determine the sum or product of subarrays efficiently.
>2. **~={purple}Prefix Sum Strategies=~**:
>* **~={blue}Subarray Sums=~**:
>	* **~={red}When to Use=~**: Need to find the sum of subarrays.
>	* **~={red}What to Note=~**:
>		* **~={pink}Define prefix[i]=~**: Define to contain everything before or, instead inclusive.
>		* **~={pink}Subquery Logic=~**: How to query the prefix sum. Usually is a range between index `i` and `j` from the original array.

## Time Complexity

>[!Note]- Time Complexity of Prefix Sums
> <!-- Multiline -->
> * **~={purple}Building Prefix Sum=~**: We will iterate the entire array once, hence $O(n)$
> * **~={purple}Accessing Prefix Sum=~**: Accessing an array, hence $O(1)$
> ````col
>```col-md
> ![[Drawing 2024-12-16 08.24.54.excalidraw | 300 | center]]
>```
>```col-md
We go through our **~={blue}input array=~** exactly once, thus for a size $n=4$ array, we will incur $O(4) = O(n)$ operations
>
>```
>````

## Points of Interest

>[!Note]- What does `prefix[i]` represent?
> <!-- Multiline -->
>The sum of all elements up to index `i` (inclusive)
>
> ![[Drawing 2024-12-16 08.22.39.excalidraw | center | 550]]

>[!Note]- Sum between range index `i` and `j` (Does not include 0)
> <!-- Multiline -->
>To find the sum of the subarray in-between index $i$ and $j$ (inclusive), we perform:
>* `prefix[j] - prefix[i - 1]`
>
>To illustrate this, for example, $i=1$, $j=2$. Where we will:
>1. Add the sum of everything up to and including $j=2$
>2. Remove everything before (not including) $i=1$
>
> ![[Drawing 2024-12-18 08.14.05.excalidraw | center | 450]]

>[!Note]- Define `prefix[0] = 0` ?
> <!-- Multiline -->
>Sometimes, it may make more sense for `prefix[i]` to represent the sum of everything before index `i`. This is when we want a way to:
>* Find the sum between two indices, and
>* Be able to find the sum of the entire prefix sum using 2 indices (i.e. index 0 and the last index)
>
>Hence the first value of the prefix sum would be 0. To build such a prefix sum, we can:
>```cpp
>int n = nums.size();
>vector<int​> prefix(n + 1);
>for (int i{0}; i < n; ++i) {
>	prefix[i + 1] = prefix[i] + nums[i];
>}
>```

>[!Note]- Sum between range index `i` and `j` (Includes 0)
> <!-- Multiline -->
>To find the sum of the subarray in-between index $i$ and $j$ of the original array (inclusive), we perform:
>* `prefix[j + 1] - prefix[i]`
> ![[Drawing 2025-02-09 16.53.00.excalidraw | center | 450]]

>[!Note]- How to query a prefix sum / split the prefix sum at every point in the original array?
> <!-- Multiline -->
>* If we built a prefix sum of length $n$
>* We construct a prefix sum of length $n + 1$, as it includes 0 at index 0
>* If we want to compute every possible split:
>```cpp
>for (int i{1}; i < prefix.size(); ++i) {
>	int leftHalfSum = prefix[i] - prefix[0];
>	int rightHalfSum = prefix[prefix.size() - 1] - leftHalf;
>}
>```
> ![[Drawing 2025-01-26 16.54.56.excalidraw | center | 400]]

>[!Note]- How to query everything in a prefix sum
> <!-- Multiline -->
>This will make every possible query to the prefix sum
>```cpp
>for (int i{0}; i < prefix.size(); ++i) {
>	for (int j{0}; j < i; ++j) {
>		int comp = prefix[i] - prefix[j];
>	}
>}
>```

>[!Note]- Range Minimum Query (Sparse Table Technique)
> <!-- Multiline -->
>**~={purple}Information=~**
>* Allows us to efficiently find the smallest, largest, GDC, LCM of a subarray efficiently
>* **~={green}Preprocessing Complexity=~**: $O(nlog(n))$
>* **~={green}Query Complexity=~**: $O(1)$
>
>**~={purple}Construct Sparse Table=~**
>
>* **~={blue}Sparse Table Format=~**: The $ith$ row represents subarrays of lengths $2^i$
>* **~={green}Base Case=~**: The first row (`st[0][j]`) is initialised directly from the input array, as we are dealing with subarrays of length $2^0$
>* **~={red}Recursive Relation=~**:
>1. **~={purple}Specify Subarray Range=~**: This is decided by row $i$
>2. **~={purple}Compute Min of Each Subarray Starting at $j$=~**: We start at index $j$
>* Compute the result for two overlapping ranges from the row above
>* Min value from **~={blue}first=~** subrange: `st[i - 1][j]`
>* Min value from **~={blue}second=~** subrange: `st[i - 1][j + 2^(i - 1)]`
>* Result = `min(st[i - 1][j], st[i - 1][j + 1 << (i - 1)])`
>
> ![[Drawing 2025-01-26 12.29.56.excalidraw | center | 700]]
>
>```cpp
>int smallestSubarray(vector<int​> nums) {
>	int n = nums.size();
>	int log = log2(n) + 1;
>	
>	// Log rows, n columns
>	vector<vector<int​>> st(log, vector<int​>(n));
>
>	// (2) First row is the same as nums array
>	for (int i{0}; i < n; ++i) st[0][i] = nums[i];
>	
>	// (3) Build sparse table
>	for (int i{1}; i < log; ++i) {
>		for (int j{0}; j + (1 << i) < n; ++j) {
>			st[i][j] = min(st[i - 1][j], st[i - 1][j + (1 << (i - 1))]);
>		}
>	}
>}
>```
>
>**~={purple}Query Sparse Table=~**
>
>* **~={blue}Key Idea=~**: 
>	* For a given range `[L, R]`, find the largest power of 2 such that $2^k<=(R - L + 1)$
>	* Break the range [L, R] into two overlapping subranges of size $2^k$
>		* Min value from **~={blue}first=~** subrange: st[k][L]
>		* Min value from **~={blue}second=~** subrange: st[k][R - 2^k + 1]
>		* Result = min of the 2 above
>
> ![[Drawing 2025-01-26 21.13.23.excalidraw | center | 300]]
>
>```cpp
>int query(const vector<vector<int​>>& st, int L, int R) {
>	// Compute the largest power of 2 within the range
>	int k = log2(R - L + 1);
>	return min(st[k][L], st[k][R - (1 << k) + 1]);
>}
>```

# Templates

>[!Info]- Building Subarray Sums
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Set 0th Element**=~: By definition, the 0th element, will be the 0th element in the provided `array`.
>2. ~={purple}**Build Prefix Sum**=~: For each subsequent, `ith` element, we will sum the previous value in the prefix sum, `prefix[i-1]`, with the `ith` value, `array[i]`.
>
><u>**C++ Code**</u>
>```cpp
>int fn(arr) {
>	vector<int​> prefix(arr.size());
>	// (1) Set 0th element in prefix sum
>	prefix[0] = arr[0];
>	// (2) Sum the previous value in prefix sum and current value in
>	// array
>	for (int i{1}; i < arr.size(); ++i) {
>		prefix[i] = prefix[i - 1] + nums[i]
>	}
>}
>```
><u>**Visual Explanation**</u>
>````col
>```col-md
>**First Iteration**
>
> ![[Drawing 2024-12-16 08.09.39.excalidraw | 300 | center]]
>```
>```col-md
>**Completed Prefix Sum**
>
> ![[Drawing 2024-12-16 08.12.50.excalidraw | 300 | center]]
>```
>````

>[!Info]- Building 2D Prefix Sum
><!-- Multiline -->
><u>**Explanation**</u>
>1. **~={purple}Definition=~**: `prefix[i + 1][j + 1]`, represents the summation of the rectangle from nums[0][0] to nums[i][j] (inclusive)
>2. ~={purple}**Build Prefix Sum**=~:
>	1. **~={purple}Start at index [1][1]=~**: This is because prefix[1][1] corresponds to nums[0][0]
>	2. **~={purple}Add the left and top values of the prefix=~**: We're adding 2 rectangles effectively
>	3. **~={purple}Subtract top diagonal left value=~**: The 2 rectangles we add have overlap, thus we remove that overlap here
>	4. **~={purple}Add the top diagonal left value from nums=~**: This is the current number we're on, so we add it
> ![[Drawing 2025-02-09 21.36.16.excalidraw | center | 700]]
>
> **~={green}Visual Explanation of Overlap=~**
> 
> ![[Drawing 2025-02-09 22.36.45.excalidraw | center | 650]]
> 
><u>**C++ Code**</u>
>```cpp
>int fn(int n) {
>	vector<vector<int​>> prefix(n + 1, vector<int​>(n + 1, 0));
>		for (int i{1}; i < n + 1; ++i) {
>			for (int j{1}; j < n + 1; ++j) {
>				prefix[i][j] = prefix[i - 1][j] + prefix[i][j - 1] 
>						- prefix[i - 1][j - 1] + forest[i - 1][j - 1];
>			}
>		}
>}
>```
>**~={green}Querying=~**
>
>![[Drawing 2025-02-09 22.48.55.excalidraw | center | 650]]
>
>```cpp
>int fn(int y2, x2, y1, x1 (index 1 based)) {
>	int numTrees = prefix[y2][x2] - prefix[y1 - 1][x2] 
>				   - prefix[y2][x1 - 1] + prefix[y1 - 1][x1 - 1];
>}
>```
>
> 
y

# Example Problems

> [!Question]- Number of Ways to Split Array
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an integer array `nums`, find the number of ways to split the array into two parts so that the first section has a sum greater than or equal to the sum of the second section. The second section should have at least one number.
>
>**~={red}Solution=~**:
>1. **~={purple}Build Prefix Sum=~**: Trivial.
>2. **~={purple}Subarray Queries=~**: Iterate through the prefix sum. The size of the left section is `preifx[i]`, and the size of the right section is `prefix[prefix.size() - 1] - prefix[i]` (i.e. the last value in the prefix sum, subtract the left section).
>
> ![[Drawing 2024-12-18 21.59.35.excalidraw | center | 450]]
>```cpp
>int waysToSplitArray(vector<int​>& nums) {
>	// (1) Build Prefix Sum
>	vector<int​> prefix(nums.size());
>	prefix[0] = nums[0];
>	for (int i {1}; i < nums.size(); ++i) {
>		prefix[i] = prefix[i - 1] + nums[i];
>	}
>	// (2) Subarray Queries
>	int result{0};
>	for (int i{0}; i < nums.size() - 1; ++i) {
>		int leftSize = prefix[i];
>		int rightSize = prefix[nums.size() - 1] - leftSize;
>		if (leftSize >= rightSize) {
>			result++;
>		}
>		return result;
>	}
>```

> [!Question]- Minimum Value to Get Positive Step by Step Sum
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an array of integers nums, you start with an initial positive value startValue.
> * In each iteration, you calculate the step by step sum of startValue plus elements in nums (from left to right).
> * Return the minimum positive value of startValue such that the step by step sum is never less than 1.
>
>**~={red}Solution=~**:
>1. **~={purple}Build Prefix Sum=~**: Trivial.
>2. **~={purple}Subarray Queries=~**: Iterate through the prefix sum, and find the smallest value. 
>* If the smallest value is <= 0, our `startValue` will have to be $1-smallest value$. (i.e. If we have $-5$ in our prefix sum, if we had an initial "boost" of $1-(-5)=6$, this would make $-5$ go up to $1$ as required.)
>* If the smallest value in the prefix sum is 1 or greater, then return 1.
>
> ![[Drawing 2024-12-18 22.21.01.excalidraw | 550 | center]]
>```cpp
>int minStartValue(vector<int​>& nums) {
>	// (1) Build Prefix Sum
>	vector<int​> prefix(nums.size());
>	prefix[0] = nums[0];
>	for (int i {1}; i < nums.size(); ++i) {
>		prefix[i] = prefix[i - 1] + nums[i];
>	}
>	// (2) Subarray Queries
>	int minValue = *min_element(prefix.begin(), prefix.end());
>	if (minValue >= 1) {
>		return 1;
>	} else {
>		return (1 - minValue);
>	}
>```

> [!Question]- K Radius Subarray Averages
> <!-- Multiline -->
> **~={red}Question=~**:
> * You are given a 0-indexed array nums of n integers, and an integer k.
> * The k-radius average for a subarray of nums centered at some index i with the radius k is the average of all elements in nums between the indices i - k and i + k (inclusive). If there are less than k elements before or after the index i, then the k-radius average is -1.
> * Build and return an array avgs of length n where avgs[i] is the k-radius average for the subarray centered at index i.
> 
> ![[Pasted image 20241220080331.png | center | 300]]
>
>**~={red}Answer=~**:
>1. **~={purple}Build Prefix Sum=~**: Note we build a prefix sum to include 0. This is because we wish to be able to 
>2. **~={purple}Subarray Queries=~**:
>* **~={red}If out of bounds=~**: Just skip this iteration. This occurs when `i < k` or `i > (n - k - 1)`
>
> ![[Drawing 2024-12-20 08.16.45.excalidraw | center | 500]]
>
>* **~={green}If in bounds=~**: Find the sum of the window. This can be found using `prefix[i + k + 1] - prefix[i - k]`
>
> ![[Drawing 2024-12-20 08.27.14.excalidraw | 300 | center]]
>
>```cpp
>vector<int​> getAverages<int​>& nums, int k) {
>	int n = nums.size();
>	// (1) Build Prefix Sum
>	vector<int​> prefix(n + 1);
>	for (int i{0}; i < n; ++i) {
>		prefix[i + 1] = prefix[i] + nums[i];
>	}
>	// (2) Subarray Queries
>	vector<int​> result;
>	for (int i{0}; i < n; ++i) {
>		// (3) If out of bounds
>		if (i < k || i > (n - k - 1)) {
>			continue;
>		}
>		// (3) If in bounds
>		double total = prefix[i + k + 1] - prefix[i - k];
>		int average = total / (2 * k + 1);
>		result.push_back(average);
>	}
>	return result;
>}
>```

> [!Question]- Maximum Side Length of Square with Sum Constraint (2D Prefix Sum + Maximal Binary Search)
> <!-- Multiline -->
> **~={red}Question=~**:
> - Given a `mat` (2D matrix) and an integer `threshold`, find the **largest side length** of a square such that **the sum of elements inside it does not exceed** `threshold`.
>
> ~={green}**Solution Overview**=~:
> 1. **~={purple}Precompute 2D Prefix Sum=~**:
>    - Construct a **prefix sum matrix** to allow $O(1)$ sum retrievals for any submatrix.
> 2. **~={purple}Binary Search on Side Length=~**:
>    - Use **binary search** to determine the largest valid square size.
>    - The `isValid()` function checks if a square of a given size **exists** within the threshold.
>
> ~={green}**Key Observations**=~:
> - **~={purple}2D Prefix Sum Query=~**:
>   - To find the sum of any square from `(x1, y1)` to `(x2, y2)`, use:
>     ```
>     sum = prefix[y2][x2] - prefix[y1-1][x2] - prefix[y2][x1-1] + prefix[y1-1][x1-1]
>     ```
>   - This allows us to efficiently query sums in **constant time**.
> - ~={purple}**Binary Search on Side Length**=~:
>   - Instead of checking **every** square size, use **binary search** to quickly find the **maximum possible** side length.
>
> ~={red}**Code Implementation**=~:
> ```cpp
> class Solution {
> public:
>     int maxSideLength(vector<vector<int​>>& mat, int threshold) {
>         // (1) Compute 2D Prefix Sum
>         int rows = mat.size();
>         int cols = mat[0].size();
> 
>         vector<vector<int​>> prefix(rows + 1, vector<int​>(cols + 1, 0));
> 
>         for (int i{1}; i < rows + 1; ++i) {
>             for (int j{1}; j < cols + 1; ++j) {
>                 prefix[i][j] = prefix[i - 1][j] + prefix[i][j - 1] -
>                                prefix[i - 1][j - 1] + mat[i - 1][j - 1];
>             }
>         }
> 
>         // (2) Binary Search on Side Length
>         int left{1};
>         int right = min(rows, cols);
> 
>         while (left <= right) {
>             int mid = left + (right - left) / 2;
> 
>             if (isValid(mid, threshold, prefix)) {
>                 left = mid + 1; // Try a larger square
>             } else {
>                 right = mid - 1; // Reduce the square size
>             }
>         }
> 
>         return right;
>     }
> 
>     bool isValid(int mid, int threshold, vector<vector<int​>>& prefix) {
>         int rows = prefix.size();
>         int cols = prefix[0].size();
> 
>         // 1 Based Indexing in Prefix Sum
>         for (int i{1}; i <= rows - mid; ++i) {
>             for (int j{1}; j <= cols - mid; ++j) {
>                 int x1 = j;
>                 int y1 = i;
>                 int x2 = x1 + mid - 1;
>                 int y2 = y1 + mid - 1;
> 
>                 int sum = prefix[y2][x2] - prefix[y1 - 1][x2] - 
>                           prefix[y2][x1 - 1] + prefix[y1 - 1][x1 - 1];
> 
>                 if (sum <= threshold) return true;
>             }
>         }
> 
>         return false;
>     }
> };
> ```
>
> ~={blue}**Complexity Analysis**=~:
> - ~={green}**Time Complexity**=~:
>   - **Prefix Sum Construction**: $O(mn)$
>   - **Binary Search**: $O(\log \min(m, n))$
>   - **Validation Check per Side Length**: $O(mn)$ (in worst case)
>   - **Total Complexity**: $O(mn \log \min(m, n))$
>
> - ~={green}**Space Complexity**=~:
>   - $O(mn)$ for storing the 2D prefix sum.
>

# CP Problems

> [!Question]- Longest Subarray with Sum Divisible by 7
> <!-- Multiline -->
> ~={red}Question=~:
> * Given an integer array `nums`, find the **longest contiguous subarray** whose sum is **divisible by 7**.
>
> ~={red}Solution (Prefix Sum with Modulo & Hash Map)=~:
> 1. **~={purple}Compute Prefix Sum=~**:
>    - Construct a **prefix sum array**, where `prefix[i]` represents the sum of elements from `nums[0]` to `nums[i]`.
>    - Compute `prefix[i] % 7` at every index.
> 2. **~={purple}Track First Occurrence of Each Modulo=~**:
>    - Use a **hash map (`earliestIndex`)** to store the **first index** where a given remainder appears.
>    - If a remainder **reappears**, a subarray exists between the **earliest index** and the **current index** with a sum that is divisible by 7.
> 3. **~={purple}Update the Longest Valid Subarray=~**:
>    - If a remainder has been seen before, update `result` with the length of the subarray.
>
> ![[Drawing 2025-02-09 15.17.06.excalidraw | center | 500]]
> 
> ~={green}Example Walkthrough=~:
> ```
> nums = [3, 5, 1, 6, 2]
> prefix = [3, 8, 9, 15, 17]
> mod 7  = [3, 1, 2, 1,  3]   // Modulo 7
>
> // 1 appears at index 1 first → when it appears again at index 3, subarray (1,3) is valid
> // 3 appears at index 0 first → when it appears again at index 4, subarray (0,4) is valid
> ```
>
> ~={green}Code (Using Prefix Sum & Hash Map)=~:
> ```cpp
>
> using namespace std;
>
> int main() {
>     int n;
>     cin >> n;
>     vector<long long​> nums(n);
>     
>     for (int i{0}; i < n; ++i) {
>         cin >> nums[i];
>     }
>
>     vector<long long​> prefix(n + 1, 0);
>     prefix[0] = nums[0];
>
>     // (1) Compute Prefix Sum
>     for (int i{1}; i < n; ++i) {
>         prefix[i] = prefix[i - 1] + nums[i - 1];
>     }
>
>     // (2) Compute Modulo 7 for Each Prefix Sum
>     for (int i{0}; i < n; ++i) {
>         prefix[i] %= 7;
>     }
>
>     unordered_map<int, int> earliestIndex;
>     int result{0};
>
>     // (3) Track First Occurrence & Compute Maximum Subarray Length
>     for (int i{0}; i < prefix.size(); ++i) {
>         if (earliestIndex.find(prefix[i]) == earliestIndex.end()) {
>             earliestIndex[prefix[i]] = i;  // Store first occurrence
>         } else {
>             result = max(result, i - earliestIndex[prefix[i]]);
>         }
>     }
>
>     cout << result;
> }
> ```
>
> ~={green}Key Points=~:
> * **~={blue}Time Complexity=~**:
>   - **$O(n)$** (prefix sum + hash map lookups).
> * **~={blue}Space Complexity=~**:
>   - **$O(7) = O(1)$** (since modulo 7 results in at most 7 unique keys in `earliestIndex`).
> * ~={blue}**Why Modulo Works**=~:
>   - If `prefix[i] % 7 == prefix[j] % 7`, then `prefix[j] - prefix[i]` is **divisible by 7**, forming a valid subarray. Refer to Alg C++ Number Theory for Proof

> [!Question]- Stack Augmentation and Range Updates
> 
> ~={red}Question=~:
> 
> - You are given `numStacks` stacks and `numInstructions` range update operations.
> - Each operation `(start, end)` increments all stacks in the given range `[start, end]`.
> - After processing all instructions, return the **median** number of items across all stacks.
> 
> ~={red}Solution=~:
> 
> 1. **~={purple}Augmented Array for Range Updates=~**:
>     - Instead of iterating over all stacks for each range operation (which is inefficient), use **difference array technique**.
>     - Increment `augmented[start]` by `1` (starting the increase).
>     - Decrement `augmented[end + 1]` by `1` (ending the increase).
> 2. **~={purple}Convert Difference Array to Prefix Sum=~**:
>     - Compute the **prefix sum** of `augmented[]` to get the final values of each stack.
> 3. ~={purple}**Find the Median**=~:
>     - Sort the `prefix[]` array and find the median index `(numStacks - 1) / 2`.
>     - This gives the median stack height.
> 
> ![[Drawing 2025-02-09 18.40.21.excalidraw | center | 500]]
> 
> ~={green}C++ Code=~:
> 
> ```cpp
> int main() {
>     ios::sync_with_stdio(false);
>     cin.tie(nullptr);
>     
>     int numStacks, numInstructions;
>     cin >> numStacks >> numInstructions;
>     
>     vector<int​> augmented(numStacks, 0);
> 
>     // (1) Apply Range Updates
>     for (int i{0}; i < numInstructions; ++i) {
>         int start, end;
>         cin >> start >> end;
> 
>         augmented[start]++;
>         end++;
>         augmented[min(end, numStacks - 1)]--;
>     }
> 
>     // (2) Compute Prefix Sum
>     vector<int​> prefix(numStacks, 0);
>     prefix[0] = augmented[0];
>     for (int i{0}; i < numStacks; ++i) {
>         prefix[i + 1] = prefix[i] + augmented[i + 1];
>     }
> 
>     // (3) Find Median Stack Height
>     sort(prefix.begin(), prefix.end());
>     int median = (numStacks - 1) / 2;
>     int result = prefix[median];
> 
>     cout << result;
> }
> ```

#flashcards/dsa/patterns/prefixsums
