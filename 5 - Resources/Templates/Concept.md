# Overview
## Time Complexity

>[!Note]+ Time Complexity of Prefix Sums
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

>[!Note]+ Prefix Sums Recipe
> <!-- Multiline -->
>1. ~={purple}**Prefix Sums=~**:

## Points of Interest

>[!Note]+ What does `prefix[i]` represent?
> <!-- Multiline -->
>The sum of all elements up to index `i` (inclusive)

>[!Note]+ Point of Interest Two
> <!-- Multiline -->
>Response One

# Templates

>[!Info]+ Building Prefix Sum Template
><!-- Multiline -->
><u>**Explanation**</u>
>1. ~={purple}**Set 0th Element**=~: By definition, the 0th element, will be the 0th element in the provided `array`.
>2. ~={purple}**Build Prefix Sum**=~: For each subsequent, `ith` element, we will sum the previous value in the prefix sum, `prefix[i-1]`, with the `ith` value, `array[i]`.
>
><u>**C++ Code**</u>
>```cpp
>int fn(arr) {
>	vector<int​> prefix(arr.size());
>	// (1) Set 0th element in prefix sum
>	prefix[0] = arr[0];
>	// (2) Sum the previous value in prefix sum and current value in
>	// array
>	for (int i{1}; i < arr.size(); ++i) {
>		prefix[i] = prefix[i - 1] + nums[i]
>	}
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

# Example Problems

> [!Question]+ Problem One
> <!-- Multiline -->
<!--SR:!2024-12-11,3,250-->
