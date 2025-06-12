> [!QUOTE] Quick Notes
> Linear data structure and predictable dynamics to reduce $O(n^2)$ to $O(n)$

# Overview
## Recipe

>[!Note]- Two Pointers Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Two Pointers**=~: 
>* **~={green}Linear DS=~**: Usually requires a linear data structure (array, linked list)
>* **~={green}Predictable Dynamics=~**: The input is symmetrical (palindrome), sorted, etc
>2. **~={purple}Two Pointer Strategies=~**:
>* **~={blue}Inward Traversal=~**:
>	* **~={red}When to Use=~**: When we want to compare values at both ends of the data structure.
>	* **~={red}What to note=~**:
>		* **~={pink}Initialisation=~**: Place left and right pointers at both ends of the data structure.
>		* **~={pink}While Loop Condition=~**: `(left < right)` if you don't want to compare the values when `left = right`. Otherwise, `(left <= right)`.
>		* **~={pink}Update Pointer Logic=~**: What conditions should you set to update pointer positions?
>
>* **~={blue}Unidirectional Traversal=~**:

## Time Complexity

>[!Note]- Time Complexity of Two Pointers
> <!-- Multiline -->
> It is common to have to compare every unique pair in an array. This can be done by:
> ```cpp
> for (int i{0}; i < array.size(); ++i) {
> 	for (int j = i + 1; j < array.size(); ++j) {
> 		...
> 	}
> }
>```
> The time complexity of checking every unique pair is $O(n^2)$, because for a $n$ length array:
> ````col
>```col-md
> ![[Drawing 2024-12-14 14.24.27.excalidraw | 250 | center]]
>```
>```col-md
>There are:
>* **~={red}3 Comparisons=~**
>* **~={green}2 Comparisons=~**
>* **~={blue}1 Comparison=~**
>
>Where the sum from $1$ to $n-1$ is $\frac{(n-1)((n-1)+1)}{2}$ making it $O(n^2)$
>```
>````
>Where in a 2 pointers approach, the summation of the movement of both pointers generally will not exceed $O(n)$

## Points of Interest

>[!Note]- Inward Traversal, $\leq$ vs $<$ for While Loop Condition
> <!-- Multiline -->
>For inward traversal, we generally use $<$. This is because we do not need to compare the values at the left and right pointer, when they are pointing at the same index position. This stands true for questions like **~={red}Two Sum=~**, for both when the array is:
>````col 
>```col-md
>**~={purple}Even=~**: 	
> ![[Drawing 2024-12-14 15.28.06.excalidraw | 250 | center]]
>``` 
>```col-md
>~={purple}**Odd**=~: 
> ![[Drawing 2024-12-14 15.32.31.excalidraw | 290 | center]]
>``` 
>```` 
>

>[!Note]- Inward Traversal, does it Miss Comparisons?
> <!-- Multiline -->
>Given the following situation in the **~={red}Two Sum=~** problem:
>
> ![[Drawing 2024-12-14 22.02.48.excalidraw | 600 | center]]

>[!Note]- Unidirectional Traversal, how to traverse 2 arrays?
> <!-- Multiline -->
>Given the following situation in the **~={red}Two Sum=~** problem:
>
> ![[Drawing 2024-12-14 22.02.48.excalidraw | 600 | center]]

>[!Note]- Floyd Cycle Detection
> <!-- Multiline -->
> * https://www.youtube.com/watch?v=wjYnzkAhcNk&t=336s

# Templates

>[!Info]- Inward Traversal
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Initialise Left and Right**=~: Set the left pointer to the 0th element, and right pointer as the last.
>2. ~={purple}While the pointers are not equal =~: The moment both pointers are on the same index, exit out of the while loop.
>3. ~={purple}**Updating Pointers**=~: At each iteration, move the pointers close to each other. Deciding which pointers to move will depend on the problem.
>
><u>**C++ Code**</u>
>```cpp
>int fn(arr) {
>	// (1) Set the left pointer to the 0th element, and right pointer
>	// as the last
>	int left{0};
>	int right{arr.size() - 1};
>	// (2) While the pointers are not equal
>	while (left < right) {
>		// (3) Perform some logic, where we:
>		// 1. left++;
>		// 2. right--;
>		// 3. Both
>	}
>}
>```
><u>**Visual Explanation**</u>
>````col
>```col-md
>**Initial Pointer Location**
>
> ![[Drawing 2024-12-14 15.03.46.excalidraw]]
>```
>```col-md
>**Terminating Condition**
>
> ![[Drawing 2024-12-14 15.04.41.excalidraw]]
>```
>````

