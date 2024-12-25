> [!QUOTE] Quick Notes
> **~={purple}Reverse Process=~**:
>1. Save a reference to the next node (`nextNode = curr.next`)
>2. Change the current node's next pointer to link the to the previous node (`curr.next = prev`)
>3. Move both `prev` and and `curr` forward by one

# Overview
## Recipe

>[!Note]- Reversing Linked List Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Prefix Sums=~**: Whenever we have to determine the sum or product of subarrays efficiently.
>2. **~={purple}Prefix Sum Strategies=~**:
>* **~={blue}Subarray Sums=~**:
>	* **~={red}When to Use=~**: Need to find the sum of subarrays.
>	* **~={red}What to Note=~**:
>		* **~={pink}Define prefix[i]=~**: Define to contain everything before or, instead inclusive.
>		* **~={pink}Subquery Logic=~**: How to query the prefix sum. Usually is a range between index `i` and `j` from the original array.

## Time Complexity

>[!Note]- Time Complexity of Reversing Linked List
> <!-- Multiline -->
> * Two Pointers, so $O(n)$

## Points of Interest

>[!Note]- Why do we return `head`
> <!-- Multiline -->
>Instead of returning the entire linked list, we return the beginning of the linked list.

# Templates

>[!Info]- Reversing Linked List
><!-- Multiline -->
><u>**Explanation**</u>
>1. **~={blue}Create a Prev Pointer=~**: This will always point exactly one before where we are at, starting at `nuillptr`. This will mean the end of our reversed linked list will end at `nullptr`.
>2. **~={blue}Create a Curr Pointer=~**: This will traverse the entire linked list, starting from `head`.
>3. **~={blue}Traverse while Curr != nullptr=~**:
>	1. **~={blue}Create nextNode=~**: This will be the node after the `curr` pointer. This is done so we don't lose the position of our next node. 
>	2. **~={blue}Reverse Direction=~**: We will then reverse the direction of the pointer at `curr`, performing, `curr->next = prev`
>	3. **~={blue}Setup Prev Node for next Iteration=~**: We then set the current node, to be the `prev` node. This will be reversed in the next iteration.
>	4. **~={blue}Setup Curr Node for Next Iteration=~**: Shift the current node to where the next node is.
> 
>
><u>**C++ Code**</u>
>```cpp
>Node* reverseList(Node* head) {
>	// Having prev point at a null pointer (object type irrelevant atm)
>	Node* prev{nullptr};
>	Node* curr{head};
>	while (curr != nullptr) {
>		Node* nextNode = curr->next; // So we can iterate to next node
>		curr->next = prev; // Reverse direction
>		prev = curr; // Next node to be reversed
>		curr = nextNode; // Next node to traverse to
>	}
>	return prev;
>}
>```
><u>**Visual Explanation**</u>
>
> ![[Drawing 2024-12-25 13.12.25.excalidraw | center | 700]]

# Example Problems

> [!Question]- Reverse Linked List II
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return _the reversed list_.
>
>**~={red}Solution=~**:
>1. **~={blue}Create Dummy=~**: In case we reverse the head node.
>2. **~={blue}Create a PrevLeft Pointer=~**: This pointer will be right before the section we reverse. This is done, so later we can connect this pointer and, set it's next property to the end of the reversed list.
>3. **~={blue}Create a Prev and Curr Pointer=~**: This is done to reverse the specified section (review template)
>
> ![[Drawing 2024-12-26 10.25.40.excalidraw | center | 400]]
>
>4. **~={blue}Reverse Section=~**: To so for $right - left$ times
>
> ![[Drawing 2024-12-26 10.26.18.excalidraw | center | 700]]
>
>5. **~={blue}Connect the=~**:
>* The beginning of the new merged section with the "end" of the linked list
>* Next of the pointer right before the merged section with the new beginning of the reversed list
>
> ![[Drawing 2024-12-26 08.29.30.excalidraw | center | 700]]
>
>```cpp
>ListNode* reverseBetween(ListNode* head, int left, int right) {
>	// (1) Create Dummy
>	ListNode* dummy = new ListNode(-1, head);
>	ListNode* prevLeft = dummy;
>	
>	// (2) Create Prev Left Pointer
>	for (int i{1}; i < left; ++i) {
>		prevLeft = prevLeft->next;
>	}
>	
>	// (3) Create Prev and Curr Pointer
>	ListNode* prev = prevLeft->next;
>	ListNode* curr = prev->next;
>	int numTraversed{right - left};
>	
>	// (4) Reverse Section
>	for (int i{1}; i <= numTraversed; ++i) {
>		ListNode* nextNode = curr->next;
>		curr->next = prev;
>		prev = curr;
>		curr = nextNode;
>	}
>	
>	// (5) Connect
>	prevLeft->next->next = curr;
>	prevLeft->next = prev;
>	return dummy->next;
>}
>```

#flashcards/dsa/patterns/reversinglinkedlist
