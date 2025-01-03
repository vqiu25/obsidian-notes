> [!QUOTE] Quick Notes
>When problems involve cycles, middle elements, or "meeting points" in linked lists

# Overview
## Recipe

>[!Note]- Fast and Slower Pointers Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Fast & Slow Pointers=~**: Whenever problems involve cycles, middle elements, or "meeting points" in linked lists.

## Time Complexity

>[!Note]- Time Complexity of Fast and Slow Pointers
> <!-- Multiline -->
> * Two Pointers, so $O(n)$

## Points of Interest

>[!Note]- Why do we return `head`
> <!-- Multiline -->
>Instead of returning the entire linked list, we return the beginning of the linked list.

>[!Note]- Allocating a Pointer on the Stack vs Heap
> <!-- Multiline -->
> Assume the following ListNode definition:
> ```cpp
> struct ListNode {
> 	int val;
> 	ListNode* next;
> 	ListNode(int x, ListNode *next) : 
> 		val(x), 
> 		next(next) {}
> }
>```
>* **~={green}Stack allocation=~** (e.g. `ListNode dummy{-1, head};`) is typically used for small, short-lived objects where automatic cleanup when exiting the function is desired
>* **~={red}Heap allocation=~** (e.g. `ListNode* dummy = new ListNode{-1, head};`) is for dynamic objects that need to survive beyond their current scope or when you need more control over memory lifetime. However, you must remember to `delete` them.
>````col
>```col-md
>**~={green}Stack Example=~**
>```cpp
>// Stack Allocation
>ListNode dummy{-1, head};
>
>// If we want pointers pointing at the above node
>ListNode* fast{&dummy};
>
>// Access variables inside 
>return dummy.next;
>```
>```col-md
>**~={red}Heap Example=~**
>```cpp
>// Heap Allocation
>ListNode* dummy = new ListNode{-1, head};
>
>// If we want pointers pointing at the above node/point at the same thing
>ListNode* fast{dummy};
>
>// Access variables inside 
>return dummy->next;
>```
>````

