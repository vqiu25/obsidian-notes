> [!QUOTE] Quick Notes
> Traverse a graph to find the shortest path

# Overview
## Recipe

>[!Note]- Graph BFS Recipe
> <!-- Multiline -->
>1. ~={purple}**Why DFS on Graphs**=~:

## Time Complexity

>[!Note]- Time Complexity of BFS on Graph
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: The time complexity is $O(n+e)$, where $n$ is the number of nodes and $e$ is the number of edges in the graph. Every node and every edge is visited once during the traversal.
> 	* However, for dense graphs, $e=n(n-1)/2$, as every node, is connected to every other node, i.e. for every n nodes, they have n-1 neighbours, all with edges, resulting in $O(n^2)$. An adjacency matrix will always be $O(n^2)$ though.
> * **~={purple}Space Complexity=~**: $O(n+e)$ for storing nodes and edges

## Points of Interest

>[!Note]- BFS visiting node property
> <!-- Multiline -->
> * **~={blue}Min Steps=~**: BFS on a graph always visits nodes according to their distance from the **starting point**. This is the key idea behind BFS on graphs - **every time you visit a node**, you must have reached it in the minimum steps possible from wherever you started your BFS.
> * **~={blue}Level By Level=~**: Moving to another level, is like increasing the distance of 1 from the origin

> [!Note]- Convert BT to Adjacency List
> <!-- Multiline -->
> **~={red}Question=~**:
>* Convert a binary tree into an adjacency list representation for graph-based algorithms.
>
>**~={red}Solution=~**:
>* **~={purple}Steps=~**:
>	1. Traverse the binary tree using DFS to generate an edge list.
>	2. Convert the edge list into an adjacency list to enable bidirectional traversal.
>
>**~={green}<u>DFS for Edge List</u>=~**:
>* Traverse the binary tree and record edges between each node and its children.
>```cpp
>void dfs(TreeNode* root, vector<vector<int​>>& edges) {
>    if (root == nullptr) return;
>    dfs(root->left, edges);
>    dfs(root->right, edges);
>
>    if (root->left != nullptr) {
>        edges.push_back({root->val, root->left->val});
>    }
>    if (root->right != nullptr) {
>        edges.push_back({root->val, root->right->val});
>    }
>}
>```
>
>**~={green}<u>Convert to Adjacency List</u>=~**:
>* Build a bidirectional adjacency list from the edge list.
>```cpp
>unordered_map<int, vector<int​>> adjList;
>for (vector<int​> edge : edges) {
>    adjList[edge[0]].push_back(edge[1]);
>    adjList[edge[1]].push_back(edge[0]);
>}
>```

> [!Note]- Return List of Nodes at Level `K` in Adjacency List
> <!-- Multiline -->
> **~={red}Question=~**:
>* Using an adjacency list, find all nodes that are exactly `K` edges away from a given starting node.
>
>**~={red}Solution=~**:
>* **~={purple}Steps=~**:
>	1. Perform BFS starting from the given node.
>	2. Traverse the graph level by level, tracking visited nodes to avoid cycles.
>	3. Stop BFS after `K` levels and collect the nodes at this level.
>
>**~={green}<u>BFS for Nodes at Distance `K`</u>=~**:
>* Use a queue for level-by-level traversal and a set to track visited nodes.
>```cpp
>vector<int​> bfs(unordered_map<int, vector<int​>> adjList, int start, int k) {
>    if (k == 0) return {start};
>    vector<int​> result;
>    deque<int​> queue;
>    unordered_set<int​> visited;
>    queue.push_back(start);
>    visited.insert(start);
>    int distance{0};
>
>    while (!queue.empty() && distance < k) {
>        int currentLength = queue.size();
>        for (int i{0}; i < currentLength; ++i) {
>            int node = queue.front();
>            queue.pop_front();
>            for (int neighbour : adjList[node]) {
>                if (visited.find(neighbour) == visited.end()) {
>                    visited.insert(neighbour);
>                    queue.push_back(neighbour);
>                    // At the kth level, collect nodes
>                    if (distance == k - 1) {
>                        result.push_back(neighbour);
>                    }
>                }
>            }
>        }
>        distance++;
>    }
>    return result;
>}
>```