>[!Info]- Unidirectional Traversal using 2 Arrays
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Initialise Left and Right**=~: Set the left pointers to both start at the 0th element in their respective array.
>2. ~={purple}While loop until One Pointer reaches the End=~: For each iteration, we must increment either one or both of the pointers. Deciding which pointers to move will depend on the problem.
>3. ~={purple}**Exhaust Pointers**=~: If one pointer has not reached the end of the array, force it to
>
><u>**C++ Code**</u>
>```cpp
>int fn(arr1, arr2) {
>	// (1) Set the left pointer to the 0th element, and right pointer
>	// as the last
>	int leftOne{0};
>	int leftTwo{0};
>	// (2) While the left pointer has not exceeded the right
>	while (leftOne < arr1.size() && leftTwo < arr2.size()) {
>		// 1. leftOne++;
>		// 2. leftTwo++;
>		// 3. Both
>	}
>	// (3) Exhaust Pointers
>	while (leftOne < arr1.size()) {
>		// Do some logic
>		leftOne++
>	}
>	while (leftTwo < arr2.size()) {
>		// Do some logic
>		leftTwo++
>	}
>}
>```
><u>**Visual Explanation**</u>
>````col
>```col-md
>**Initial Pointer Location**
>
> ![[Drawing 2024-12-14 18.11.10.excalidraw]]
>```
>```col-md
>**Terminating Condition**
>* When one of the $left$ pointers reaches to the end of the array. 
>* Following, exhaust the remaining pointer.
>```
>````

# Example Problems
## Inward Traversal

> [!Question]- Two Sum
> <!-- Multiline -->
> **~={red}Question=~**
> * Given a **sorted** array of unique integers and a target integer, return `true` if there exists a pair of numbers that sum to target, `false` otherwise.
> * For example, given `nums = [1, 2, 4, 6, 8, 9, 14, 15]` and `target = 13`, return true because `4 + 9 = 13`.
>
>**~={red}Solution=~**
>1. **~={purple}Compute Sum=~**: In every iteration, we compute the two sum.
>2. **~={purple}Pointer Shifting Logic=~**:
>* If `sum == target`: If we have found the target, we can return `true`
>* If `sum < target`: This means, we need to try a larger number in the array. The `left` pointer will always point to a value less than or equal to the value at the right pointer, because the array is sorted. Incrementing the `left` pointer will in a greater or equal sum.
> ![[Drawing 2024-12-14 21.34.08.excalidraw | 400 | center]]
>* If `sum > target`: By similar logic, decrementing the `right` pointer would result in a sum thats smaller or equal.
>3. **~={purple}Complexity=~**: $O(n)$ as the summation of the pointer movements is $n$
> ```cpp
> bool twoSum(vector<int​>& nums, int target) {
> 	int left{0};
> 	int right{nums.size() - 1};
>
>	 while (left < right) {
>		 // (1) Compute Sum
>		 int sum = nums[left] + nums[right];
>		 // (2) Pointer Logic
>		 if (sum == target) {
>			 return true;
>		 } else if (sum < target) {
>			 left++;
>		 } else if (sum > target) {
>			 right--;
>		 }
>	 }
>	 return false
> }
>```
> 

