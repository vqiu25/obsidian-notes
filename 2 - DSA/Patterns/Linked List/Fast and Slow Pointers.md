> [!QUOTE] Quick Notes
>* We have 2 pointers, that move at different speeds
>* When the pointers move at different speeds, usually the "fast" pointer moves two nodes per iteration, whereas the "slow" pointer moves one node per iteration (although this is not always the case).

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
> * Two Pointers, so $O(n)$

## Points of Interest

>[!Note]- What does `prefix[i]` represent?
> <!-- Multiline -->
>The sum of all elements up to index `i` (inclusive)
>
> ![[Drawing 2024-12-16 08.22.39.excalidraw | center | 550]]

# Templates

>[!Info]- Fast and Slow Pointers
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Set Slow and Fast Pointers**=~: Have them point to where the head is.
>2. ~={purple}**Increment Pointers**=~: Have the slow pointer increment once, and the fast pointer increment twice.
>
><u>**C++ Code**</u>
>```cpp
>int fn(Node* head) {
>	Node* slow = head;
>	Node* fast = head;
>	
>	while (fast != nullptr && fast->next != nullptr) {
>		// Do something here
>		slow = slow->next;
>		fast = fast->next->next;
>	}
>}
>```
><u>**Visual Explanation**</u>
>
> ![[Drawing 2024-12-24 21.08.54.excalidraw | center | 700]]
# Example Problems

> [!Question]- Middle of Linked List
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given the `head` of a singly linked list, return _the middle node of the linked list_.
> * **~={green}Input=~**: `head = [1,2,3,4,5]`
> * ~={red}Output=~: `[3,4,5]` as the middle node of the list is node 3.
>
>**~={red}Solution=~**:
>1. **~={purple}Set Fast and Slow Pointers=~**: Trivial.
>2. **~={purple}Increment Pointers=~**: If we have one pointer moving twice as fast as the other, then by the time it reaches the end, the slow pointer will be halfway through since it is moving at half the speed.
>* Note: If we wanted to return the first middle instead, our while condition would be `(fast->next != nullptr && fast->next->next != nullptr)`
>
>```cpp
>Node* middleNode(ListNode* head) {
>	Node* fast = head;
>	Node* slow = head
>	}
>	while (fast != nullptr && fast->next != nullptr) {
>		slow = slow->next;
>		fast = fast->next->next;
>	}
>	return slow;
>}
>```
> ![[Drawing 2024-12-24 21.28.01.excalidraw | center | 700]]

> [!Question]- Linked List Cycle
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

Return Kth Node from End

Remove Duplicates from Sorted List

#flashcards/dsa/patterns/fastandslowpointers