>[!Note]- BFS with Implicit Graphs
> <!-- Multiline -->
>Sometimes, a graph is more subtle. The input may look nothing like one of the formats we have talked about. Remember that a graph is any abstract collection of elements (nodes) connected by some abstract relationship (edges). If a problem involves transitioning between states, then try to think about if the states can be nodes and the transition criteria can be edges. Additionally, if the problem wants the shortest path or fewest operations etc., it is a great candidate for BFS

# Templates

>[!Info]- Graph BFS Template (Iterative)
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

> [!Info]- Multi-Source BFS (Iterative)
> <!-- Multiline -->
> **<u>~={purple}Explanation=~</u>**:
> 
> Multi-source BFS extends the standard BFS by allowing multiple starting nodes. Instead of a **single source**, we push **all starting points** into the queue initially. This ensures that BFS propagates outward from multiple locations simultaneously. Given a grid of 0's and 1's, what is the shortest distance from 0 to 1's. We will store these values in the original grid.
>
> ![[Drawing 2025-03-07 20.30.11.excalidraw | center | 550]]
> 
> **~={purple}Steps=~**:
> 1. ~={green}**Identify All Starting Points**=~:
>    - Iterate through the grid or input data to collect **all sources**.
>    - Push these sources into the queue **with an initial step count of 0**.
> 2. ~={green}**Perform BFS**=~:
>    - Process all nodes in the queue **level by level**.
>    - Expand to **valid, unvisited** neighbours, updating their state.
> 3. ~={green}**Update the Grid / Distance Matrix**=~:
>    - The BFS modifies an auxiliary grid/matrix to store the **shortest distance** from any source.
>
> ~={red}**Key Differences from Standard BFS=~**:
> - ~={purple}**Standard BFS**=~: Starts from **one** node.
> - ~={purple}**Multi-Source BFS**=~: Starts from **many** nodes, treating them as a single level in the BFS queue.
>
> ~={blue}**C++ Code**=~:
> * Change all 1's to 0 
> * Change all 0's to INT_MAX
> * When expanding from a cell during BFS, `nextStep` represents the number of steps taken from the source to reach the neighbour cell `(nextRow, nextCol)`. The condition: `grid[nextRow][nextCol] > nextStep` ensures that we update the neighbour cell **only if** the new path offers a smaller (or shorter) number of steps than what was previously recorded in `grid[nextRow][nextCol]`.
> 
> ```cpp
> deque<pair<pair<int, int>, int>> queue; // Stores {row, col, steps}
> 
> // Push all starting points into queue
> for (int i{0}; i < rows; ++i) {
>     for (int j{0}; j < cols; ++j) {
>         if (grid[i][j] == 1) { // This cell is a source
>             queue.push_back({{i, j}, 0}); 
>             grid[i][j] = 0; // Mark as visited with step 0
>         } else {
>             grid[i][j] = INT_MAX; // Mark all other cells as "unvisited"
>         }
>     }
> }
> 
> // BFS traversal
> while (!queue.empty()) {
>     auto [pos, currSteps] = queue.front();
>     queue.pop_front();
>     int currRow = pos.first, currCol = pos.second;
>     
>     for (auto& dir : directions) {
>         int nextRow = currRow + dir[0];
>         int nextCol = currCol + dir[1];
>         int nextStep = currSteps + 1; // Explicit step tracking
>         
>         if (nextRow >= 0 && nextCol >= 0 && 
>             nextRow < rows && nextCol < cols &&
>             grid[nextRow][nextCol] > nextStep) { 
>             // Only update if we found a shorter path
>             grid[nextRow][nextCol] = nextStep;
>             queue.push_back({{nextRow, nextCol}, nextStep});
>         }
>     }
> }
> ```
>
> ~={blue}**Key Points**=~:
> - ~={green}**Time Complexity**=~: $O(n \times m)$ (Each cell is processed once).
> - ~={green}**Space Complexity**=~: $O(n \times m)$ (For the queue in the worst case).
> - ~={green}**Use Cases**=~:
>   - Finding the shortest distance **from multiple sources** (e.g., shortest distance to land from all water cells).
>   - Propagation problems (e.g., spreading fire, infection, or signal in a grid).

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

