> [!QUOTE] Quick Notes
> Once you call a function, the function gets executed and if the program calls another function before the previous function is able to finish, then you basically pause the previous function at the point where it stopped and when the times comes you resume it back.

# Example Problems

> [!Question]- Sum of First $n$ Natural Numbers
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an integer $n$, output the sum of the first 1...n numbers.
>
>**~={red}Solution=~**:
>* **~={purple}Define Math Equation=~**: 
>	* $S(n) = n + S(n - 1)$
>	* $S(0) = 1$
>* **~={purple}Translate Math Equation toe Base Case=~**:
>	* `if (n == 0) return 1`
>* **~={purple}Translate Math Equation to Recursive=~**:
>	* `int sum = n + sum(n - 1)`
>	* `return sum`
>
> ![[Drawing 2024-12-28 23.45.49.excalidraw | center | 300]]
>
>```cpp
>int sum(n) {
>	if (n == 0) return 1;
>	
>	int sum = n + sum(n - 1);
>	return
>}
>```

> [!Question]- Max Number in Array
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a number N and an array A of N numbers. Print the maximum value in this array.
>
>**~={red}Solution=~**:
>To handle traversing through an array, we keep the index in the function call, and increment it in each call
>* **~={purple}Define Math Equation=~**: 
>	* $S(arr, i) = max(arr[i], S(arr, i + 1))$
>	* $S(arr, arr.size()) = -\inf$
>* **~={purple}Translate Math Equation toe Base Case=~**:
>	* `if (index == arr.size()) return INT_MIN`
>* **~={purple}Translate Math Equation to Recursive=~**:
>	* `int max = max(arr[i], maxNum(arr, i + 1))`
>	* `return max`
>
>```cpp
>int maxNumber(vector<int​>& array, int index) {
>	if (index == array.size()) return INT_MIN;
>	
>	int maxNum = max(array[index], maxNumber(array, i + 1))
>	return maxNum;
>}
>```

> [!Question]- Counting 3n+1 Sequence Length
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a number n, you should print the length of the 3n + 1 sequence starting with n.
>* If the number n is odd, the next number will be 3n + 1.
>* If the number n is even, the next number will be n/2.
>* For example, the 3n + 1 sequence of 3 is {3, 10, 5, 16, 8, 4, 2, 1} and its **~={blue}length is 8=~**.
>
>**~={red}Solution=~**:
>To handle returning a count, we can simply return 1 for the base case, and an increment of 1 for the recursive case. Note that the base case may be what shall happen for the smallest value of $n$.
>* **~={purple}Define Math Equation=~**: 
>	* **~={green}If Even=~**: $S(n) = 1 + S(n/2)$
>	* **~={red}If Odd=~**: $S(n) = 1 + S(3n + 1)$
>	* $S(1) = 1$ (When we reach 1, the sequence ends)
>* **~={purple}Translate Math Equation toe Base Case=~**:
>	* `if (n == 1) return 1`
>* **~={purple}Translate Math Equation to Recursive=~**:
>	* `return 1 + sequenceLength(n/2)`
>	* `return 1 + sequenceLength(3 * n + 1)`
>
>```cpp
>int sequenceLength(int n) {
>	if (n == 1) return 1;
>	
>	if (n % 2 == 0) {
>		return 1 + sequenceLength(n / 2);
>	} else {
>		return 1 + sequenceLength(3 * n + 1);
>	}
>}
>```

#flashcards/dsa/patterns/recursion
