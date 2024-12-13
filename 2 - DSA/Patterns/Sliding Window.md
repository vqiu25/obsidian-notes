# Overview
## Time Complexity

>[!Note]+ Time Complexity of Sliding Windows
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
## Recipe

>[!Note]+ Sliding Windows Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Sliding Windows**=~:
>2. ~={purple}**Dynamic or Fixed**=~:
>
>**<u>Dynamic</u>**
>
>**<u>Fixed</u>**
## Points of Interest

>[!Note]+ Point of Interest One
> <!-- Multiline -->
>Response One

>[!Note]+ Left Pointer Passing Right Pointer in Dynamic Sliding Window
> <!-- Multiline -->
>Response One

>[!Note]+ Computing Total Number of Subarrays in a Sliding Window
> <!-- Multiline -->
>If we have a window $(left, right)$, how many valid windows/subarrays exist that end at $right$.
>````col 
>```col-md 
> ![[Drawing 2024-12-10 17.26.29.excalidraw | center | 250]]
>``` 
>```col-md 
>For a particular window, starting at index $left$ and ending at index $right$, there are $right-left+1$ subarrays ending at $right$. 
>
>As an example, there are ~={red}3 valid subarrays=~, which can be shown via ~={green}3 - 1=~ + 1 = 3
>``` 
>```` 
>
>

# Templates

>[!Info]+ Dynamic Window Sizes Template
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Adding Elements to Window**=~: We will shift the right pointer once every iteration in our for loop.
>2. ~={purple}**Removing Elements from Window**=~: The moment we've shifted our **right** pointer, our window may fall into an ~={red}invalid=~ state. As such, we will move our left **pointer** up until we reach a ~={green}valid=~ state.
>3. ~={purple}**Updating Answer**=~: Update our result once we have reached a ~={green}valid=~ state.
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
>		// (3) Do some logic to update the answer
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

>[!Info]+ Fixed Window Size Template
><!-- Multiline -->
>**<u>Explanation</u>**
>1. **~={purple}Initialisation=~**: $k$ is the size of our fixed window size
>2. **~={purple}Build Initial Window=~**: Sum the first $k$ elements
>3. **~={purple}Slide Window=~**: Shift the window by adding the $kth$ element and so on. Additionally, remove elements from index 0.
>4. **~={purple}Update Answer=~**: Update answer based on new window
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
>		// (4) Update Answer
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

> [!Question]+ Problem One
> <!-- Multiline -->
<!--SR:!2024-12-11,3,250-->
