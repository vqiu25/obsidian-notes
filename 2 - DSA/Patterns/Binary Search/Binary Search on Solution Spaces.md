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
>    - The final value of `left` is the smallest value that satisfies the condition defined in `check(mid)`. If nothing isValid, then it will return `right + 1`.
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

> [!Info]- Binary Search on Solution Space (Maximising Min and Minimising Max for K Chunks)
> <!-- Multiline -->
> **<u>Explanation</u>**
> **<u>C++ Code (Maximising the Minimum Subarray Sum with K Subarrays)</u>**
> ```cpp
> int maximizeMin(vector<int​>& arr, int k) {
>     // Smallest possible value for the minimum (Defined by Question)
>     int left = 1;
>     // Largest possible value for the minimum (Expected Number)
>     int right = accumulate(arr.begin(), arr.end(), 0) / k;
>
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>
>         if (isValid(arr, k, mid)) {
>             left = mid + 1; // Try larger minimum values
>         } else {
>             right = mid - 1; // Try smaller minimum values
>         }
>     }
>
>     return right; // The largest valid minimum
> }
>
> bool isValid(vector<int​>& arr, int k, int mid) {
>     int chunks = 0, currSum = 0;
>
>     for (int num : arr) {
>         currSum += num;
>         if (currSum >= mid) {
>             chunks++;
>             currSum = 0;
>         }
>     }
>
>     // Check if we can divide into at least k chunks
>     return chunks >= k;
> }
> ```
> 
> **<u>C++ Code (Minimising the Maximum Subarray Sum with K Subarrays)</u>**
> ```cpp
> int minimizeMax(vector<int​>& arr, int k) {
> 	// Smallest possible value for the maximum
>     int left = *max_element(arr.begin(), arr.end());
>     // Largest possible value for the maximum
>     int right = accumulate(arr.begin(), arr.end(), 0);
>
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>
>         if (isValid(arr, k, mid)) {
>             right = mid - 1; // Try smaller maximum values
>         } else {
>             left = mid + 1; // Try larger maximum values
>         }
>     }
>
>     return left; // The smallest valid maximum
> }
>
> bool isValid(vector<int​>& arr, int k, int mid) {
>     int chunks = 1, currSum = 0;
>
>     for (int num : arr) {
> 	    currSum += num;
>         if (currSum > mid) {
>             chunks++;
>             currSum = num;
>         }
>     }
>	// Check if we can divide into at most k chunks
>     return chunks <= k;
> }
> ```

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