> [!Question]- Squares of Sorted Array
> <!-- Multiline -->
> **~={red}Question=~**
> * Given an integer array `nums` sorted in **non-decreasing** order, return _an array of **the squares of each number** sorted in non-decreasing order_.
>
>**~={red}Solution=~**
>1. **~={purple}Insight=~**: When squared, the largest values will appear at both the very left and right ends.
>
> ![[Drawing 2024-12-15 23.22.53.excalidraw | 250 | center]]
> 2. **~={purple}Setup While Loop=~**: Here, we want, `while (left <= right)`. This is because when we compare the square at the left pointer and the right pointer, only one of them will be added to our `result` array. Thus, to ensure every value get's added:
>
> ![[Drawing 2024-12-15 23.31.15.excalidraw | center | 400]]
>
>3. **~={purple}Compute Square=~**: Compute the square at both the `left` and right` pointer. 
>4. **~={purple}Pointer Shifting Logic=~**:
>* If `leftSquare < rightSquare`: If the value at the `right` pointer is larger, shift the `right` pointer down
>* Else: Shift the `left` pointer up
>
>```cpp
> vector<int​> sortedSquares(vector<int​>& nums) {
>     int left = 0;
>     int right = nums.size() - 1;
>     vector<int​> result(nums.size());
>     int decrement = right;
>
>     // (2) Setup while loop logic
>     while (left <= right) {
>         // (3) Compute Square
>         int leftSquared = nums[left] * nums[left];
>         int rightSquared = nums[right] * nums[right];
>
>         // (4) Pointer Logic
>         if (leftSquared < rightSquared) {
>             result[decrement] = rightSquared;
>             right--;
>         } else {
>             result[decrement] = leftSquared;
>             left++;
>         }
>         decrement--;
>     }
>     return result;
> }
>```

> [!Question]- Reverse Words in a String III (For -> While)
> <!-- Multiline -->
> **~={red}Question=~**
> 
> Given a string `s`, reverse the string according to the following rules:
> * All the characters that are not English letters remain in the same position.
> * All the English letters (lowercase or uppercase) should be reversed.
> 
> Return `s` _after reversing it_.
>
>**~={red}Solution=~**
>
>1. **~={purple}Pointer Shifting Logic=~**: Iterate through the input array. Establish $left$ to be at index 0. When we locate a space character, record where it is, and treat it as $right$. Now using a while loop between $left$ and $right$ to swap this segment around. 
>
>```cpp
> string reverseWords(string s) {
>     int front{0};
>     for (int i{0}; i <= s.size(); ++i) {
> 	    if (s[i] == ' ' || i == s.size()) {
> 		    // Set left to be where "front" is
> 		    int left{front};
> 		    // Set right to be 1 before the space character
> 		    int right{i - 1};
> 		    
> 			while (left < right) {
> 				swap(s[left], s[right]);
> 				left++;
> 				right--;
> 			}
> 			// Set the new front to be 1 after the space character
> 			front = i + 1;
> 	    }
> 	}
> }
>```

## Unidirectional Traversal

> [!Question]- Shortest Word Distance II
> <!-- Multiline -->
> **~={red}Question=~:**
> - You are given a list of words called `wordsDict`, and you will be given multiple queries of the form (`word1`, `word2`).
> - Return the shortest distance between `word1` and `word2` in `wordsDict`.
> - Assume `word1` and `word2` are always present in `wordsDict`.
> 
> **~={red}Solution=~:**
> 1. **~={blue}Precompute Indices:=~** 
>    - Use an `unordered_map<string, vector<int>>` (`valToIndex`) to store all indices of each word.
>    - This allows `O(1)` lookup time for any word in subsequent queries.
> 2. ~={blue}**Two Pointers Approach=~:**
>    - Use **two pointers** (`left` and `right`) to iterate over the **sorted index lists** of `word1` and `word2`.
>    - **~={green}Compare & Update Minimum Distance=~:**  
>      - Compute `abs(indexOne[left] - indexTwo[right])`, updating `minDistance`.
>    - ~={green}**Move the Smaller Index Pointer=~:**  
>      - If `indexOne[left] > indexTwo[right]`, increment `right`, else increment `left`.
> 
> **~={green}Code=~:**
> ```cpp
> class WordDistance {
> private:
>     unordered_map<string, vector<int​>> valToIndex;
> public:
>     WordDistance(vector<string​>& wordsDict) {
>         for (int i{0}; i < wordsDict.size(); ++i) {
>             valToIndex[wordsDict[i]].push_back(i);
>         }
>     }
>     
>     int shortest(string word1, string word2) {
>         vector<int​> indexOne = valToIndex[word1];
>         vector<int​> indexTwo = valToIndex[word2];
>         int left{0};
>         int right{0};
>         int minDistance{INT_MAX};
>         while (left < indexOne.size() && right < indexTwo.size()) {
>             minDistance = min(minDistance, abs(indexOne[left] - indexTwo[right]));
>             if (indexOne[left] > indexTwo[right]) {
>                 right++;
>             } else {
>                 left++;
>             }
>         }
>         return minDistance;
>     }
> };
> ```
> 
> ~={green}**Key Points**=~:
> - ~={red}**Time Complexity=~:**
>   - **Preprocessing (`WordDistance` constructor)**: `O(n)`, where `n` is the size of `wordsDict`.
>   - **Query (`shortest` method)**: `O(m + k)`, where `m` and `k` are the number of occurrences of `word1` and `word2`.
> - ~={red}**Space Complexity=~:** `O(n)`, as we store all word indices.
> - **~={red}Why Two Pointers?=~**
>   - Both index lists are **sorted**, allowing for an efficient linear scan (`O(m + k)`) rather than `O(m * k)` brute force.
>   - We also don't miss anything as we are trying to find the smallest difference between a value in these arrays as we are constantly trying to close the gap (we never increase it)

> [!Question]- Remove Duplicates from Sorted Array II - Slow/Fast Pointers
> <!-- Multiline -->
> ![[Drawing 2025-02-24 08.27.19.excalidraw | center | 500]]

## Floyd Cycle Detection

> [!Question]- Shortest Word Distance II
> <!-- Multiline -->
> **~={red}Question=~:**
> - Find the duplicate number in the array
> 
> **~={red}Solution=~:**
> ![[Drawing 2025-05-27 11.31.11.excalidraw | center | 700]]
> 

#flashcards/dsa/patterns/twopointers