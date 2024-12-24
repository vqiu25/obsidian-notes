> [!QUOTE] Quick Notes
> Whenever we need to track the frequency of things.
> * Prefix Sums with Unordered Maps (Exact Condition)
> * Using Unordered Maps to Count (Other)

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
>3. **~={purple}Locate Target=~**: To identify whether the exact constraint $k$ is met (in this case, subarray sum equals k), we note that a subarray shall exist, if the current index of the prefix sum - a prior value in the prefix sum = k.
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

> [!QUOTE] Quick Notes
> When we wish to find the number of sub arrays that fall under an exact condition.
> 1. Preprocess to get to desired input format
> 2. Build Prefix Sum
> 3. Build Count Map (For count) or Index Map (For length)
> 4. Specify a target and find it in the prefix sum (prior to the current index)

> [!Question]- Count Number of Nice Subarrays (Count)
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
>A subarray that sums to k, will now be equivalent to having k odd numbers in it. To identify whether the exact constraint is met (in this case, subarray sum equals k), we note that a subarray shall exist, if the current index of the prefix sum - a prior value in the prefix sum = k.

> [!Question]- Contiguous Array (Length)
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
>A subarray that sums to 0 will therefore have an equal number of zeros and ones. To identify whether the exact constraint $k$ is met (in this case, subarray sum equals 0), we note that a subarray shall exist, if the current index of the prefix sum - a prior value in the prefix sum = k.
>
>1. **~={blue}Preprocessing=~**: Convert all 0's to -1's
>2. **~={blue}Build Prefix Sum=~**: Build prefix sum (n + 1) size. This is because...
>3. **~={blue}Build Index Map=~**: Create a map, where in every iteration, we will add the `ith` value in the prefix sum to, along with it's corresponding index. (We will only do this for the first time we encounter the `ith` value as this will yield the longest subarray should it exist)
>4. **~={blue}Locate Target=~**: To identify if we can find a subarray with sum 0, we need to find if the current value of the prefix sum - a prior value = 0. If it exists, obtain the corresponding subarray length by subtracting the current index, with the index of the prior value
>
>```cpp
>int findMaxLength(vector<int​>& nums) {
>	// (0) Preprocessing
>	for (int i{0}; i < nums.size(); ++i) {
>		if (nums[i] == 0) nums[i] = -1;
>	}
>	// (1) Build Prefix Sum
>	int n = nums.size();
>	vector<int​> prefix(n + 1, 0);
>	for (int i{0}; i < n; ++i) {
>		prefix[i + 1] = prefix[i] + nums[i];
>	}
>	
>	unordered_map<int, int​> indexPos;
>	int result{0};
>	
>	for (int i{0}; i < n + 1; ++i) {
>		// Note that our target is our current sum
>		// This is because we are looking for the same value, but
>		// earlier in the prefix sum
>		int target = prefix[i];
>		if (indexPos.find(currentSum) != indexPos.end()) {
>			result = max(result, i - indexPos[currentSum]);
>		} else {
>			// If it wasn't found, then this is the first time it
>			// appeared, thus add it to the indexPos map
>			indexPos[prefix[i]] = i;
>		}
>	}
>	return result;
>}
>```
>
>**~={blue}Two Things to Consider=~**:
> ![[Drawing 2024-12-24 11.17.31.excalidraw | center | 700 ]]

## Other
### Maps with Integers

> [!QUOTE] Quick Notes
> Whenever we need to count the frequency of something.

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
\### Maps with DS

### Maps with DS

> [!QUOTE] Quick Notes
> Whenever we need to store the input in association with a list of something
> 1. Define hash map
> 2. Preprocessing to put input into hash map format
> 3. Utilise hash map

> [!Question]- Group Anagrams
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an array of strings strs, group the anagrams together. You can return the answer in any order.
> * ~={green}Input=~: strs = `["eat","tea","tan","ate","nat","bat"]`
> * **~={red}Output=~**: `[["bat"],["nat","tan"],["ate","eat","tea"]]`
>
>**~={red}Solution=~**:
>
>1. **~={blue}Define Hashmap=~**: `unordered_map<string, vector<string​>​> m`
>* We want to map every sorted string -> string
>* Every string that has the same sorted string = anagram
>2. **~={blue}Preprocessing=~**: Iterate through the input list of strings to create the defined hashmap
>3. **~={blue}Utilise Hashmap through Comparison=~**: Every bucket in the hashmap corresponds to a vector of strings that are anagrams. As such, store them in a vector, and return it.
>
> ![[Drawing 2024-12-23 18.23.54.excalidraw | center | 700]]
>
>```cpp
>vector<vector<string​>> groupAnagrams(vector<string​>& strs) {
>	// (1) Define hash map
>	unordered_map<string, vector<string​>​> m;
>	// (2) Preprocessing
>	for (string s : strs) {
>		string unsorted = s;
>		sort(s.begin(), s.end());
>		m[s].push_back(unsorted);
>	}
>	// (3) Utilise hashmap
>	vector<vector<string​>> result;
>	for (const auto& pair : m) {
>		result.push_back(pair.second);
>	}
>	return result;
>}
>```

