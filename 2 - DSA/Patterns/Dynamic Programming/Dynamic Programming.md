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

>[!Info]- Top Down Dynamic Programming
><!-- Multiline -->
><u>**Explanation**</u>
>
>Here’s the text extracted so you can copy and paste it:
>1. **~={blue}Function Parameters=~**: Graph in the form of an adjacency list, as well as the starting node
>2. **~={blue}Queue and Set=~**:
>* Queue: Next node to traverse in BFS order
>* Set: Track visited nodes
>
>Add the starting node to both.
>
>3. **~={blue}BFS=~**: While the queue is not empty:
>* We find the front node in the queue, and remove it
>* For every one of its **~={red}unvisited neighbours=~**, mark them as visited (set), and add them to the queue so their neighbours can be explored in breadth-first order.
>
><u>**C++ Code**</u>
>```cpp
>int bfs(unordered_map<int, vector<int​>>& graph, int startNode) {
>	// (1) Queue and Set
>	deque<int​> queue; // To keep track of nodes to visit
>	unordered_set<int​> visited; // To keep track of visited nodes
>	
>	queue.push_back(startNode); // Add the starting node to the queue
>	visited.insert(startNode); // Mark the starting node as visited
>	
>	int result = 0;
>	
>	// (2) BFS Loop
>	while (!queue.empty()) {
>		int node = queue.front(); // Get the current node from the queue
>		queue.pop_front(); // Remove it from the queue
>		
>		// Do Some Logic (example: result += node or processing the node)
>		
>		// Explore all unvisited neighbors
>		for (int neighbor : graph[node]) {
>			// Check if not visited
>			if (visited.find(neighbor) == visited.end()) {
>				visited.insert(neighbor); // Mark neighbor as visited
>				queue.push_back(neighbor); // Add neighbor to the queue
>			}
>		}
>	}
>	return result;
>}
>```

# Example Problems

> [!Question]- Shortest Path in Binary Matrix
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an `n x n` binary matrix `grid`, return the length of the shortest clear path from the top-left cell `(0, 0)` to the bottom-right cell `(n-1, n-1)`.
>* A **clear path**:
>	1. All cells in the path are `0`.
>	2. All adjacent cells in the path are **8-directionally connected** (share an edge or a corner).
>
>* Return `-1` if no such path exists.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2025-01-02 14.52.47.excalidraw | center | 600]]
>
>**~={green}<u>Global Variables</u>=~**
>* `rows` and `cols`: Dimensions of the grid.
>* `directions`: **8-directional moves**:
>	- **{1, 0}**: Move down.
>	- **{-1, 0}**: Move up.
>	- **{0, 1}**: Move right.
>	- **{0, -1}**: Move left.
>	- **Diagonal moves**: {1, 1}, {1, -1}, {-1, 1}, {-1, -1}.
>
>```cpp
>vector<vector<int​>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}, {1, 1}, {1, -1}, {-1, 1}, {-1, -1}};
>```
>
>
>**~={green}<u>BFS Traversal</u>=~**:
>* **Queue**:
>	- Each element is a vector `{row, col, steps}` where:
>		1. `row` and `col` are the current cell coordinates.
>		2. `steps` is the number of steps taken to reach this cell.
>* **Seen**:
>	- A 2D boolean array to track visited cells and avoid revisiting them.
>* **Steps**:
>	1. Initialise the queue with the starting cell `{0, 0, 1}` and mark it as seen.
>	2. While the queue is not empty:
>		- Pop the front element.
>		- Check if it is the bottom-right cell `(n-1, n-1)`. If yes, return `steps`.
>		- For all valid neighboring cells (using `directions`), mark them as seen, update the steps, and push them to the queue.
>	3. If the queue is exhausted and the target is not reached, return `-1`.
>
>```cpp
>int bfs(vector<vector<int​>>& grid, vector<vector<bool​>>& seen, int startRow, int startCol) {
>    if (grid[startRow][startCol] == 1) return -1; // Extra Step
>    
>    deque<vector<int​>> queue;
>    queue.push_back({startRow, startCol, 1});
>    seen[startRow][startCol] = true;
>
>    while (!queue.empty()) {
>        vector<int​> square = queue.front();
>        queue.pop_front();
>
>        int currRow = square[0];
>        int currCol = square[1];
>        int currSteps = square[2];
>
>        if (currRow == this->rows - 1 && currCol == this->cols - 1) {
>            return currSteps;
>        }
>
>        for (vector<int​> direction : this->directions) {
>            int nextRow = currRow + direction[0];
>            int nextCol = currCol + direction[1];
>
>            if (isValid(grid, seen, nextRow, nextCol)) {
>                seen[nextRow][nextCol] = true;
>                queue.push_back({nextRow, nextCol, currSteps + 1});
>            }
>        }
>    }
>    return -1;
>}
>```
>
>**~={green}<u>Helper Function: Valid</u>=~**:
>* Checks whether the next cell is within bounds, not already visited, and contains a `0`.
>```cpp
>bool isValid(vector<vector<int​>>& grid, vector<vector<bool​>>& seen, int nextRow, int nextCol) {
>    return ((0 <= nextRow) && (nextRow < rows)) && 
>           ((0 <= nextCol) && (nextCol < cols)) && 
>           (!seen[nextRow][nextCol]) && 
>           (grid[nextRow][nextCol] == 0);
>}
>```

#flashcards/dsa/patterns/dynamicprogramming