> [!Question]- Matrix: Update Nearest Zero
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an `m x n` binary matrix `mat`, return a matrix `result` such that for each cell in `mat`, the value in `result` is the distance to the nearest `0` in `mat`.
>* The distance between two adjacent cells is `1`.
>
>**~={red}Solution=~**:
>* **~={purple}Approach=~**:
>	1. Instead of performing BFS for each `1` (which is too slow), treat all `0`s as a single "top node."
>	2. Initialize a queue with all the `0`s and perform BFS level by level.
>	3. As we visit cells with `1`, record the number of steps it takes to reach them from the nearest `0`.
>
> ![[Drawing 2025-01-02 17.57.45.excalidraw | center | 500]]
>
>**~={green}<u>Global Variables</u>=~**:
>* `rows` and `cols`: Number of rows and columns in the matrix.
>* `directions`: Array representing the 4 cardinal directions for movement.
>```cpp
>vector<vector<int​>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
>```
>
>**~={green}<u>Iterate Through Matrix</u>=~**:
>* Collect all `0` cells as starting nodes and initialize the `seen` matrix and the result matrix.
>```cpp
>vector<vector<int​>> updateMatrix(vector<vector<int​>>& mat) {
>    this->rows = mat.size();
>    this->cols = mat[0].size();
>
>    vector<vector<bool​>> seen(rows, vector<bool​>(cols, false));
>    vector<vector<int​>> startingNodes;
>
>    for (int i{0}; i < rows; ++i) {
>        for (int j{0}; j < cols; ++j) {
>            if (mat[i][j] == 0) {
>                startingNodes.push_back({i, j, 0});
>            }
>        }
>    }
>
>    vector<vector<int​>> result(rows, vector<int​>(cols, 0));
>
>    bfs(mat, seen, result, startingNodes);
>    return result;
>}
>```
>
>**~={green}<u>BFS</u>=~**:
>* Use BFS to traverse the matrix starting from all `0`s, updating distances for `1`s.
>```cpp
>void bfs(vector<vector<int​>>& mat, vector<vector<bool​>>& seen, vector<vector<int​>>& result, vector<vector<int​>> startingNodes) {
>    deque<vector<int​>> queue;
>    for (vector<int​> startingNode : startingNodes) {
>        queue.push_back(startingNode);
>        seen[startingNode[0]][startingNode[1]] = true;
>    }
>
>    while (!queue.empty()) {
>        vector<int​> node = queue.front();
>        queue.pop_front();
>
>        int currRow = node[0];
>        int currCol = node[1];
>        int currStep = node[2];
>
>        for (vector<int​>& direction : directions) {
>            int nextRow = currRow + direction[0];
>            int nextCol = currCol + direction[1];
>            int nextStep = currStep + 1;
>            if (isValid(mat, seen, nextRow, nextCol)) {
>                seen[nextRow][nextCol] = true;
>                queue.push_back({nextRow, nextCol, nextStep});
>                result[nextRow][nextCol] = nextStep;
>            }
>        }
>    }
>}
>```
>
>**~={green}<u>Helper Function: Valid</u>=~**:
>* Checks if the next cell is within bounds, hasn't been visited, and contains a `1`.
>```cpp
>bool isValid(vector<vector<int​>>& mat, vector<vector<bool​>>& seen, int nextRow, int nextCol) {
>    return ((0 <= nextRow) && (nextRow < rows)) && 
>           ((0 <= nextCol) && (nextCol < cols)) && 
>           (!seen[nextRow][nextCol]) && 
>           (mat[nextRow][nextCol] == 1);
>}
>```

