> [!QUOTE] Quick Notes
> Find optimal value of something, where decisions can impact future decisions

# Overview
## Recipe

>[!Note]- Dynamic Programming Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Dynamic Programming**=~: If the problem asks for:
>* The optimal value, **~={green}max=~** or **~={red}min=~** of something, or the number of ways to do something
>* Where at each step, you need to make a decision, and that decision impacts future decisions
>	* A decision could be picking between two elements
>	* Decisions affecting future decisions could be something like "if you take an element $x$, then you can't take an element $y$ in the future"
>2. **~={purple}Top Down Approach=~**:
>
>> **~={red}Question=~**: You are given an integer array cost where $cost[i]$ is the cost of the $ith$ step on a staircase. Once you pay the cost, you can either climb one or two steps. You can either start from the step with index $0$, or the step with index $1$. Return the minimum cost to reach the top of the floor (outside the array, not the last index of cost).
>
>* **~={blue}A function or DS (bottom-up) that will compute or contain the answer to the problem for any given $state$=~**
>	* **~={red}Define what the function returns=~**: `dp(state)` returns the **~={red}min=~** number of steps required for a given $state$
>	* **~={orange}Define what arguments the function takes (State Variables)=~**: $State = i$, where $i$ is the index along the input
>* **~={blue}A recurrence relation to transition between states=~**:
>	* At each step, we can take 1 or 2 steps. Thus the min cost to reach step $i$ is:
>		* `dp(i) = min(dp(i - 1) + cost[i - 1], dp(i - 2) + cost[i - 2])`
>* **~={blue}Base cases=~**:
>	* We can start at step 0 or 1, hence:
>		* if (i == 0 || i == 1) return 0;
>
>This results in the following code:
>
>```cpp
>int minCost(vector<int​> cost) {
>	// Cost to reach 1 step outside the array
>	return dp(cost.size());
>}
>
>int dp(int i) {
>	if (i == 0 || i == 1) return 0;
>	
>	int cost = min(dp(i - 1) + cost[i - 1], dp(i - 2) + cost[i - 2]);
>	return cost;
>}
>```
>1. **~={purple}Convert Recursion to Dynamic Programming=~**:
>* Have `memo` as a global variable
>* Have `memo[i]` store the value from the recursive relation, storing the max value at state $i$
>
>```cpp
>int minCost(vector<int​> cost) {
>	// Cost to reach 1 step outside the array, hence + 1
>	memo = vector(cost.size() + 1, -1); 
>	return dp(cost.size());
>}
>
>int dp(int i) {
>	if (i == 0 || i == 1) return 0;
>	if (memo[i] != -1) return memo[i];
>	
>	memo[i] = min(dp(i - 1) + cost[i - 1], dp(i - 2) + cost[i - 2]);
>	return memo[i];
>}
>```
>2. **~={purple}Convert Top Down to Bottom Up=~**:
>* **~={red}Memo=~**: Keep the `memo` vector. This is sized accordingly to our state variables.
>* **~={red}Base Case=~**: Set the base case in the `memo` vector accordingly. Here, `memo[0] = 0` and `memo[1] = 0`
>* **~={red}For Loop=~**: Write a for-loop(s) that iterate over your state variables. If you have multiple state variables, you will need nested for-loops. These loops should start iterating from the base cases and end at the answer state.
>	* Each iteration of the inner-most for loop represents a given state, and is equivalent to a function call. Change all `dp()` calls to `memo[]` accesses
>
>```cpp
>int minCost(vector<int​> cost) {
>	int n = cost.size();
>	vector<int​> memo(n + 1);
>	
>	// Base Cases
>	memo[0] = 0;
>	memo[1] = 0;
>
>	// Iterate from one after the base case
>	for (int i{2}; i < n + 1; ++i) {
>		memo[i] = min(memo[i - 1] + cost[i - 1], memo[i - 2] + cost[i - 2]);
>	}
>	return dp[n];
>}
>```

## Time Complexity

