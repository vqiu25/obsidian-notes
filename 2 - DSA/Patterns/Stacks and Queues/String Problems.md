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

> [!Question]- Remove Stars from a String  
> <!-- Multiline -->  
> ~={red}**Question**=~:  
> * You are given a string `s`, which contains stars `*`.
> * In one operation, you can:
> 	* Choose a star in `s`.
> 	* Remove the closest **non-star** character to its **left**, as well as remove the star itself.
> * Return _the string after **all** stars have been removed_.
>  
> **~={red}Solution=~**:  
> 1. ~={blue}**Recognise LIFO (Last-In-First-Out) Structure**=~:  
>    - The last added character is the first to be removed when encountering a `'*'`.  
>    - A **stack** is ideal for this problem.  
> 2. ~={blue}**Handle '*' Removal**=~:  
>    - If the current character is `'*'`, remove the last added character using `stack.pop_back()`.  
> 3. ~={blue}**Push Non-'*' Characters**=~:  
>    - If the character is not `'*'`, push it to the stack.  
> 4. ~={blue}**Reconstruct the Final String**=~:  
>    - Convert the stack back into a string and return it.  
>  
> **~={green}Code=~**:  
> ```cpp  
> class Solution {  
> public:  
>     string removeStars(string s) {  
>         deque<char​> stack;  
>  
>         for (char c : s) {  
>             if (c == '*') {  
> 	            // (2) Remove the last added character  
>                 stack.pop_back();
>             } else {  
> 	            // (3) Add character to stack 
>                 stack.push_back(c); 
>             }  
>         }  
>  
>         // (4) Convert stack to string  
>         return string(stack.begin(), stack.end());
>     }  
> };  
> ```  
>  
> ~={green}**Key Points**=~:  
> * **Time Complexity**:  
>   - **O(n)**: We iterate through `s` once, performing `push_back()` and `pop_back()` operations.  
> * **Space Complexity**:  
>   - **O(n)**: In the worst case, the entire string (without stars) is stored in the stack.  

#flashcards/dsa/patterns/stringproblems