> [!Question]- Shortest Alternating Paths in a Graph
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given a directed graph with `n` nodes, where each edge is either **red** or **blue**. 
>* Return an array `result` where `result[i]` is the shortest number of edges to reach node `i` starting from node `0`, using alternating red and blue edges. If there is no valid path, return `-1` for that node.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2025-01-02 21.17.08.excalidraw |center | 500]]
>
>* **~={purple}Approach=~**:
>	1. Build two adjacency lists: one for red edges and one for blue edges.
>	2. Perform BFS starting from node `0`, tracking the current edge colour and number of steps.
>	3. Alternate the edge colours during traversal, marking visited states with both node and edge colour.
>
>**~={green}<u>Build Adjacency Lists</u>=~**:
>* Create separate adjacency lists for red and blue edges from the input.
>```cpp
>vector<int​> shortestAlternatingPaths(int n, vector<vector<int​>>& redEdges, vector<vector<int​>>& blueEdges) {
>    unordered_map<int, vector<int​>> redAdjList;
>    unordered_map<int, vector<int​>> blueAdjList;
>
>    for (vector<int​> redEdge : redEdges) {
>        redAdjList[redEdge[0]].push_back(redEdge[1]);
>    }
>
>    for (vector<int​> blueEdge : blueEdges) {
>        blueAdjList[blueEdge[0]].push_back(blueEdge[1]);
>    }
>
>    vector<int​> result(n, -1);
>    bfs(redAdjList, blueAdjList, result, 0);
>    return result;
>}
>```
>
>**~={green}<u>Perform BFS</u>=~**:
>* Use BFS to traverse the graph starting from node `0`.
>* Note that we are pushing {Node, Colour Edge To Get To Node, Num Steps To Get To Node}, to our queue.
>* Everytime we pop something from the queue, check if it has been visited before or not, if so, mark it in the `result` array. This will be the shortest distance, by property of DFS.
>```cpp
>void bfs(unordered_map<int, vector<int​>>& redAdjList,
>         unordered_map<int, vector<int​>>& blueAdjList,
>         vector<int​>& result,
>         int startNode) {
>    deque<vector<int​>> queue;
>    set<pair<int, int​>> visited; // Tracks {node, colour} visited states
>
>    queue.push_back({startNode, RED, 0}); // Node, Colour, Steps
>    queue.push_back({startNode, BLUE, 0});
>    visited.insert({startNode, RED});
>    visited.insert({startNode, BLUE});
>
>    while (!queue.empty()) {
>        vector<int​> node = queue.front();
>        queue.pop_front();
>
>        int currNode = node[0];
>        int currColour = node[1];
>        int currSteps = node[2];
>
>        if (result[currNode] == -1) {
>            result[currNode] = currSteps;
>        }
>
>        unordered_map<int, vector<int​>> adjList;
>        if (currColour == RED) {
>            adjList = blueAdjList; // Alternate to blue edges
>        } else {
>            adjList = redAdjList; // Alternate to red edges
>        }
>
>        for (int neighbour : adjList[currNode]) {
>        // To "flip" our current colour, so we traverse to the 
>        // opposite colour, we can subtract our current colour
>        // from 1
>            if (visited.find({neighbour, 1 - currColour}) == visited.end()) {
>                visited.insert({neighbour, 1 - currColour});
>                queue.push_back({neighbour, 1 - currColour, currSteps + 1});
>            }
>        }
>    }
>}
>```

> [!Question]- Snakes and Ladders: Shortest Path
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an `n x n` board representing a game of Snakes and Ladders:
>	- Each cell contains `-1` (no snake or ladder) or the destination square if it has a snake or ladder.
>* Return the minimum number of moves required to reach the last square (`n * n`) starting from square `1`. If it is not possible to reach the last square, return `-1`.
>
>**~={red}Solution=~**:
>
>* **~={purple}Approach=~**:
>	1. Use BFS to explore all possible paths, starting from square `1`.
>	2. For each square, consider moving 1 to 6 steps ahead (like rolling a die).
>	3. If the destination square is reached, return the number of steps taken.
>	4. Handle snakes and ladders by jumping to the destination square if one exists.
>
>**~={green}<u>Convert Square Number to Board Coordinates</u>=~**:
>
>* Convert a square number (1-based index) into `board[row][col]` coordinates:
>	- Calculate row and column using integer division and modulo.
>	- Reverse the column direction for odd rows to handle the zigzag nature of the board.
>	
> **~={blue}Row Calculation=~**:
> * Subtract `1` from the curr number to make it `0`-based.
> * Divide by `n` to determine how many complete rows the square is part of:
>   * `row = (currNum - 1) / n`
> * Flip the row index to account for bottom-to-top numbering:
>   * `boardRow = n - 1 - row`
>
> **~={blue}Column Calculation=~**:
> * Subtract `1` from the curr number to make it `0`-based.
> * Use modulo `(currNum - 1) % n` to find the position within its row:
>   * `col = (currNum - 1) % n`
> * For right-to-left rows (odd-numbered), reverse the column:
>   * `boardCol = n - 1 - col`
>
>
> ![[Drawing 2025-01-02 23.36.35.excalidraw | center | 500]]
>
>```cpp
>pair<int​, int​> numberToCoords(int currNum, int n) {
>    int boardRow = (currNum - 1) / n;
>    int boardCol = (currNum - 1) % n;
>
>    if (boardRow % 2 == 1) { 
>        boardCol = n - 1 - boardCol; // Reverse column for zigzag rows
>    }
>    return {n - 1 - boardRow, boardCol}; // Convert to bottom-up indexing
>}
>```
>
>**~={green}<u>BFS Traversal</u>=~**:
>* Use BFS to explore all possible paths starting from square `1`:
>	- Process each node, tracking the current square (`currNum`) and steps taken (`currSteps`).
>	- For each square, attempt to roll the die (1 to 6 steps forward).
>	- If the next square has a snake or ladder, jump to the destination square.
>	- Skip already visited squares to avoid cycles.
>```cpp
>int snakesAndLadders(vector<vector<int​>>& board) {
>   int rows = board.size();
>   int cols = board[0].size();
>
>   deque<vector<int​>> queue;
>   queue.push_back({1, 0}); // Start at square 1 with 0 steps
>   unordered_set<int​> seen;
>   seen.insert(1);
>
>   while (!queue.empty()) {
>        vector<int​> node = queue.front();
>        queue.pop_front();
>
>        int currNum = node[0];
>        int currSteps = node[1];
>		
>        // Reached last square, which is the square (Rows x Cols)
>        if (currNum == rows * cols) return currSteps;
>		
>        // Look at the next 6 squares (With a cap so we don't overflow)
>        for (int next = currNum + 1; next <= min(currNum + 6, rows * cols); ++next) {
>            pair<int​, int​> coords = numberToCoords(next, rows);
>            int destination = next;
>            if (board[coords.first][coords.second] != -1) {
> 	           // Jump to destination
>                destination = board[coords.first][coords.second]; 
>            }
>            if (seen.find(destination) == seen.end()) {
>                seen.insert(destination);
>                queue.push_back({destination, currSteps + 1});
>            }
>        }
>   }
>   return -1; // No path to last square
>}
>```

