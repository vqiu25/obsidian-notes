> [!QUOTE] Quick Notes
> Searching an imaginary space

# Overview
## Recipe

> [!Note]- Binary Search Recipe
> <!-- Multiline -->
> 1. ~={purple}**Why Binary Search**=~: Use binary search on the solution space when solving "maximum" or "minimum" problems, provided:
>    - You can efficiently verify if a condition is satisfied for a given value \(x\).
>    - **~={green}Maximum Problems=~**:
>        - If \(x\) is possible, all values \(< x\) are also possible.
>        - If (x) is not possible, all values (> x) are also not possible.
>    - **~={red}Minimum Problems=~**:
>        - If \(x\) is possible, all values \(> x\) are also possible.
>        - If \(x\) is not possible, all values \(< x\) are also not possible.

## Points of Interest

> [!Note]- Min or Max K that Satisfies Condition
> <!-- Multiline -->
> 1. ~={purple}**Min K**=~:
>    - **~={green}Condition=~**: When `check(mid)` is satisfied.
>    - **~={green}Action=~**: Set `right = mid - 1` to continue searching for smaller values.
>    - ~={green}**Why**=~: We are looking for the smallest value that satisfies the condition, so we narrow the search space to the left.
> 
> 2. ~={purple}**Max K**=~:
>    - ~={green}**Condition**=~: When `check(mid)` is satisfied.
>    - ~={green}**Action**=~: Set `left = mid + 1` to continue searching for larger values.
>    - **~={green}Why=~**: We are looking for the largest value that satisfies the condition, so we narrow the search space to the right.

> [!Note]- Why do we use `<=` here for maximal and minimal
> <!-- Multiline -->
> - Using `<=` ensures that all potential values in the search space, including the last candidate when `left == right`, are considered.
> - **~={purple}This is crucial because=~**:
>    - **~={green}For maximal=~**, the largest value satisfying the condition is at `right` after the loop ends.
>    - **~={red}For minimal=~**, the smallest value satisfying the condition is at `left` after the loop ends.
> - Without `<=`, the loop would exit prematurely, potentially missing valid candidates.

# Templates

> [!Info]- Binary Search on Solution Space (Minimal K)
> <!-- Multiline -->
> **<u>Explanation</u>**
> 1. **~={purple}Define Search Space=~**:
>    - `left`: The smallest possible value for the answer.
>    - `right`: The largest possible value for the answer.
> 2. **~={purple}Exit Condition=~**:
>    - Use `while(left <= right)` to explore all possible values in the search space.
> 3. **~={purple}Narrow Search Space=~**:
>    - Compute `mid` as the potential candidate for the answer.
>    - Evaluate `check(mid)`:
>      - **~={green}If true=~**: Narrow the search to smaller values, setting `right = mid - 1`.
>      - **~={red}If false=~**: Narrow the search to larger values, setting `left = mid + 1`.
> 4. **~={purple}Return Value=~**:
>    - The final value of `left` is the smallest value that satisfies the condition defined in `check(mid)`.
>
>**<u>C++ Code</u>**
> ```cpp
> int fn(vector<int​>& arr) {
>     // (1) Define Search Space
>     int left = MINIMUM_POSSIBLE_ANSWER;
>     int right = MAXIMUM_POSSIBLE_ANSWER;
>     
>     // (2) Exit Condition
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>         
>         // (3) Narrow Search Space
>         if (check(mid)) {
>             right = mid - 1; // Condition satisfied, look for smaller vals
>         } else {
>             left = mid + 1; // Condition not satisfied, look for larger vals
>         }
>     }
>     
>     // (4) Return smallest value that satisfies the condition
>     return left;
> }
>
> // Function to evaluate the condition for a given value
> bool check(int x) {
>     // Implement based on problem requirements
>     return BOOLEAN;
> }
> ```
> **~={green}Key Points=~**
> - **~={purple}Time Complexity=~**:
>    - $O(\log(\text{search space size})) \times O(\text{check function complexity})$.
> - **~={purple}Space Complexity=~**:
>    - $O(1)$ for the iterative approach.