>[!Note]- Time Complexity of Dynamic Programming
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: We calculate each state only once, hence if we have $N$ states, and it takes $F$ complexity of work at each state, $O(NF)$
> * **~={purple}Space Complexity=~**: Our `memo` array will store all the states. Hence $O(n)$

## Points of Interest

>[!Note]- What is a state, and what are some common states?
> <!-- Multiline -->
> With dynamic programming, `dp(state)`, `state` is a set of variables that describe a "scenario", where `dp(state)` is the optimal value at that state.
> 
> **~={blue}Common States Include=~**:
> * **~={green}Index Along Array=~**: Tracks position in a sequence (e.g., `dp[i]` for the first `i` elements). 
> 	* **~={red}_Example_=~**: Maximum sum subarray ending at index `i`.
> * **~={green}Second Index Along Array=~**: Tracks relationships between two positions. 
> 	* **~={red}_Example_=~**: `dp[i][j]` for substrings between indices `i` and `j`.
> * **~={green}Explicit Numerical Constraints=~**: Tracks numerical conditions. 
> 	* **~={red}_Example_=~**: `dp[i][sum]` for subsets of the first `i` numbers forming a specific sum.
> * **~={green}Boolean Status=~**: Tracks a yes/no condition. 
> 	* **~={red}_Example_=~**: `dp[i][used]` where `used` indicates if an element has been chosen.

>[!Note]- Top Down vs Bottom Up
> <!-- Multiline -->
> * **~={red}Top Down=~**: Build the tree from top to bottom.
> * **~={green}Bottom Up=~**: Build the tree from bottom to top. Has less overhead than recursion. However a language that implements tail recursion won't benefit too much.

# Templates

>[!Info]- Bottom Up DP: Single For Loop
><!-- Multiline -->
><u>**~={purple}Characteristics=~**</u>
>* **~={blue}State Dependency=~**: Each state depends **only on prior calculated values**, often stored in the `dp` array.
>
>**~={purple}<u>Code Example (Fibonacci)</u>=~**
>* **~={red}State=~**: $i$ will represent the $ith$ fibonacci number, where $i=0...n$
>* **~={red}DP Array Size=~**: 
>	* `vector<int​> dp(n + 1, 0)`: The `nth` fibonacci number is at index `n`, hence the the size of our array needs to be `n + 1`
>* **~={red}Base Case=~**:
>	* **~={green}Explanation=~**: This allows us to start our For Loop at $i=2$
>	* `dp[0] = 0`: First fibonacci number is 0
>	* `dp[1] = 1`: Second fibonacci number is 1
>* **~={red}Recursive Relation=~**:
>	* **~={green}Explanation=~**: Sum of the previous, and second previous values
>	* `dp[i] = dp[i - 1] + dp[i - 2]`
>```cpp
>int fib(int n) {
>	if (n <= 1) {
>		return n;
>	}
>	vector<int​> dp(n + 1, 0);
>		
>	// Base Case
>	dp[0] = 0;
>	dp[1] = 1;
>	
>	// Recursive Relation
>	// * Start right after base case, i = 2
>	for (int i{2}; i < n + 1; ++i) {
>		dp[i] = dp[i - 1] + dp[i - 2];
>	}
>	
>	return dp[n];
>}
>
>```