> [!Question]- Minimum Consecutive Cards to Pick Up
> <!-- Multiline -->
> **~={red}Question=~**:
> * You are given an integer array cards where cards[i] represents the value of the ith card. A pair of cards are matching if the cards have the same value.
> * Return the minimum number of consecutive cards you have to pick up to have a pair of matching cards among the picked cards. If it is impossible to have matching cards, return -1.
> * ~={green}Input=~: cards = `[3,4,2,3,4,7]`
> * **~={red}Output=~**: 4 (We can pick the cards 3, 4, 2, 3 which contain a matching pair)
>
>**~={red}Solution=~**:
>
>1. **~={blue}Define Hashmap=~**: `unordered_map<int, vector<int​>​> cardPos`
>* We want to map every card -> index positions (a card can appear in different places)
>* Because we are looking at consecutive cards, now that we have a vector of index positions, we can determine the min distance between each card that is of the same value
>2. **~={blue}Preprocessing=~**: Iterate through the input list of cards to create the defined hashmap
>3. **~={blue}Utilise Hashmap through Comparison=~**: Iterate through the index positions of each card value, where we compute the subarray length of each consecutive card, and take the smallest.
>
> ![[Drawing 2024-12-23 21.07.20.excalidraw | center | 700]]
>
>```cpp
>int minimumCardPickup(vector<int​>& cards) {
>	// (1) Define hash map
>	unordered_map<int, vector<int​>​> cardPos;
>	// (2) Preprocessing
>	for (int i{0}; i < cards.size(); ++i) {
>		cardPos[cards[i]].push_back(i);
>	}
>	// (3) Utilise hashmap
>	int minLength{INT_MAX};
>	for (const auto& pair : cardPos) {
>		for (int i{0}; i < pair.second.size() - 1; ++i) {
>			// Subarray length
>			minLength = min(minLength, pair.second[i+1] - pair.second[i] + 1);
>		}
>	}
>	// If minLength is INT_MAX, return -1, otherwise return it's value
>	return minLength == INT_MAX ? -1 : minLength;
>}
>```

> [!Question]- Max Sum of a Pair With Equal Sum of Digits 
> <!-- Multiline -->
> **~={red}Question=~**:
> * You are given a 0-indexed array nums consisting of positive integers. You can choose two indices i and j, such that i != j, and the sum of digits of the number nums[i] is equal to that of nums[j].
> * Return the maximum value of nums[i] + nums[j] that you can obtain over all possible indices i and j that satisfy the conditions.
> * ~={green}Input=~: nums = `[18,43,36,13,7]`
> * **~={red}Output=~**: 54
> 	* (0, 2), both numbers have a sum of digits equal to 9, and their sum is 18 + 36 = 54. } Note: 1 + 8 = 3 + 6 = 9
> 	* (1, 4), both numbers have a sum of digits equal to 7, and their sum is 43 + 7 = 50.
> 	* So the maximum sum that we can obtain is 54.
>
>**~={red}Solution=~**:
>
>1. **~={blue}Define Hashmap=~**: `unordered_map<int, priority_queue<int​>​> matches`
>* We want to map every digit sum -> input values from the array
>* Because every number that has the same digit sum is grouped together, if we take the 2 largest from each group and compare, we will find the largest sum.
>2. **~={blue}Preprocessing=~**: Iterate through the input list of cards to create the defined hashmap
>3. **~={blue}Utilise Hashmap through Comparison=~**: Take the largest 2 values that share the same digit sum and sum them. Do this for every bucket.
>
> ![[Drawing 2024-12-23 21.28.27.excalidraw | center | 700]]
> 
>```cpp
>int maximumSum(vector<int​>& nums) {
>	// (1) Define hash map
>	unordered_map<int, priority_queue<int​>​> matches;
>	// (2) Preprocessing
>	for (int i{0}; i < nums.size(); ++i) {
>		// Convert to digit sum
>		int num[nums[i]];
>		int sum{0};
>		while (num > 0) {
>			sum += num % 10;
>			num /= 10;
>		}
>		matches[sum].push(nums[i]);
>		
>	}
>	// (3) Utilise hashmap
>	int maxSum{-1};
>	for (const auto& pair : cardPos) {
>		if (pair.second.size() > 1) {
>			for (int i{0}; i < pair.second.size(); ++i) {
>				int highest = pair.second.top();
>				pair.second.pop();
>				int secondHighest = pair.second.top();
>				maxSum = max(maxSum, highest + secondHighest);
>			}
>		}
>	}
>	return maxSum;
>}
>```

### Sliding Windows with Maps

> [!Question]- Longest Substring Without Repeating Characters
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a string `s`, find the length of the **longest** **substring** without repeating characters.
> * ~={green}Input=~: s = "abcabcbb"
> * **~={red}Output=~**: 3
> 	* The answer is "abc", with the length of 3.
>
>**~={red}Solution=~**:
>We want to keep track of the number of distinct values are in our array through a map.
>
>* **~={purple}Expanding Window Condition=~**: Always expand, and keep track how much of each value is in the input.
>* **~={purple}Invalid Logic=~**: If what was just added (`s[right]`) has a count that exceeds 1, then we are in a invalid state.
>* **~={purple}Shrinking Window Condition=~**: Always shrink, and reduce count in map.
>* ~={purple}**Process Current Window**=~: Compute the current window length, and compare with the current know max.
>
> ![[Drawing 2024-12-23 21.28.27.excalidraw | center | 700]]
> 
>```cpp
>int lengthOfLongestSubstring(string s) {
>	unordered_map<int, int> count;
>	int left{0};
>	int length{0};
>	// (1) Expand Window Condition
>	for (int right{0}; right < s.size(); ++right) {
>		count[s[right]]++;
>		// (2) Invalid Logic
>		while (count[s[right]] > 1) {
>			count[s[left]]--;
>			// (3) Shrink Window
>			left++;
>		}
>		// (4) Process Current Window
>		length = max(length, right - left + 1);
>	}
>	return length;
>}
>```

#flashcards/dsa/patterns/counting
