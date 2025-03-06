> [!QUOTE] Quick Notes
> Finding and determining properties of subcomponents

# Overview
## Recipe

>[!Note]- Sliding Windows Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Sliding Windows**=~:
> * **~={green}Subcomponents Exist=~**: A window can be used to slide across the data structure unidirectionally, where subcomponents can be processed.
>2. **~={purple}Sliding Window Strategies=~**:
>* **~={blue}Dynamic=~**:
>	* **~={red}When to Use=~**: When asked to find the longest or shortest subcomponent that satisfies a condition.
>	* **~={red}What to Note=~**:
>		* **~={pink}Expanding Window Condition=~**: Always expand window by `++right`, though increment `curr` only if a condition is met.
>		* **~={pink}Window Invalid Logic=~**: After the window has been expanded, it may invalid. Use a `while` loop on when the condition is invalid.
>		* **~={pink}Shrinking Window Condition=~**: When the window is invalid, increment `left` to shrink the window, adjusting `curr` until the window is valid.
>		* **~={pink}Process Current Window=~**: Compute some kind of property, such as the number of valid subarrays for the current window.
>		
> ![[Drawing 2024-12-21 09.13.06.excalidraw | center | 500]]
>* **~={blue}Fixed=~**:
>	* **~={red}When to Use=~**: When asked to find a subcomponent of a certain length.
>	* **~={red}What to Note=~**: 
>		* **~={pink}Build Initial Window=~**: Sum the first $k$ elements of the input.
>		* **~={pink}Slide Window=~**: Remove `ans[left]`, and add `ans[right]`.
>		* **~={pink}Process Current Window=~**: Compute some kind of property, such as the average for the current window.

## Time Complexity

>[!Note]- Time Complexity of Sliding Windows
> <!-- Multiline -->
> The time complexity of checking every subarray (contiguous) is $O(n^2)$, because for a $n$ length array:
> ````col
>```col-md
>![[Drawing 2024-12-10 08.17.02.excalidraw | center | 300]]
>```
>```col-md
>There are:
>* ~={purple}Length 1=~: 4 / ($n$)
>* ~={green}Length 2=~: 3 / ($n-1$)
>* ~={red}Length 3=~: 2 / ($n-2$)
>* ~={orange}Length 4=~: 1 / ($n-3$)
>
>And the sum from $1$ to $n$ is $\frac{n(n+1)}{2}$ making it $O(n^2)$
>```
>````
>Where in Sliding Windows:
>* The left pointer, will only ever shift $n$ places
>* The right pointer will only ever shift $n$ places
>
>Thus the complexity will be $O(n) + O(n) = O(2n) = O(n)$

## Points of Interest

>[!Note]- Sliding windows general process
> <!-- Multiline -->
>Where ever the right pointer is, the left pointer can shift up to and including it. To illustrate this, for a problem like:
>* Finding the longest subarray whose sum is $<k$. Where $k=4$.
>
>You'll note that in every iteration, the window concludes at where the right pointer is:
> ![[Drawing 2024-12-16 22.53.59.excalidraw | 550 | center]]

>[!Note]- Do we miss any valid windows?
> <!-- Multiline -->
>The moment we shift the left pointer, there will be no more windows containing values before and including the previous value the left pointer was pointing at. This is by design as, for a problem like:
>* Finding the longest subarray whose sum is $<k$. Where $k=4$.
>
>Let's say we are at the following valid state, and moving to the next iteration where we will have to shift the right pointer:
> ![[Drawing 2024-12-17 08.06.06.excalidraw | 250 | center]]
> This will then lead to the invalid state:
>  ![[Drawing 2024-12-17 08.07.06.excalidraw | 250 | center]]
> Because we have an array of positive integers, as $1+2+3>4$ already, should the right pointer remain or move forwards, the state will remain invalid, and that will remain the case unless the **~={green}the left pointer shifts=~**. Any future windows that contain 1 will all be invalid (i.e. $[1,2,3,4]$), thus disregarding 1 will not cause us to miss valid windows.

>[!Note]- Can the left pointer pass the right pointer in dynamic window?
> <!-- Multiline -->
>No. The left pointer will only ever move up to where the right pointer is.