>[!Info]- Bottom Up DP: Nested For Loop (Direct Prior State)
><!-- Multiline -->
><u>**~={purple}Characteristics=~**</u>
>* **~={blue}State Dependency=~**: Each state `dp[i]` depends on **all prior states** (`dp[j]` for `j < i`). (SUBSEQUENCE NOT NECESSARILY CONTINOUS)
>
>**~={purple}<u>Code Example (Longest Increasing Subsequence)</u>=~**
>* **~={red}State=~**: $dp[i]$ represents the **length of the LIS** ending at index $i$.
>* **~={red}DP Array Size=~**: 
>	* `vector<int> dp(n, 1)`: The size of the DP array matches the size of the input array `nums`. Initialise to 1 because each element can be its own subsequence.
>* **~={red}Base Case=~**:
>	* **~={green}Explanation=~**: This allows us to start our **~={red}Outer=~** For Loop at $i=1$
>	* `dp[0] = 1`: The LIS at index 0, is 1, as itself is the only subsequence
>* **~={red}Recursive Relation=~**:
>	* **~={green}Explanation=~**: If `nums[i] > nums[j]`, we can extend the LIS ending at `j` to include `nums[i]` (hence +1)
>	* `dp[i] = max(dp[i], dp[j] + 1)`
>```cpp
>int list(vector<int​> nums) {
>	int n = nums.size();
>	vector<int​> dp(n, 1);
>	
>	// Recursive Relation
>	// For every state
>	// * Start right after base case at i = 1
>	for (int i{1}; i < n; ++i) {
>		// Look at all prior states
>		for (int j{0}; j < i; ++j) {
>			if (nums[j] < nums[i]) {
>				dp[i] = max(dp[i], dp[j] + 1);
>			}
>		}
>	}
>	
>	return *max_element(dp.begin(), dp.end());
>}
>```
> ![[Drawing 2025-01-19 11.31.47.excalidraw | center | 700]]

>[!Info]- Bottom-Up DP: Nested For Loop (Prior State from Subtraction)
><!-- Multiline -->
><u>**~={purple}Characteristics=~**</u>
>* **~={blue}State Dependency=~**: Each state `dp[i]` depends on **specific prior states** (`dp[i - score]` for all `score` in the scoring options).
>
>**~={purple}<u>Code Example (Scoring Options)</u>=~**
>* **~={red}State=~**: $dp[i]$ represents the **number of ways to achieve a score of $i$**.
>* **~={red}DP Array Size=~**:
>	* `vector<long> dp(n + 1, 0)`: The size of the DP array is $n + 1$ to include all scores from $0$ to $n$.
>* **~={red}Base Case=~**:
>	* **~={green}Explanation=~**: There is exactly **one way** to score $0$ (by taking no scoring moves).
>	* `dp[0] = 1`.
>* **~={red}Recursive Relation=~**:
>	* **~={green}Explanation=~**: For each scoring option, if the current score `i` is greater than or equal to the scoring option:
>		* Add the number of ways to achieve the remaining score ($i - \text{score}$).
>	* `dp[i] += dp[i - \text{score}]`.
>```cpp
>long ScoringOptions(int n) {
>    // (1) Initialise DP Array
>    // Each index represents ways to achieve that score
>    vector<long​> dp(n + 1, 0);
>    vector<int​> scoreOptions{1, 2, 4}; // Available scoring options
>
>    // (2) Base Case
>    dp[0] = 1; // One way to achieve a score of 0
>
>    // (3) Fill DP Array
>    // For every state
>    for (int i{0}; i <= n; ++i) {
> 	   // For every possible score before the state
>        for (int score : scoreOptions) {
>            if (i - score >= 0) {
>                dp[i] += dp[i - score]; // Recursive Relation
>            }
>        }
>    }
>    return dp[n]; // Return the number of ways to achieve score n
>}
>```
>
>**~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**: 
>   - Outer Loop: $O(n)$ for all scores from $0$ to $n$.
>   - Inner Loop: $O(\text{scoreOptions.size()})$ for each scoring option.
>   - Total: $O(n \times \text{scoreOptions.size()})$.
> - **~={purple}Space Complexity=~**:
>   - $O(n)$ for the DP array.