## Implicit Graphs (DFS or BFS)

> [!Question]- Open the Lock (BFS Shortest Path)
> <!-- Multiline -->
> **~={red}Question=~**:
>* You have a lock represented as a 4-digit combination, where each digit can be rotated up (`+1`) or down (`-1`).
>* You are given a list of `deadends`, which are combinations that cannot be used.
>* Starting at "0000", return the minimum number of moves to reach the `target`. If it's impossible, return `-1`.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2025-01-03 13.10.54.excalidraw | center | 700]]
>
>* **~={purple}Approach=~**:
>	1. Treat each combination as a node in a graph.
>	2. Use BFS to traverse all possible combinations, starting from "0000."
>	3. Track visited nodes to avoid cycles and include `deadends` as visited.
>	4. Stop when the `target` combination is found.
>
>**~={green}<u>Initialisation</u>=~**:
>* Add the initial state "0000" to the queue.
>* Mark all `deadends` as visited to prevent traversal through them.
>```cpp
>int openLock(vector<string​>& deadends, string target) {
>    deque<vector<int​>> queue;
>    set<vector<int​>> visited;
>    // Initial state: {0, 0, 0, 0, steps = 0}
>    queue.push_back({0, 0, 0, 0, 0});
>    // Mark "0000" as visited
>    visited.insert({0, 0, 0, 0});    
>
>    // Add all deadends to visited
>    for (string deadend : deadends) {
> 	   // Immediate failure if "0000" is a deadend
>        if (deadend == "0000") return -1;
>        vector<int​> deadendVector = {deadend[0] - '0', deadend[1] - '0', deadend[2] - '0', deadend[3] - '0'};
>        visited.insert(deadendVector);
>    }
>```
>
>**~={green}<u>BFS Traversal</u>=~**:
>* Process each combination level by level.
>* For the current combination:
>	1. Check if it matches the `target`.
>	2. Generate all possible next combinations by rotating each digit up or down.
>	3. If a combination hasn’t been visited, add it to the queue.
>```cpp
>    while (!queue.empty()) {
>        vector<int​> node = queue.front();
>        queue.pop_front();
>
>        // Convert node vector to string for comparison with target
>        string currString = std::to_string(node[0]) + 
>                            std::to_string(node[1]) +
>                            std::to_string(node[2]) +
>                            std::to_string(node[3]);
>
>        int currSteps = node[4];
>        if (currString == target) return currSteps;
>
>        for (int i{0}; i < 4; ++i) {
>            // Rotate digit up
>            string moveOne = currString;
>            moveOne[i] = (currString[i] - '0' + 1) % 10 + '0';
>
>            // Rotate digit down
>            string moveTwo = currString;
>            moveTwo[i] = (currString[i] - '0' - 1 + 10) % 10 + '0';
>
>            vector<int​> moveOneVector, moveTwoVector;
>            for (int j{0}; j < 4; ++j) {
>                moveOneVector.push_back(moveOne[j] - '0');
>                moveTwoVector.push_back(moveTwo[j] - '0');
>            }
>
>            // Add valid moves to the queue
>            if (visited.find(moveOneVector) == visited.end()) {
>                visited.insert(moveOneVector);
>                moveOneVector.push_back(currSteps + 1);
>                queue.push_back(moveOneVector);
>            }
>            if (visited.find(moveTwoVector) == visited.end()) {
>                visited.insert(moveTwoVector);
>                moveTwoVector.push_back(currSteps + 1);
>                queue.push_back(moveTwoVector);
>            }
>        }
>    }
>    return -1; // If BFS completes without finding the target
>}
>```