>[!Note]- Computing total number of subarrays in a sliding window
> <!-- Multiline -->
>If we have a window $(left, right)$, how many valid windows/subarrays exist that end at $right$.
>````col 
>```col-md 
> ![[Drawing 2024-12-10 17.26.29.excalidraw | center | 250]]
>``` 
>```col-md 
>For a particular window, starting at index $left$ and ending at index $right$, there are $right-left+1$ subarrays ending at $right$. 
>
>As an example, for this window, there are ~={red}3 valid subarrays=~, which can be shown via ~={green}3 - 1=~ + 1 = 3
>``` 
>```` 
>
>

>[!Note]- While the window is valid...
> <!-- Multiline -->
> * A question may benefit from having the while condition be, `WINDOW_IS_VALID` instead. This could be helpful when each incremental shrink also represents a valid window scenario that you need to record or process. 
> * If this is the case, and we need to count the number of valid subarrays, we would be inclined to calculate the number of valid subarrays starting from `right` (If a question asks for subarrays that are at least a particular sum). If a subarray at `left` is considered valid, then every subarray that includes values beyond `right` is also valid.
> 
> ![[Pasted image 20250225110709.png | center | 250]]
> ```cpp
>int fn(arr) {
>	int left{0};
>	int curr{0};
>	
>	for (int right{0}; right < arr.size(); ++arr) {
>		// (1) Do some logic to "add" elements at arr[right] to `curr`
>
>		while (WINDOW_IS_VALID) {
>			// (2) Process current window. This is put forward, as
>			// we need to record the valid window, before shrinking
>			// it.
>			...
>			// (3) Do some logic to "remove" elements at arr[left] 
>			// from `curr`
>			left++;
>		}
>	}
>	return curr;
>}
>```

>[!Note]- Common Window is Valid/Invalid Logic
> <!-- Multiline -->
> A question may benefit from having the while condition be, `WINDOW_IS_VALID` instead. This could be helpful when each incremental shrink also represents a valid window scenario that you need to record or process. In that case, we would:
> ```cpp
>int fn(arr) {
>	int left{0};
>	int curr{0};
>	
>	for (int right{0}; right < arr.size(); ++arr) {
>		// (1) Do some logic to "add" elements at arr[right] to `curr`
>
>		while (WINDOW_IS_VALID) {
>			// (2) Process current window. This is put forward, as
>			// we need to record the valid window, before shrinking
>			// it.
>			...
>			// (3) Do some logic to "remove" elements at arr[left] 
>			// from `curr`
>			left++;
>		}
>	}
>	return curr;
>}
>```

# Templates

>[!Info]- Dynamic Window Sizes Template
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Adding Elements to Window**=~: We will shift the right pointer once every iteration in our for loop.
>2. ~={purple}**Removing Elements from Window**=~: The moment we've shifted our **right** pointer, our window may fall into an ~={red}invalid=~ state. As such, we will move our left **pointer** up until we reach a ~={green}valid=~ state.
>3. ~={purple}**Process Current Window**=~: Update our result once we have reached a ~={green}valid=~ state.
>
><u>**C++ Code**</u>
>```cpp
>int fn(arr) {
>	int left{0};
>	int curr{0};
>	
>	for (int right{0}; right < arr.size(); ++arr) {
>		// (1) Do some logic to "add" elements at arr[right] to `curr`
>
>		while (WINDOW_IS_INVALID) {
>			// (2) Do some logic to "remove" elements at arr[left] 
>			// from `curr`
>			left++;
>		}
>		
>		// (3) Process current window
>	}
>	return curr;
>}
>```
><u>**Visual Explanation**</u>
>````col
>```col-md
>**Initial Window Size**
>
>![[Drawing 2024-12-10 08.06.54.excalidraw | center | 300]]
>```
>```col-md
>**Window Size after Iterations**
>
>![[Drawing 2024-12-09 22.21.06.excalidraw | center | 300]]
>```
>````

>[!Info]- Fixed Window Size Template
><!-- Multiline -->
>**<u>Explanation</u>**
>1. **~={purple}Initialisation=~**: $k$ is the size of our fixed window size
>2. **~={purple}Build Initial Window=~**: Sum the first $k$ elements
>3. **~={purple}Slide Window=~**: Shift the window by adding the $kth$ element and so on. Additionally, remove elements from index 0.
>4. **~={purple}Process Current Window=~**: Update answer based on new window
>
><u>C++ Code</u>
>```cpp
>// (1) Initialisation
>int fn(arr, k) {
>	// (2) Build Initial Window
>	int curr{0};
>	for (int i{0}; i < k; ++i) {
>		curr += arr[i];
>	}
>	// (3) Slide Window
>	int left{0};
>	int ans = curr;
>	for (int right{k}; right < arr.size(); ++right) {
>		curr += arr[right];
>		curr -= arr[left];
>		// (4) Process Current Window
>		ans = ...
>	}
>	return ans;
>}
>```
>
><u>Visual Explanation</u>
>* For a window of size $k=2$
>![[Drawing 2024-12-11 07.59.22.excalidraw | center | 300]]

