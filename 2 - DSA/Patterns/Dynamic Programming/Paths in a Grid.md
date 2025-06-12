# Basic Paths in a Grid

## Definition

>[!Note]- What are DP Paths in a Grid Problems?
> <!-- Multiline -->
> Given an `r x c` grid (cells may be open/blocked or weighted) and allowed moves (i.e. only right and down):
> * How many distinct paths are there from the top-left cell `(1,1)` to the bottom-right cell `(r, c)`?
> * How many distinct paths are there from the top-left cell `(1,1)` to the bottom-right cell `(r, c)` if each cell carries a weight?

>[!Info]- How to tackle Path in Grid Problems
> <!-- Multiline -->
> * **~={purple}Step One=~**: Define what `dp[i][j]` is:
> 	* **~={green}Counting=~**: Number of ways to reach cell `i, j`
> 	* **~={green}Minimisation=~**: Minimum cost to reach cell `i, j`
> * ~={purple}Step Two=~: Identify how to transition from smaller subproblems. Then derive base cases.
> 	* **~={green}Counting=~**: `dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`
> 	* **~={green}Minimisation=~**: `dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + value[i][j]`
> * **~={purple}Preprocessing + Base Cases=~**:
> 	* **~={red}Padding=~**: Pad `dp`, `grid`, `cost` matrices with a top row of 0s and a left column of 0s. This helps us handle out of bound errors.

## Sample Problems

>[!Question]- **Counting**: Grid Paths I
> <!-- Multiline -->
> **~={red}Problem=~**: Given an `n x n` grid where each cell is either open (`.`) or blocked (`*`), count the number of distinct paths from the top-left cell `(1, 1)` to the bottom-right cell `(n, n)`, moving only down or right. If the start or end is blocked, the answer is 0.
> 
> **~={purple}Step One=~**: Define `dp[x]`
> `dp[i][j]` = Number of ways to reach `(i, j)`
> 
> **~={purple}Step Two=~**: Recurrence Relation
> * `dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`
> 	* If `(i, j) = '*'`, then `dp[i][j] = 0`
> 
>
> **~={purple}Step Three=~**: Base Case
> * `vector<vector<int​>​> dp(n + 1, vector<int​>(n + 1, 0));`
> * `dp[1][1] = 0`: There is 1 way to reach the first cell
> 
> ```cpp
> // Padded input grid
> vector<vector<char​>​> grid(n + 1, vector<char​>(n + 1, 0));
> 
> // If the first element is blocked, we can't reach the end
> if (grid[1][1] == '*') {
>   cout << 0;
>   return 0;
> }
> 
> // Padded dp table
> vector<vector<int​>> dp(n + 1, vector<int​>(n + 1, 0));
> dp[1][1] = 1;
> for (int i{1}; i <= n; ++i) {
>   for (int j{1}; j <= n; ++j) {
> 	// Ignore the first value
>     if (i == 1 && j == 1) continue;
> 
>     if (grid[i][j] != '*') {
>       dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
>     }
>   }
> }
>```
>
> ![[Drawing 2025-06-08 14.39.11.excalidraw | center | 450]]
>
>**~={blue}Time Complexity=~**: $O(targetSum \cdot numCoins)$