>[!Info]- Bottom-Up DP: Nested For Loop with Additional Variable
><!-- Multiline -->
><u>**~={purple}Characteristics=~**</u>
>* **~={blue}State Dependency=~**: Each state `dp[i]` depends on ~={blue}**specific**=~ prior states (`dp[i - coin]` for every `coin` in the list). (Not all prior states)
>
>**~={purple}<u>Code Example (Coin Change)</u>=~**
>* **~={red}State=~**: $dp[i]$ represents the **minimum number of coins** needed to make up the amount $i$.
>* **~={red}DP Array Size=~**:
>	* `vector<int> dp(amount + 1, INT_MAX)`: The DP array has a size of $amount + 1$ since we consider all amounts from $0$ to $amount$.
>	* Initialise with `INT_MAX` to represent an **impossible state** (no coins available for that amount).
>* **~={red}Base Case=~**:
>	* **~={green}Explanation=~**: To make up amount $0$, no coins are needed.
>	* `dp[0] = 0`: $0$ can be achieved with $0$ coins.
>* **~={red}Recursive Relation=~**:
>	* **~={green}Explanation=~**: For each coin in `coins`, if `i >= coin`, we can reduce the problem to `dp[i - coin] + 1` (use one more coin to cover the remaining amount).
>	* `dp[i] = min(dp[i], dp[i - coin] + 1)`
>```cpp
>class Solution {
>public:
>    int coinChange(vector<int​>& coins, int amount) {
>        // (1) DP Array: Represents the minimum coins needed for each amount
>        vector<int​> dp(amount + 1, INT_MAX);
>        dp[0] = 0; // Base Case: $0$ amount requires $0$ coins
>        
>		// (2) Recursive Relation
>		// For every state
>		// * Start right after base case at i = 1
>        for (int i{1}; i <= amount; ++i) {
> 	       // For every coin that could be chosen
>            for (int coin : coins) {
>                // If the coin value is less than or 
>                // equal to the current amount (Prevent Overflow)
>                if (i >= coin && dp[i - coin] != INT_MAX) {
> 	               // If the current state is `coin` amount away
> 	               // we just add 1 to get to the target amount
>                    dp[i] = min(dp[i], dp[i - coin] + 1);
>                }
>            }
>        }
>
>        // (3) Return the result for the target amount
>        return dp[amount] == INT_MAX ? -1 : dp[amount];
>    }
>};
>```
>
>**~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**: 
>   - Outer Loop: $O(\text{amount})$ for all amounts.
>   - Inner Loop: $O(\text{coins.size()})$ for each coin.
>   - Total: $O(\text{amount} \times \text{coins.size()})$.
> - **~={purple}Space Complexity=~**:
>   - $O(\text{amount})$ for the DP array.

# Example Problems

