> [!QUOTE] Quick Notes
>When we recognise a LIFO pattern

# Overview
## Recipe

>[!Note]- String Problem Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Stack=~**: Whenever we recognise a LIFO pattern.
>2. **~={purple}Stack Strategies=~**:

## Points of Interest

>[!Note]- Accessing `stack.top()` or `vector.back()`
> <!-- Multiline -->
>Always check if the stack or vector is `.empty()` before doing so to avoid getting exceptions.

# Example Problems

> [!Question]- Remove All Adjacent Duplicates In String
> <!-- Multiline -->
> **~={red}Question=~**:
> * You are given a string `s`. Continuously remove duplicates (two of the same character beside each other) until you can't anymore. Return the final string after this.
>
>**~={red}Solution=~**:
>1. **~={blue}Recognise LIFO=~**: The last character we add to the stack, is the first that shall be checked, and if it is adjacent, we will remove it (LIFO)
>2. **~={blue}Remove from Stack Logic=~**: If the stack is not empty, then we shall remove the top item, if it is adjacent
>3. **~={blue}Add to Stack Logic=~**: Otherwise, we add to the stack
>4. **~={blue}Reverse String=~**: Pop from stack, then reverse the string
>
> ![[Drawing 2024-12-26 16.34.34.excalidraw | 700 | center]]
>```cpp
>string removeDuplicates(string s) {
>	vector<char​> stack{};
>	for (char c : s) {
>		// (2) Remove from Stack
>		if (!stack.empty() && stack.back() == c) {
>			stack.pop_back();
>		} else {
>		// (3) Add to Stack
>			stack.push_back(c);
>		}
>		
>		// (4) Convert/Reverse String (Reverse necessary if Stack
>		// DS was used)
>		string result{stack.begin(), stack.end()};
>		return result;
>	}
>}
>```

> [!Question]- Simplify Path
> <!-- Multiline -->
> **~={red}Question=~**:
> * You are given a string `s`. Continuously remove duplicates (two of the same character beside each other) until you can't anymore. Return the final string after this.
>
>**~={red}Solution=~**:
>1. **~={blue}Recognise LIFO=~**: The last character we add to the stack, is the first that shall be checked, and if it is adjacent, we will remove it (LIFO)
>2. **~={blue}Remove from Stack Logic=~**: If the stack is not empty, then we shall remove the top item, if it is adjacent
>3. **~={blue}Add to Stack Logic=~**: Otherwise, we add to the stack
>4. **~={blue}Reverse String=~**: Pop from stack, then reverse the string
>
> ![[Drawing 2024-12-26 16.34.34.excalidraw | 700 | center]]
>```cpp
>string removeDuplicates(string s) {
>	vector<char​> stack{};
>	for (char c : s) {
>		// (2) Remove from Stack
>		if (!stack.empty() && stack.top() == c) {
>			stack.pop_back();
>		} else {
>		// (3) Add to Stack
>			stack.push_back(c);
>		}
>		
>		// (4) Convert/Reverse String (Reverse necessary if Stack
>		// DS was used)
>		string result{stack.begin(), stack.end()};
>		return result;
>	}
>}
>```

#flashcards/dsa/patterns/stringproblems