# Example Problems

## Dynamic Window

> [!Question]- Max Consecutive Ones
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given a binary string `s` (a string containing only `"0"` and `"1"`). You may choose up to one `"0"` and flip it to a `"1"`. What is the length of the longest substring achievable that contains only `"1"`?
>* For example, given `s = "1101100111"`, the answer is `5`. If you perform the flip at index `2`, the string becomes `1111100111`.
>
>**~={red}Solution=~**:
>Let us keep track of the number of zeros we have in our current window (`curr`). Because we can flip `k=1` zero's, our string can at most contain `1` zero.
> ![[Drawing 2024-12-17 22.39.22.excalidraw | 300 | center]]
>
>* **~={purple}Expanding Window Condition=~**: Always expand, but if the added element is a zero, `curr++`.
>* **~={purple}Window Invalid Logic=~**: If `curr > 1`, our window is now invalid.
>* **~={purple}Shrinking Window Condition=~**: Always shrink, but if the element being removed is a zero, `cur--`.
>* ~={purple}**Process Current Window**=~: At the given $right$ index position, the number of subarrays between $left$ and $right$ are all valid strings.
>
>```cpp
>int findLength(string s) {
>	int left{0}, curr{0}, result{0};
>	for (int right{0}; right < s.size(); ++right) {
>		// (1) Expanding Window Condition
>		if (s[right] == '0') {
>			curr++;
>		}
>		// (2) Window Invalid Logic
>		while (curr > 1) {
>			// (3) Shrinking Window Condition
>			if (s[left] == '0') {
>				curr--;
>			}
>			left++;
>		}
>		// (4) Process Current Window
>		result = max(result, right - left + 1)
>	}
>	return result;
>}
>```

> [!Question]- Minimum Size Subarray Sum
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.
>
>**~={red}Solution=~**:
>
>* **~={purple}Expanding Window Condition=~**: Always expand, and add to the `currentSum`.
>* **~={purple}Valid Invalid Logic=~**: Because we are looking for smallest valid window. This occurs when $currentSum >= target$
>* ~={purple}**Process Current Window**=~: Compute the current window length, and compare with the current know minimum.
>* **~={purple}Shrinking Window Condition=~**: Always shrink, and subtract from `currentSum`.
>
>```cpp
>int minSubArrayLen(int target, vector<int​>& nums) {
>	int left{0}, currentSum{0}, minLength{INT_MAX};
>	for (int right{0}; right < s.size(); ++right) {
>		// (1) Expanding Window Condition
>		currentSum += nums[right];
>		// (2) Window Valid Logic
>		while (currentSum >= target) {
>			// (3) Process Current Window
>			minLength = min(minLength, left - right + 1)
>			// (4) Shrinking Window Condition
>			currentSum -= nums[left];
>			left++;
>		}
>	}
>	// (5) If minLength never changed, return 0, as there was never
>	// a valid window
>	return minLength == INT_MAX ? 0 : minLength;
>}
>```

> [!Question]- Maximum Number of Vowels in a Substring of Given Length
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a string s and an integer k, return the maximum number of vowel letters in any substring of s with length k.
>* Vowel letters in English are 'a', 'e', 'i', 'o', and 'u'.
>
> ![[Drawing 2024-12-21 08.53.35.excalidraw | 300 | center]]
>
>**~={red}Solution=~**:
>Let us keep track of the number of vowels we have in our current window (`curr`).
>
>* **~={purple}Expanding Window Condition=~**: Always expand, but if the added element is a vowel, `curr++`.
>* **~={purple}Window Invalid Logic=~**: If the current window exceeds k `left - right + 1 > k`
>* **~={purple}Shrinking Window Condition=~**: Always shrink when invalid, but if the element being removed is a vowel, `cur--`.
>* ~={purple}**Process Current Window**=~: For this iteration, once the window is valid (will contain the most number of vowels), compare against the current max number of vowels found in a prior window. 
>
>```cpp
>int maxVowels(string s, int k) {
>	int left{0}, curr{0}, maxNumVowels{0};
>	unordered_set<char​> vowels{'a', 'e', 'i', 'o', 'u'};
>	for (int right{0}; right < s.size(); ++right) {
>		// (1) Expanding Window Condition
>		if (vowels.find(s[right]) != s.end()) {
>			curr++;
>		}
>		// (2) Window Invalid Logic
>		while (left - right + 1 > k) {
>			// (3) Shrinking Window Condition
>			if (vowels.find(s[left] != s.end())) {
>				curr--;
>			}
>			left++;
>		}
>		// (4) Process Current Window
>		maxNumVowels = max(maxNumVowels, curr);
>	}
>	return result;
>}
>```