> [!Question]- Evaluate Division (DFS or BFS on "Weighted Graph")
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given a list of equations with values, e.g., `a / b = 2.0`, and a list of queries asking for values such as `a / c`. 
>* If no valid path exists for a query, return `-1`.
>* Build a graph from the equations and solve each query using DFS.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2025-01-03 13.21.26.excalidraw | center | 450]]
>
>* **~={purple}Approach=~**:
>	1. Represent the equations as a weighted directed graph:
>		- Each node is a variable (e.g., `a`, `b`).
>		- Each edge represents the equation (`a / b = value`).
>	2. Use DFS to evaluate queries by traversing the graph.
>	3. If a query has no valid path, return `-1`.
>
>**~={green}<u>Building the Graph</u>=~**:
>* Construct an adjacency list to represent the graph:
>	- For each equation `a / b = value`, add:
>		1. An edge from `a` to `b` with weight `value`.
>		2. An edge from `b` to `a` with weight `1 / value`.
>```cpp
>​vector<double​> calcEquation(vector<vector<string​>>& equations, vector<double​>& values, vector<vector<string​>>& queries) {
>​    unordered_map<string, vector<pair<string, double>>> adjList;
>
>​    for (int i{0}; i < equations.size(); ++i) {
>​        string source = equations[i][0];
>​        string dest = equations[i][1];
>
>​        adjList[source].push_back({dest, values[i]});
>​        adjList[dest].push_back({source, 1 / values[i]});
>​    }
>```
>
>**~={green}<u>Processing Queries</u>=~**:
>* For each query:
>	1. Use DFS to check if there’s a valid path between `source` and `dest`.
>	2. If no path exists or if either node doesn’t exist, return `-1`.
>```cpp
>​    vector<double​> result;
>​    for (const auto& query : queries) {
>​        string source = query[0];
>​        string dest = query[1];
>
>​        dfs(adjList, result, source, dest);
>​    }
>
>​    return result;
>}
>```
>
>**~={green}<u>DFS Implementation</u>=~**:
>* Traverse the graph to evaluate the value of the query:
>	1. Use a stack for iterative DFS and a set to track visited nodes.
>	2. Multiply edge weights as you traverse the graph.
>	3. If the destination node is reached, return the cumulative product.
>	4. If the stack is exhausted without finding the destination, return `-1`.
>```cpp
>​void dfs(unordered_map<string, vector<pair<string, double>>>& adjList, vector<double​>& result, string source, string dest) {
>​    if (adjList.find(source) == adjList.end() || adjList.find(dest) == adjList.end()) {
>​        result.push_back(-1);
>​        return;
>​    }
>    
>​    deque<pair<string, double>> stack;
>​    set<string​> visited;
>​    stack.push_back({source, 1});
>​    visited.insert(source);
>
>​    while (!stack.empty()) {
>​        pair<string, double> node = stack.back();
>​        stack.pop_back();
>
>​        string currNode = node.first;
>​        double currValue = node.second;
>
>​        if (currNode == dest) {
>​            result.push_back(currValue);
>​            return;
>​        }
>
>​        for (const auto& neighbour : adjList[currNode]) {
>​            string nextNode = neighbour.first;
>​            double multiplier = neighbour.second;
>​            if (visited.find(nextNode) == visited.end()) {
>​                visited.insert(nextNode);
>​                stack.push_back({nextNode, currValue * multiplier});
>​            }
>​        }
>​    }
>​    result.push_back(-1);
>}
>```
>

#flashcards/dsa/patterns/graphdfs