> [!question]- DFS with Binary Search (Minimum Effort Path)
> <!-- Multiline -->
> **~={red}Question=~**
> * Given a grid `heights`, where `heights[i][j]` represents the height of the cell at `(i, j)`, find the minimum effort required to travel from the top-left cell `(0, 0)` to the bottom-right cell `(rows-1, cols-1)`.
> * The effort of a path is defined as the maximum absolute difference in height between two consecutive cells in the path.
>
> **~={red}Solution=~**
> 1. **~={purple}Define Search Space=~**:
>    - `left`: The minimum possible effort (0).
>    - `right`: The maximum possible effort, which is the largest height in the grid.
> 2. **~={purple}Binary Search on Effort=~**:
>    - At each midpoint (`mid`), check if it is possible to reach the destination with a maximum effort of `mid`.
>    - Use a Depth-First Search (DFS) to validate the condition.
> 3. **~={purple}Greedy Narrowing of Search Space=~**:
>    - If the condition is satisfied for `mid`, try smaller efforts (`right = mid - 1`).
>    - Otherwise, try larger efforts (`left = mid + 1`).
> 4. **~={purple}Return Value=~**:
>    - The value of `left` after the binary search ends is the minimum effort required.
>
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> int minimumEffortPath(vector<vector<int​>>& heights) {
>     int left{0};
>     int right{0};
>     int rows = heights.size();
>     int cols = heights[0].size();
>
>     // Determine the largest value in the grid for the search space
>     for (int i{0}; i < rows; ++i) {
>         for (int j{0}; j < cols; ++j) {
>             right = max(right, heights[i][j]);
>         }
>     }
>
>     // Binary search on the effort
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>         if (isValidEffort(heights, mid)) {
>             right = mid - 1; // Try smaller efforts
>         } else {
>             left = mid + 1; // Try larger efforts
>         }
>     }
>     return left; // Minimum effort required
> }
>
> bool isValidEffort(vector<vector<int​>>& heights, int minEffort) {
>     int rows = heights.size();
>     int cols = heights[0].size();
>     deque<pair<int, int>> stack;
>     vector<vector<bool​>> visited(rows, vector<bool​>(cols, false));
>
>     stack.push_back({0, 0});
>     visited[0][0] = true;
>
>     while (!stack.empty()) {
>         pair<int, int> node = stack.back();
>         stack.pop_back();
>
>         int currRow = node.first;
>         int currCol = node.second;
>
>         // If we reached the destination
>         if (currRow == rows - 1 && currCol == cols - 1) return true;
>
>         for (const auto& direction : directions) {
>             int nextRow = currRow + direction[0];
>             int nextCol = currCol + direction[1];
>             
>             // Check validity and compute effort
>             if (isValidPosition(heights, visited, nextRow, nextCol)) {
>                 int effort = abs(heights[nextRow][nextCol] - heights[currRow][currCol]);
>                 if (effort <= minEffort) {
>                     stack.push_back({nextRow, nextCol});
>                     visited[nextRow][nextCol] = true;
>                 }
>             }
>         }
>     }
>
>     return false;
> }
>
> bool isValidPosition(vector<vector<int​>>& heights, vector<vector<bool​>>& visited, int nextRow, int nextCol) {
>     int rows = heights.size();
>     int cols = heights[0].size();
>     return (0 <= nextRow) && (nextRow < rows) &&
>            (0 <= nextCol) && (nextCol < cols) &&
>            (!visited[nextRow][nextCol]);
> }
> ```
>
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:
>   - Binary Search: $O(\log(\text{max height}))$.
>   - DFS per check: $O(rows \cdot cols)$.
>   - Total: $O((rows \cdot cols) \cdot \log(\text{max height}))$.
> - **~={purple}Space Complexity=~**:
>    - $O(rows \cdot cols)$ for the visited matrix and stack.

> [!question]- Maximise Min (Divide Chocolate to Maximise Sweetness)
> <!-- Multiline -->
> **~={red}Question=~**
> * You are given an array `sweetness` where `sweetness[i]` represents the sweetness level of the $i^{th}$ piece of chocolate.
> * Divide the chocolate into \(k + 1\) pieces such that the minimum sweetness of the piece with the least sweetness is maximised.
> * Return _the maximum possible sweetness of the least sweet piece._
>
> **~={red}Solution=~**
> 1. **~={purple}Define Search Space=~**:
>    - `left`: Minimum possible sweetness for the least sweet piece (\(1\)).
>    - `right`: Maximum possible sweetness, which is the total sweetness divided by \(k + 1\).
> 2. **~={purple}Binary Search on Sweetness=~**:
>    - Use binary search to find the maximum possible minimum sweetness (\(mid\)).
>    - If it is possible to divide the chocolate into \(k + 1\) pieces with at least \(mid\) sweetness, increase the lower bound (`left = mid + 1`).
>    - Otherwise, decrease the upper bound (`right = mid - 1`).
> 3. **~={purple}Validation=~**:
>    - Use the `isValid` function to determine if it is possible to divide the chocolate into \(k + 1\) pieces, each with at least \(mid\) sweetness.
> 4. **~={purple}Return Value=~**:
>    - The final value of `right` is the maximum possible minimum sweetness.
>
> **<u>~={green}C++ Code=~</u>**
> **~={blue}Search Space=~**
> 
>  ![[Drawing 2025-01-05 22.52.51.excalidraw | center | 700]]
>  
> ```cpp
> int maximizeSweetness(vector<int​>& sweetness, int k) {
>     // Define search space
>     int left{1};
>     int right = accumulate(sweetness.begin(), sweetness.end(), 0) / (k + 1);
>
>     // Binary search on sweetness
>     while (left <= right) {
>         int mid = left + (right - left) / 2;
>         if (isValid(sweetness, k, mid)) {
>             left = mid + 1; // Try larger sweetness
>         } else {
>             right = mid - 1; // Try smaller sweetness
>         }
>     }
>     return right; // Maximum possible minimum sweetness
> }
> ```
> **~={blue}Validation=~**
> * Every time we test for a new max, min sweetness, we identify how many chunks we can break the chocolate into. If it exceeds $k+1$, then it is valid, as we can split the chocolate among ourselves and our friends.
> * In other words, we're trying to make sure if each subarray / chunk is at least of $mid$ sweetness
> 
> ```cpp
> bool isValid(vector<int​>& sweetness, int k, int mid) {
>     int chunks{0};
>     int currSum{0};
>
>     // Check if it's possible to divide into k + 1 valid chunks
>     for (int i{0}; i < sweetness.size(); ++i) {
>         currSum += sweetness[i];
>         if (currSum >= mid) {
>             chunks++;
>             currSum = 0; // Reset current chunk
>         }
>     }
>     return chunks >= k + 1;
> }
> ```
>
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:
>   - Binary Search: $O(\log(\text{total sweetness}))$.
>   - Validation (isValid): $O(n)$ per call, where \(n\) is the size of the `sweetness` array.
>   - Total: $O(n \cdot \log(\text{total sweetness}))$.
> - **~={purple}Space Complexity=~**:
>    - $O(1)$ (constant space).

#flashcards/dsa/patterns/binarysearch/solutionspace
