> [!QUOTE] Quick Notes
> Determine the sum of subarrays efficiently

# Overview
## Recipe

>[!Note]- Fast and Slower Pointers Recipe
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

#flashcards/dsa/patterns/fastandslowpointers
