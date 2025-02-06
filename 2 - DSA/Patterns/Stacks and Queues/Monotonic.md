> [!QUOTE] Quick Notes
> May be useful when we need to find the next greater or smaller element in a sequence

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
>		// (&& SOME CONDITION)
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

> [!Question]- Daily Temperatures
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an array of integers `temperatures` represents the daily temperatures, return _an array_ `answer` _such that_ `answer[i]` _is the number of days you have to wait after the_ `ith` _day to get a warmer temperature_. If there is no future day for which this is possible, keep `answer[i] == 0` instead.
>* **~={green}Input=~**: temperatures = `[73,74,75,71,69,72,76,73]`
>* ~={red}**Output**=~: `[1,1,4,2,1,1,0,0]`
>
> ![[Drawing 2024-12-27 15.11.55.excalidraw | 300 | center]]
>
>**~={red}Solution=~**:
>0. **~={blue}Stack Purpose=~**: The stack stores indices of the `temperatures` array, and the values at these indices represent days with temperatures that haven't yet found a "next warmer day."
>0. **~={blue}Monotonic Property=~**: The stack is monotonic decreasing order, so everytime we encounter temp larger then the top element, we keep popping until that is no longer the case
> 
>The key thing to note here is that:
>
> ````col
>```col-md
>**~={pink}If the next element is smaller=~**
>We just keep adding to the stack:
> ![[Drawing 2024-12-27 15.16.52.excalidraw | center | 250]]
>```
>```col-md
>**~={green}If the next element is greater=~**
>1. We check if is greater than the element on the top of the stack
>2. If so, store any results into `result`
>3. Then `.pop()` to remove the top temp from the stack
>4. Add the "greater temp" on to the stack
>
> ![[Drawing 2024-12-27 15.33.20.excalidraw | 500 | center]]
>
>```
>````
> 
>```cpp
>vector<int​> dailyTemperatures(vector<int​>& temperatures) {
>	stack<int​> s;
>	vector<int​> result(temperatures.size());
>	
>	// (2) Iterate through temperatures array
>	for (int i{0}; i < temperatures.size(); ++i) {
>		// (2.1) If the current temp is greater than the temp at
>		// the top of the stack
>		int currentTemp = temperatures[i];
>		while (!s.empty() && currentTemp > temperatures[s.top()]) {
>			// (2.2) Store that value by subtracting the index of
>			// the current temp minus the index of the temp at the
>			// top of the stack
>			result[s.top()] = i - s.top();
>			s.pop();
>		}
>		// (4) Add the next element to the stack
>		s.push(i);
>	}
>	return result;
>}
>```

> [!Question]- Next Greater Element
> <!-- Multiline -->
> **~={red}Question=~**:
>* The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.
>* For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2024-12-27 12.33.03.excalidraw | 700 | center]]
> 
>```cpp
>vector<int​> nextGreaterElement(vector<int​>& nums1, vector<int​>& nums2) {
>	unordered_map<int, int​> indexPos;
>	stack<int​> s;
>	vector<int​> result(nums1.size(), -1);
>	
>	// (1) Store Index Positions of Nums 1 (Value -> Index)
>	for (int i{0}; i < nums1.size(); ++i) {
>		indexPos[nums1[i]] = i;
>	}
>	
>	// (2) Iterate through num2 array
>	for (int i{0}; i < nums2.size(); ++i) {
>		// (2.1) If the current top value in the stack is smaller
>		// than the next value in nums2 (that means we've found the
>		// next largest)
>		while (!s.empty() && s.top() < nums2[i]) {
>			// (2.2) If the value on the top is contained in nums1
>			if (indexPos.find(s.top()) != indexPos.end()) {
>				// (2.2) Store that value
>				result[indexPos[s.top()]] = nums2[i];
>			}
>			// (3) Keep popping until the current top value is no
>			// longer smaller than the next value in nums2
>			s.pop();
>		}
>		// (4) Add the next element to the stack
>		s.push(nums2[i]);
>	}
>	return result;
>}
>```

> [!Question]- Asteroid Collision  
> <!-- Multiline -->  
> **~={red}Question=~**:  
> * You are given an array `asteroids` where:  
>   - **Positive values** move **right**.  
>   - **Negative values** move **left**.  
> * When two asteroids **collide**, the larger one remains.  
> * If they are equal in size, both are destroyed.  
> * Return the state of the asteroids **after all collisions**.  
>  
> **~={red}Solution=~**:  
> 1. **~={blue}Recognise LIFO (Last-In-First-Out) Nature=~**:  
>    - We process asteroids in order, but collisions depend on the **last added** asteroid.  
>    - When adding to the stack, an asteroid and propogate through, destroying multiple other asteroids. This is monotonocity.  
> 2. **~={blue}Iterate through Asteroids=~**:  
>    - We will check the last 2 elements of the stack repeatedly until we enter a steady state
>    - In particular, the stack must have:
> 	   - At least 2 items in it
> 	   - The second to last asteroid must be going ->, and the top asteroids must be going <-, we enter a while loop until neither of these conditions are the case anymore
> 3. **~={blue}Handle Collisions=~**:  
>      - **Smaller one is destroyed**: The one with a **smaller absolute value** is removed.  
>      - **Equal sizes destroy each other**.  
>      - **Larger one survives**.  
> 4. ~={blue}**Convert Stack to Output**=~:  
>    - The stack holds the remaining asteroids after all collisions.  
>  
> ~={green}**Code**=~:  
> ```cpp  
> class Solution {  
> public:  
>     vector<int​> asteroidCollision(vector<int​>& asteroids) {  
>         deque<int​> stack;  
>         stack.push_back(asteroids[0]);  
>  
>         for (int i{1}; i < asteroids.size(); ++i) {  
>             stack.push_back(asteroids[i]);  
> 			
>             // (*) Because we want to pop twice next,
>             // we need to make sure the size is at least 2
>             if(stack.size() <= 1) continue;  
>  
>             int topValue = stack.back();  
>             stack.pop_back();  
>             int secondTopValue = stack.back();  
>             stack.push_back(topValue);  
>  
>             while (stack.size() >= 2 && topValue < 0 
>             && secondTopValue >= 0) {  
>                 if (abs(topValue) == abs(secondTopValue)) {  
>                     stack.pop_back();  
>                     stack.pop_back();  // Both are destroyed  
>                 } else if (abs(topValue) > abs(secondTopValue)) {  
>                     stack.pop_back();  
>                     stack.pop_back();  
>                     stack.push_back(topValue);  // Larger asteroid remains  
>                 } else if (abs(topValue) < abs(secondTopValue)) {  
>                     stack.pop_back();  // Top asteroid is destroyed  
>                 }  
> 					 
>                 // (*)
>                 if (stack.size() <= 1) break;  
>                 else {  
>                     topValue = stack.back();  
>                     stack.pop_back();  
>                     secondTopValue = stack.back();  
>                     stack.push_back(topValue);  
>                 }  
>             }  
>         }  
>  
>         return vector<int​>{stack.begin(), stack.end()};  
>     }  
> };  
> ```  
>  
> ~={green}**Key Points**=~:  
> * ~={blue}**Time Complexity**=~:  
>   - **O(n)**: Each asteroid is processed once, and we pop from the stack at most `n` times.  
> * **~={blue}Space Complexity=~**:  
>   - **O(n)**: The worst case is no collisions, and all asteroids remain in the stack.  

# Good Problems

* leetcode.com/problems/online-stock-span/description/

#flashcards/dsa/patterns/monotonic