> [!Info]- Binary Search on Solution Space (Maximal K)
> <!-- Multiline -->
> **<u>Explanation</u>**
> 1. **~={purple}Define Search Space=~**:
>    - `left`: The smallest possible value for the answer.
>    - `right`: The largest possible value for the answer.
> 2. **~={purple}Exit Condition=~**:
>    - Use `while(left <= right)` to explore all possible values in the search space.
> 3. **~={purple}Narrow Search Space=~**:
>    - Compute `mid` as the potential candidate for the answer.
>    - Evaluate `check(mid)`:
>      - **~={green}If true=~**: Narrow the search to larger values, setting `left = mid + 1`.
>      - **~={red}If false=~**: Narrow the search to smaller values, setting `right = mid - 1`.
> 4. **~={purple}Return Value=~**:
>    - The final value of `right` is the largest value that satisfies the condition defined in `check(mid)`.
>
>**<u>C++ Code</u>**
> ```cpp
> int fn(vector<int​>& arr) {
>     // (1) Define Search Space
>     int left = MINIMUM_POSSIBLE_ANSWER;
>     int right = MAXIMUM_POSSIBLE_ANSWER;
>     
>     // (2) Exit Condition
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>         
>         // (3) Narrow Search Space
>         if (check(mid)) {
>             left = mid + 1; // Condition satisfied, look for larger val
>         } else {
>             right = mid - 1; // Condition not satisfied, look for smaller val
>         }
>     }
>     
>     // (4) Return largest value that satisfies the condition
>     return right;
> }
>
> // Function to evaluate the condition for a given value
> bool check(int x) {
>     // Implement based on problem requirements
>     return BOOLEAN;
> }
> ```
> **~={green}Key Points=~**
> - **~={purple}Time Complexity=~**:
>    - $O(\log(\text{search space size})) \times O(\text{check function complexity})$.
> - **~={purple}Space Complexity=~**:
>    - $O(1)$ for the iterative approach.

# Example Problems

> [!question]- Min Eating Speed (Koko's Bananas)
> <!-- Multiline -->
> **~={red}Question=~**
> * Given a list of `piles` where `piles[i]` represents the number of bananas in the $i^{th}$ pile, and an integer `h` representing the maximum number of hours available, determine the minimum eating speed `k` (bananas per hour) such that all bananas can be eaten within `h` hours.
> 
> **~={red}Solution=~**
> 1. **~={purple}Define Search Space=~**:
>    - `left`: Minimum possible speed, i.e., \(1\) banana per hour.
>    - `right`: Maximum possible speed, i.e., the size of the largest pile.
> 2. **~={purple}Check Function=~**:
>    - For a given speed \(k\), calculate the total hours required to eat all bananas in `piles`.
>    - If the total hours are less than or equal to `h`, the speed \(k\) is valid.
> 3. **~={purple}Binary Search=~**:
>    - If `check(k)` is true, narrow the search to smaller speeds (`right = mid - 1`).
>    - If `check(k)` is false, narrow the search to larger speeds (`left = mid + 1`).
> 4. **~={purple}Return Value=~**:
>    - The final value of `left` represents the minimum speed \(k\) to satisfy the condition.
> 
> **<u>~={green}C++ Code=~</u>**
> 
> **~={blue}Binary Search=~**
> 
> ````col
>```col-md
>**~={purple}Starting Solution Space=~**
>* **~={green}Left=~**: 1 banana per hour
>* ~={red}Right=~: Max num in a pile
>
> ![[Drawing 2025-01-04 22.05.23.excalidraw | center]]
>```
>```col-md
>**~={purple}Reducing Solution Space=~**
>
> ![[Drawing 2025-01-04 22.10.14.excalidraw | center | 250]]
>```
>````
> 
> ```cpp
> int minEatingSpeed(vector<int​>& piles, int h) {
>     int left = 1;
>     int right = 0;
>     for (int bananas : piles) {
>         right = max(right, bananas); // Find the largest pile
>     }
>     
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>         if (check(mid, piles, h)) {
>             right = mid - 1; // Try smaller speeds
>         } else {
>             left = mid + 1; // Try larger speeds
>         }
>     }
>     
>     return left; // Minimum speed to satisfy the condition
> }
>```
>
>**~={blue}Check K Condition=~**
>
> ![[Drawing 2025-01-04 22.16.44.excalidraw | center | 325]]
> 
>```cpp
> bool check(int k, vector<int​>& piles, int h) {
>     long hours = 0;
>     for (int bananas : piles) {
> 	    // Calculate hours required for speed k
> 	    // Since we are rounding, cast with double to avoid
> 	    // losing information
>         hours += ceil((double)bananas / k);
>     }
>     return hours <= h; // Check if total hours fit within limit
> }
> ```
>
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:
>   - Binary Search: $O(\log(\text{max pile size}))$.
>   - Check Function: $O(n)$ per call.
>   - Total: $O(n \cdot \log(\text{max pile size}))$.
> - **~={purple}Space Complexity=~**:
>    - $O(1)$ (constant space).

#flashcards/dsa/patterns/binarysearch/solutionspace
