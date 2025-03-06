# 0/1 Knapsack Problems

>[!Note]- What is the 0/1 Knapsack Problem?
> <!-- Multiline -->
>* Involves selecting items with given weights and values to maximise the total value without exceeding a weight capacity
>* There are $(n + 1) \cdot (c + 1)$ combinations:
>	* For every capacity from $0$ to $c$
>	* We can choose a subset of items, of which there are $0$ to $n$ subsets
>
>![[Pasted image 20250122204814.png | center | 450]]

>[!Info]- Bottom-Up DP: 0/1 Knapsack Problem
><!-- Multiline -->
><u>**~={purple}Characteristics=~**</u>
>* **~={blue}State Dependency=~**: Each state `dp[i][j]` depends on the state of the previous row:
>   - `dp[i-1][j]`: Value without including the current item.
>   - `dp[i-1][j - weights[i-1]]`: Value after including the current item.
>
>**~={purple}<u>Code Example (Knapsack Problem)</u>=~**
>* **~={red}State=~**: $dp[i][j]$ represents the **maximum value** achievable using the first `i` items with a knapsack capacity of `j`.
>* **~={red}DP Table Size=~**:
>	* `vector<vector<int​>> dp(n + 1, capacity + 1)`: A 2D table of size `(n + 1) x (capacity + 1)`.
>* **~={red}Base Case=~**:
>	* **~={green}Explanation=~**: If there are no items or the capacity is `0`, the maximum value is `0`. Hence row 0 and col 0 are all set to 0.
>	* `dp[0][j] = 0` and `dp[i][0] = 0`.
>* **~={red}Recursive Relation=~**:
>	* **~={green}Explanation=~**:
>		* If the weight of the current item (`weights[i-1]`) is less than or equal to the current capacity `j`, choose the maximum of:
>
>			1. Value obtained by including the item: `values[i-1] + dp[i-1][j - weights[i-1]]`.
>			2. Value without including the item: `dp[i-1][j]`.
>
>		* Otherwise, exclude the item: `dp[i][j] = dp[i-1][j]`.
>
>```cpp
>int FindKnapsack(int capacity, vector<int​>& weights, vector<int​>& values, int n) {
>    // Create DP Table
>    vector<vector<int​>​> dp(n + 1, vector<int​>(capacity + 1, 0));
>    int rowSubsets = dp.size() + 1;
>    int colCapacities = dp[0].size() + 1;
>    
>    // Fill DP Table
>    for (int i = 0; i < rowSubsets; ++i) {
>        for (int j = 0; j < colCapacities; ++j) {
>            // Base Case
>            if (i == 0 || j == 0)
>                dp[i][j] = 0;
>            // Recursive Relation
>            // 1. If the weight of the item we want to add to the 
>            // knapsack is not greater than the capacity of the
>            // knapsack
>            else if (weights[i - 1] <= j)
> 	           // 2. If we include the item:
> 	           // * Case 1: Add the value of the item with the {current
> 	           // max value, without the current item (which is one
> 	           // row above, and at the column/capacity which 
> 	           // excludes the current weight)} OR
> 	           // Case 2: We exclude the item, thus taking the 
> 	           // value directly 1 row above. (Excluding the
> 	           // item could be a better choice)
> 	           
>                dp[i][j] = max(values[i - 1] + dp[i - 1][j - weights[i - 1]], 
> 			             dp[i - 1][j]);
>            // Exclude the item
>            else
>                dp[i][j] = dp[i - 1][j];
>        }
>    }
>    return dp[n][capacity];
>}
>```
>
>**~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**: 
>   - $O(n \times \text{capacity})$: Double loop iterating over items and capacities.
> - **~={purple}Space Complexity=~**: 
>   - $O(n \times \text{capacity})$: Space for the 2D DP table.

**~={purple}If we include the item=~**:
- **~={green}Case 1=~**: Include the item by adding its value to the maximum value obtainable from the remaining capacity (from the previous row, at the column corresponding to the current capacity minus the item's weight).
- **~={red}Case 2=~**: Exclude the item entirely, taking the value from the previous row at the same capacity. Excluding the item might be the better choice if it leads to a higher value overall.

# Unbounded Knapsack

# General Matrix

# Longest Common Subsequence

> [!Info]- Bottom-Up DP: Longest Common Subsequence
> 
> **~={purple}<u>Characteristics</u>=~**
> 
> - **State Dependency**: Each state `dp[i][j]` depends on:
>     - `dp[i-1][j-1]`: If the current characters match, extend the subsequence.
>     - `dp[i-1][j]` or `dp[i][j-1]`: If characters do not match, take the maximum of either excluding the character from `text1` or `text2`.
> 
> ~={purple}<u>Code Example (Longest Common Subsequence)</u>=~
> 
> - **~={green}State=~**: `dp[i][j]` represents the **length of the longest common subsequence (LCS)** between `text1[0:i-1]` and `text2[0:j-1]`.
> - **~={green}DP Table Size=~**:
>     - `vector<vector<int>> dp(rows, cols)`: A 2D table where `rows = text1.size() + 1` and `cols = text2.size() + 1`.
> - ~={red}**Base Case**=~:
>     - Explanation: An empty string compared to any string has an LCS length of `0`. Row 0 and Column 0 are entirely 0's.
>     - `dp[0][j] = 0` and `dp[i][0] = 0`.
> - **~={red}Recursive Relation=~**:
>     - Explanation:
>         1. If `text1[i-1] == text2[j-1]`: Extend the subsequence by `+1`:
>             - `dp[i][j] = dp[i-1][j-1] + 1`.
>         2. Otherwise: Take the maximum subsequence length by excluding a character from `text1` or `text2`:
>             - `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
> 
> ```cpp
> int longestCommonSubsequence(string text1, string text2) {  
>     int rows = text1.size() + 1;  
>     int cols = text2.size() + 1;  
>     vector<vector<int​>> dp(rows, vector<int​>(cols, 0));  
>   
>     for (int i{1}; i < rows; ++i) {  
>         for (int j{1}; j < cols; ++j) {  
> 	        // One back on both of the strings + 1
>             if (text1[i - 1] == text2[j - 1]) {  
>                 dp[i][j] = dp[i - 1][j - 1] + 1; // Matching characters  
>             } else {
> 		        // One back on text 1 vs One back on text 2  
>                 dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]); // Non-matching  
>             }  
>         }  
>     }  
>   
>     return dp[rows - 1][cols - 1]; // Final LCS length  
> }
> ```
> 
> **~={purple}<u>Visual</u>=~**
>  ![[Drawing 2025-01-25 14.48.21.excalidraw | center | 700]]
> 
> ~={purple}<u>Key Points</u>=~:
> 
> - **~={green}Time Complexity=~**:
>     - `O(n × m)`: Nested loop iterating over both strings.
> - ~={green}**Space Complexity**=~:
>     - `O(n × m)`: Space for the 2D DP table.

#flashcards/dsa/patterns/multidimensionaldynamicprogramming
