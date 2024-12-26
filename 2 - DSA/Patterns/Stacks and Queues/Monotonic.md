> [!QUOTE] Quick Notes
> Finding and determining properties of subcomponents

# Overview
## Recipe

>[!Note]- Monotonic Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Monotonic?**=~:
> * **~={green}Next Greater or Smaller=~**: Whenever we need to find the next greater or smaller element in a sequence

## Time Complexity

>[!Note]- Time Complexity of Motonicity
> <!-- Multiline -->
> $O(n)$ as if we're adding elements from an array to the stack, that element will only ever be at most :
> * **~={green}Added=~**: Once
> * **~={red}Removed=~**: Once
> 
> Hence $O(2n) = O(n)$ 

# Templates

>[!Info]- Monotonically Increasing Stack
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Maintaining Monotonicity**=~: If the value we're pushing to the stack is smaller than the current top value, we will pop from the stack until this is no longer the case
>2. ~={purple}**Adding to Stack**=~: Otherwise, simply push to the stack
>
><u>**C++ Code**</u>
>```cpp
>int fn(arr) {
>	stack<int​> stack;
>	
>	for (int a : arr) {
>		while (!stack.empty() && stack.top() > a) {
>			stack.pop();
>		}
>		// Between the above and below lines, do some logic depending 
>		// on the problem
>		stack.push(a);
>	}
>}
>```
><u>**Visual Explanation**</u>
>
> ![[Drawing 2024-12-26 22.41.35.excalidraw | 700 | center]]

# Example Problems

> [!Question]- Next Greater Element
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

* Daily Temp
* Online Stock Span

#flashcards/dsa/patterns/monotonic