>[!Note]- Purpose of dummy pointers
> <!-- Multiline -->
> (To keep track of the head for final return)
>To aid in situations where:
>* ~={blue}**Delete Or Losing Where Head Node Is**=~: It prevents problems if the head itself needs to be removed (there's always a “previous” node). In that case, we will always be returning `dummy.next`.
>* **~={blue}Prevent out of Bounds=~**: If we need iterate `k + 1` on the linked list, but the linked list is only size `k`, then it will avoid that exception

>[!Note]- Why do we return `dummy.next`
> <!-- Multiline -->
>If the head pointer is removed, then returning `dummy.next` will give the desired output. This is because the head pointer is now pointing at something that is no longer "valid".
>
> ![[Drawing 2024-12-25 11.08.08.excalidraw | 450 | center]]

>[!Note]- How to append nodes together
> <!-- Multiline -->
> ![[Drawing 2024-12-25 18.04.08.excalidraw | 700 | center]]

# Templates

>[!Info]- Fast and Slow Pointers
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Set Slow and Fast Pointers**=~: Have them point to where the head is.
>2. **~={purple}While loop Condition=~**: We use `while (current != nullptr && current->next != nullptr)` to ensure `current` and `current->next` both exist before comparing values. This way, we don't go out of bounds, and we can safely move through every node in the list.
>3. ~={purple}**Increment Pointers**=~: Have the slow pointer increment once, and the fast pointer increment twice.
>
><u>**C++ Code**</u>
>```cpp
>int fn(Node* head) {
>	// (1) Set Slow and Fast Pointers
>	Node* slow = head;
>	Node* fast = head;
>	
>	// (2) While Loop Condition
>	// This is to also avoid exceptions:
>	// * (fast)->next
>	// * (fast->next)->next
>	// Stuff in brackets cannot be null!
>	while (fast != nullptr && fast->next != nullptr) {
>		// (3) Increment Pointers
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
## Fast and Slow Pointers

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
> * Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
> * **~={green}Input=~**: `head = [3,2,0,-4]`, `pos = 1`
> * ~={red}Output=~: true
> 
> ![[Drawing 2024-12-24 21.49.33.excalidraw | 250 | center]]
> 
>**~={red}Solution=~**:
>1. **~={purple}Set Fast and Slow Pointers=~**: Trivial.
>2. **~={purple}Increment Pointers=~**: If we have one pointer moving twice as fast as the other, the fast pointer will reach overlap and reach the slow pointer eventually should there be a cycle.
>
>```cpp
>bool hasCycle(ListNode* head) {
>	Node* fast = head;
>	Node* slow = head
>	}
>	while (fast != nullptr && fast->next != nullptr) {
>		slow = slow->next;
>		fast = fast->next->next;
>		if (slow == fast) {
>			return true;
>		}
>	}
>	return false;
>}
>```
>
>**~={purple}Will Fast Pointer always catch up with Slow Pointer?=~**
>* The fast pointer moves 2 steps, and the slow pointers moves 1 step. Thus in every iteration, the fast pointer will gain a distance of 1 node at every iteration.
>* Hence, the max number of steps required for the fast pointer to catch up with the slower pointer is $k$ steps, where $k$ is the length of the cycle. The worst case happens when:
>
> ![[Drawing 2024-12-24 22.05.28.excalidraw | 700 | center]]

> [!Question]- Retreive Kth Node from End of List
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given the head of a linked list and an integer k, return the kth node from the end.
> 
>**~={red}Solution=~**:
>1. **~={purple}Set Fast and Slow Pointers=~**: Trivial.
>2. **~={purple}Increment Pointers=~**: First shift the fast pointer ahead by k. Then increment them both till the fast pointer reaches the end/nullptr.
>
>```cpp
>Node* removeNthFromEnd(ListNode* head, int k) {
>	Node* fast = head;
>	Node* slow = head;
>	for (int i{0}; i < k; i++) {
>		fast = fast->next;
>	}
>	while (fast != nullptr) {
>		slow = slow->next;
>		fast = fast->next
>	}
>	return slow;
>}
>```

> [!Question]- Remove Kth Node from End of List
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given the head of a linked list and an integer k, return the kth node from the end.
> 
>**~={red}Solution=~**:
>1. **~={blue}Check if Size > 1=~**: If the linked list only has 1 node, immediately return `nullptr`
>2. **~={blue}Create Dummy Node=~**: This is to deal with the possibility of removing the head node, and also, to deal with when k = 2, and the size of the linked list is also 2
>3. **~={blue}Iterate K + 1=~**:
>	1. Iterate K: We want the fast pointer to be k ahead, as we want to remove the kth value from the end
>	2. Iterate + 1: That said, we want to be at the node, 1 before the kth value from the end, so that we can "skip" over the kth value, removing it.
>4. **~={blue}Iterate Pointers=~**: Iterate pointers to the end
>5. **~={blue}Return dummy.next=~**: As if we removed the head pointer, what it points to is now irrelevant
>
>```cpp
>ListNode* removeKthFromEnd(ListNode* head, int k) {
>	// (1) If the linked list is only size 1, return nullptr as
>	// deleting any node will cause it to be empty
>	if (head->next == nullptr) return nullptr;
>
>	// (2) Create a dummy node that points to head
>	ListNode dummy{-1, head};
>	
>	ListNode* slow{&dummy};
>	ListNode* fast{&dummy};
>	
>	// (3) Iterate K + 1
>	for (int i{0}; i < k + 1; i++) {
>		fast = fast->next;
>	}
>	
>	// (4) Increment Pointers to the End
>	while (fast != nullptr) {
>		slow = slow->next;
>		fast = fast->next
>	}
>	
>	// (5) Return dummy.next, instead of head
>	return dummy.next;
>}
>```

## Dummy Pointers

> [!Question]- Remove Linked List Elements
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return _the new head_.
> 
>**~={red}Solution=~**:
>0. **~={blue}Dummy Setup=~**:
>* Create Dummy Node That Points to Head
>* Create a Prev Pointer that Points to the Dummy Node:
>* Create a Curr Pointer that Points to Head
>1. **~={blue}If the target value is found=~**: The next of the prev pointer, has to be one after the curr pointer
> ![[Drawing 2024-12-25 21.11.38.excalidraw | center | 300]]
>3. **~={blue}If the target value is not found=~**: Shift the prev pointer to where the current pointer is
> ![[Drawing 2024-12-25 21.15.39.excalidraw |600 | center]]
>5. **~={blue}Move where the curr pointer is to the right by one=~**
>
>```cpp
>ListNode* removeElements(ListNode* head, int val) {
>	ListNode dummy{-1, head};
>	ListNode* prev{&dummy};
>	ListNode* curr{head};
>
>	while (curr != nullptr) {
>		if (curr->val == val) {
>			prev->next = curr->next;
>		else {
>			prev = curr;
>		}
>		curr = curr->next;
>	}
>	return dummy.next;
>}
>```
>
>**~={purple}Will Fast Pointer always catch up with Slow Pointer?=~**
>* The fast pointer moves 2 steps, and the slow pointers moves 1 step. Thus in every iteration, the fast pointer will gain a distance of 1 node at every iteration.
>* Hence, the max number of steps required for the fast pointer to catch up with the slower pointer is $k$ steps, where $k$ is the length of the cycle. The worst case happens when:
>

> [!Question]- Remove Duplicates from Sorted List
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given the `head` of a sorted linked list, _delete all duplicates such that each element appears only once_. Return _the linked list **sorted** as well_.
>
> ![[Drawing 2024-12-24 22.27.10.excalidraw | 250 | center]]
>
>**~={red}Solution=~**:
>1. **~={purple}While Loop Condition=~**: We use `while (current != nullptr && current->next != nullptr)` to ensure `current` and `current->next` both exist before comparing values. This way, we don't go out of bounds, and we can safely move through every node in the list.
>2. **~={purple}If Duplicate Detected=~**: If `current->val == current->next->val`, we remove the duplicate by skipping the next node (`current->next = current->next->next`).
>* We'd want to deallocate `current->next` if found
>	* Node* duplicate = current->next;
>	* current->next = current->next->next;
>	* delete duplicate;
>1. **~={purple}Otherwise Go Subsequent Node=~**: If the next node isn’t a duplicate, we simply advance to the next node (`current = current->next`).
>
>```cpp
>Node* deleteDuplicates(Node* head) {
>	Node* current = head;
>	
>	// (1) While Loop Condition
>	while (current != nullptr && current->next != nullptr) {
>		// (2) If duplicate detected
>		if (current->val == current->next->val) {
>			current->next = current->next->next;
>		} else {
>			// (3) Otherwise, go to subsequent node
>			current = current->next;
>		}
>	}
>	return head;
>}
>```
> **~={purple}If Duplicate Found Visual=~**
> 
>  ![[Drawing 2024-12-24 22.45.05.excalidraw | center | 350]]


#flashcards/dsa/patterns/linkedlist/fastandslowpointers