> [!Question]- Fundamental Linear DP: House Robber Problem
> <!-- Multiline -->
> **~={red}Question=~**:
> * You are a robber trying to maximize the amount of money you can steal from a row of houses.
> * Each house has a certain amount of money stored, represented by `nums[i]`.
> * You **cannot rob two adjacent houses**.
> * Return the maximum amount of money you can rob without triggering alarms.
>
> **~={red}Solution=~**:
> 1. **~={purple}Define State=~**:
>    - `dp(i)`: The maximum money that can be robbed from house `0` to house `i`.
> 2. **~={purple}Base Cases=~**:
>    - `dp(0)`: If there's only one house, rob it.  
>      ```cpp
>      dp(0) = nums[0];
>      ```
>    - `dp(1)`: If there are two houses, rob the one with more money.  
>      ```cpp
>      dp(1) = max(nums[0], nums[1]);
>      ```
> 3. **~={purple}Recursive Formula=~**:
>    - For house `i`, we have two choices:
>      - Rob house `i`: Add `nums[i]` to `dp(i-2)` (skipping one house).
>      - Skip house `i`: Take the result from `dp(i-1)`.
>    - Transition Formula:  
>      ```cpp
>      dp(i) = max(dp(i - 1), dp(i - 2) + nums[i]);
>      ```
> 4. **~={purple}Memoization=~**:
>    - Store results of previously solved subproblems in a `memo` array to avoid redundant calculations.
>
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> class Solution {
> private:
>     vector<int​> nums; // Store the input array globally
>     vector<int​> memo; // Memoization array to store intermediate results
>
> public:
>     int rob(vector<int​>& nums) {
>         this->nums = nums;
>         this->memo = vector(nums.size(), -1);
>         return dp(nums.size() - 1); // Start from the last house
>     }
>
>     int dp(int i) {
>         // (1) Base Cases
>         if (i == 0) return nums[0];
>         if (i == 1) return max(nums[0], nums[1]);
>
>         // (2) Check Memoization
>         if (this->memo[i] != -1) return memo[i];
>
>         // (3) Recursive Case
>         int rob = dp(i - 2) + this->nums[i];  // Rob house i
>         int dontRob = dp(i - 1);              // Skip house i
>
>         // Store the result in memo
>         this->memo[i] = max(rob, dontRob);
>         return this->memo[i];
>     }
> };
> ```
>
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:
>   - $O(n)$: Each house's result is computed once, and memoization avoids redundant calculations.
> - **~={purple}Space Complexity=~**:
>   - $O(n)$: For the `memo` array and the recursive call stack.

> [!Info]- House Robber Problem Addendum (Circular Array)
> <!-- Multiline -->
> **~={red}Handling Circular Arrays=~**:
> 
> ![[Drawing 2025-01-18 21.38.51.excalidraw | center | 500]]
> 
> * If the houses are arranged in a **circular manner**, robbing the first house prevents robbing the last house and vice versa.
> * To address this:
>   1. Divide the array into **two subproblems**:
>      - **Subproblem 1**: Consider all houses **except the last one** (`nums[0]` to `nums[n-2]`).
>      - **Subproblem 2**: Consider all houses **except the first one** (`nums[1]` to `nums[n-1]`).
>   2. Solve each subproblem using the original **House Robber logic**.
>   3. Return the **maximum of the two results**.
>
> **~={purple}Key Adjustments in the Code=~**:
> 1. **Split the Input Array**:
>    ```cpp
>    vector<int​> numsFirst = {nums.begin(), nums.begin() + nums.size() - 1};
>    vector<int​> numsSecond = {nums.begin() + 1, nums.end()};
>    ```
> 2. **Compute Results for Both Cases**:
>    ```cpp
>    int first = dp(numsFirst.size() - 1, numsFirst, memoFirst);
>    int second = dp(numsSecond.size() - 1, numsSecond, memoSecond);
>    return max(first, second);
>    ```

> [!Question]- Fundamental Linear DP (2 For Loops): Longest Increasing Subsequence
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an integer array `nums`, return the length of the **longest strictly increasing subsequence**.
> * A subsequence is a sequence derived from `nums` by deleting some or no elements without changing the order of the remaining elements.
>
> **~={red}Solution=~**:
> 1. **~={purple}Define State=~**:
>    - `memo[i]`: Length of the longest increasing subsequence **ending at index `i`**.
> 2. **~={purple}Base Case=~**:
>    - Initially, every element is a subsequence of length 1.
>      ```cpp
>      memo[i] = 1; // Each element forms an LIS of at least length 1
>      ```
> 3. **~={purple}Recursive Formula=~**:
>    - For each `i`, iterate over all `j` where `0 <= j < i`:
>      - If `nums[i] > nums[j]`, the subsequence can be extended:
>        ```cpp
>        memo[i] = max(memo[i], memo[j] + 1);
>        ```
> 4. **~={purple}Final Answer=~**:
>    - The length of the LIS is the **maximum value in the `memo` array**:
>      ```cpp
>      return *max_element(memo.begin(), memo.end());
>      ```
>
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> class Solution {
> public:
>     int lengthOfLIS(vector<int​>& nums) {
>         // (1) Initialise the memoization array
>         vector<int​> memo(nums.size(), 1); 
>         // Each element forms its own subsequence
>
>         // (2) Fill memo array
>         for (int i{0}; i < nums.size(); ++i) {
>             for (int j{0}; j < i; ++j) {
>                 if (nums[i] > nums[j]) {
>                     memo[i] = max(memo[i], memo[j] + 1);
>                 }
>             }
>         }
>
>         // (3) Find the maximum length of LIS
>         return *max_element(memo.begin(), memo.end());
>     }
> };
> ```
>
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:
>   - $O(n^2)$: Double loop for `i` and `j`, iterating over the array.
> - **~={purple}Space Complexity=~**:
>   - $O(n)$: Space for the `memo` array.

#flashcards/dsa/patterns/dynamicprogramming