> [!Question]- Get Equal Substrings Within Budget
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given two strings `s` and `t` of the same length and an integer `maxCost`.
>* You want to change `s` to `t`. Changing the `ith` character of `s` to `ith` character of `t` costs `|s[i] - t[i]|` (i.e., the absolute difference between the ASCII values of the characters).
>* Return _the maximum length of a substring of_ `s` _that can be changed to be the same as the corresponding substring of_ `t` _with a cost less than or equal to_ `maxCost`. If there is no substring from `s` that can be changed to its corresponding substring from `t`, return `0`.
> 
> ![[Drawing 2024-12-21 15.10.01.excalidraw | 500 | center]]
>
>**~={red}Solution=~**:
>Let us keep track of the cost to convert the current window from `s` to `t` (`currCost`).
>
>* **~={purple}Expanding Window Condition=~**: Always expand, but if the added element is a different between `s` and `t`, `currCost += abs(s[i] - t[i])`. Note that both if conditions can be removed, as if they were the same, we would be adding 0.
>* **~={purple}Window Invalid Logic=~**: If the `currCost > maxCost`.
>* **~={purple}Shrinking Window Condition=~**: Always shrink, and reduce the cost of converting from `s` to `t` if they differ at this index.
>* ~={purple}**Process Current Window**=~: After a valid state has been reached, (where substring of `s` = substring of `t`), compare the length of the substring with the current largest.
>
>```cpp
>int equalSubstring(string s, string t, int maxCost) {
>	int left{0}, currCost{0}, maxLength{0};
>	for (int right{0}; right < s.size(); ++right) {
>		// (1) Expanding Window Condition
>		if (s[right] != t[right]) {
>			currCost += abs(s[right] - t[right]);
>		}
>		// (2) Window Invalid Logic
>		while (currCost > maxCost) {
>			// (3) Shrinking Window Condition
>			if (s[left] != t[left]) {
>				currCost -= abs(s[right] - t[right]);
>			}
>			left++;
>		}
>		// (4) Process Current Window
>		result = max(result, right - left + 1)
>	}
>	return result;
>}
>```

## Fixed Window

> [!Question]- Maximum Average Subarray
> <!-- Multiline -->
> **~={red}Question=~**:
> * Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return _this value_. Any answer with a calculation error less than $10^{-5}$ will be accepted.
>
> **~={red}Solution=~**:
> We will store the current largest average in a variable `curr`.
> 1. **~={purple}Build Initial Window=~**: Iterate through the first $k$ elements and sum them. Let `curr = windowSum`
> 2. **~={purple}Slide Window=~**: Remove the value at the $left$ pointer, and the add the value where the new $right$ pointer is pointing at. The $right$ pointer will start at the index position after the max index position in the initial window.
> 3. **~={purple}Process Current Window=~**: Compute the average in the current window, and store it in `curr` if it is the current largest.
> ```cpp
>double findMaxAverage(vector<int​>& nums, int k) {
>	int windowSum{0};
>	// (1) Build Initial Window
>	for (int i{0}; i < k; ++i) {
>		windowSum += nums[i];
>	}
>	int maxAverageValue{windowSize};
>	// (2) Slide Window
>	int left{0};
>	for (int right{k}; right < nums.size(); ++right) {
>		windowSum -= nums[left];
>		windowSum += nums[right];
>		left++;
>		// (3) Process Current Window
>		maxAverageValue = max(maxAverageValue, windowSum);
>	}
>	return maxAverageValue;
>}
>```

#flashcards/dsa/patterns/slidingwindows
