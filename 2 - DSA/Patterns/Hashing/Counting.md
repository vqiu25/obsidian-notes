> [!QUOTE] Quick Notes
> Whenever we need to track the frequency of things.
> * Prefix Sums with Unordered Maps (Exact Condition)
> * Using Unordered Maps to Count (Other)

# Overview
## Recipe

>[!Note]- Counting Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Prefix Sums=~**: Whenever we have to determine the sum or product of subarrays efficiently.
>2. **~={purple}Prefix Sum Strategies=~**:
>* **~={blue}Subarray Sums=~**:
>	* **~={red}When to Use=~**: Need to find the sum of subarrays.
>	* **~={red}What to Note=~**:
>		* **~={pink}Define prefix[i]=~**: Define to contain everything before or, instead inclusive.
>		* **~={pink}Subquery Logic=~**: How to query the prefix sum. Usually is a range between index `i` and `j` from the original array.

# Points of Interest

>[!Note]- Iterate through a hash map
> <!-- Multiline -->
>To iterate through a hash map, we can:
>```cpp
>unordered_map<int, int​> map;
>for (const auto& pair : map) {
>	// Key
>	int key = pair.first
>	// Value
>	int value = pair.second
>}
>```

>[!Note]- What can be stored as keys and values in a hash map?
> <!-- Multiline -->
> **~={purple}Keys=~**:
> * Hash map keys must be immutable. (Strings are mutable in C++)
>
> **~={purple}Values=~**:
> * Values do not need to be immutable.

# Templates

>[!Info]- Count the number of subarrays with an "exact" constraint
><!-- Multiline -->
>**~={red}Question=~**
>* Find the number of subarrays that sum to $k$
>
><u>**Explanation**</u>
>1. ~={purple}**Build Prefix Sum**=~: For each subsequent, `ith` element, we will sum the previous value in the prefix sum, `prefix[i]`, with the `ith` value, `nums[i]`.
>2. **~={purple}Build Count Map=~**: Create a map where in every iteration, we will add the `ith` value in the prefix sum to the map. We shall increment if it already exists.
>3. **~={purple}Locate Target=~**: To identify whether the exact constraint $k$ is met (in this case, subarray sum equals k), we note that a subarray shall exist, if the current index of the prefix sum - a prior value in the prefix sum = k. If this is met, 
>	1. Because we can have duplicates in our prefix sum, the count at which prior values of the prefix sum have appeared is stored.
>
><u>**C++ Code**</u>
>```cpp
>int fn(vector<int​> nums, int k) {
>	// (1) Build prefix sum with 0 included as the first value
>	int n = nums.size();
>	vector<int​> prefix(n + 1);
>	for (int i{0}; i < nums.size(); ++i) {
>		prefix[i + 1] = prefix[i] + nums[i]
>	}
>	// (2) Build count map
>	unordered_map<int, int​> count;
>	int result{0};
>	for (int i{0}; i < n + 1; ++i) {
>		// (3) Locate target
>		int target = prefix[right] - k;
>		if (count.find(target) != count.end()) {
>			// (3.1) Add the number of times the previous prefix sum
>			// has appeared to our result
>			result += count[target];
>		}
>		// (2) After every iteration, add the ith prefix sum to the map
>		count[prefix[i]]++;
>	}
>	return result;
>}
>```
><u>**Visual Explanation**</u>
> ![[Drawing 2024-12-22 23.06.16.excalidraw | center | 700]]

# Example Problems
## Exact Condition

> [!Question]- Count Number of Nice Subarrays
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.
> * Return _the number of **nice** sub-arrays_.
>
>**~={red}Solution=~**:
>
>This question is similar to the template, except lefts perform the following augmentation on the input first. If we iterate through our input array, and change:
>* **~={green}Even Numbers=~**: to 0
>* **~={red}Odd Numbers=~**: to 1
>
>A subarray that sums to k, will now be equivalent to having k odd numbers in it.

> [!Question]- Contiguous Array (NOT DONE)
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a binary array `nums`, return _the maximum length of a contiguous subarray with an equal number of_ `0` _and_ `1`.
>
>**~={red}Solution=~**:
>
>This question is similar to the template, except lefts perform the following augmentation on the input first. If we iterate through our input array, and change:
>* **~={red}Zeros=~**: to -1
>* **~={green}Ones=~**: Keep it as 1
>
>A subarray that sums to 0 will therefore have an equal number of zeros and ones.

## Other

> [!Question]- Maximum Number of Balloons
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a string `text`, you want to use the characters of `text` to form as many instances of the word **"balloon"** as possible.
> * You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.
>
>**~={red}Solution=~**:
>
>1. **~={purple}Count Frequency=~**: Count the number of times each character appears in the input string.
>2. **~={purple}Count "Balloon"=~**: Count the number of times each character in balloon appears.
>* If i.e. 'b' doesn't exist, in C++, it will create an entry, and set the default to 0, returning 0.
>* We need to floor half (to not overcount) the count of `l` and `o` as two of each is required to spell balloon.
>* Return the min count as that will specify the max number of times we can spell balloon
>
>```cpp
>int maxNumberOfBalloons(string text) {
>	unordered_map<char, int> count;
>	// (1) Count Frequency
>	for (char c : text) {
>		count[c]++;
>	}
>	// (2) Count Balloon
>	int bCount = count['b'];
>	int aCount = count['a'];
>	int lCount = count['l'] / 2;
>	int oCount = count['o'] / 2;
>	int nCount = count['n'];
>	
>	int minCount = min({bCount, aCount, lCount, oCount, nCount});
>	return minCount;
>```

> [!Question]- Group Anagrams
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a string `text`, you want to use the characters of `text` to form as many instances of the word **"balloon"** as possible.
> * You can use each character in `text` **at most once**. Return the maximum number of instances that can be formed.
>
>**~={red}Solution=~**:
>
>1. **~={purple}Count Frequency=~**: Count the number of times each character appears in the input string.
>2. **~={purple}Count "Balloon"=~**: Count the number of times each character in balloon appears.
>* If i.e. 'b' doesn't exist, in C++, it will create an entry, and set the default to 0, returning 0.
>* We need to floor half (to not overcount) the count of `l` and `o` as two of each is required to spell balloon.
>* Return the min count as that will specify the max number of times we can spell balloon
>
>```cpp
>int maxNumberOfBalloons(string text) {
>	unordered_map<char, int> count;
>	// (1) Count Frequency
>	for (char c : text) {
>		count[c]++;
>	}
>	// (2) Count Balloon
>	int bCount = count['b'];
>	int aCount = count['a'];
>	int lCount = count['l'] / 2;
>	int oCount = count['o'] / 2;
>	int nCount = count['n'];
>	
>	int minCount = min({bCount, aCount, lCount, oCount, nCount});
>	return minCount;
>```


#flashcards/dsa/patterns/counting
