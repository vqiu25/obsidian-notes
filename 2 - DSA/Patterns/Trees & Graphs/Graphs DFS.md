> [!QUOTE] Quick Notes
> Traverse a graph (visit everything associated with a node)

# Overview
## Recipe

>[!Note]- Graph DFS Recipe
> <!-- Multiline -->
>1. ~={purple}**Why DFS on Graphs**=~:

## Time Complexity

>[!Note]- Time Complexity of DFS on Graph
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: The time complexity is $O(n+e)$, where $n$ is the number of nodes and $e$ is the number of edges in the graph. Every node and every edge is visited once during the traversal.
> 	* However, for dense graphs, $e=n(n-1)/2$, as every node, is connected to every other node, i.e. for every n nodes, they have n-1 neighbours, all with edges, resulting in $O(n^2)$. An adjacency matrix will always be $O(n^2)$ though.
> * **~={purple}Space Complexity=~**: $O(n+e)$ for storing nodes and edges

## Points of Interest

>[!Note]- Graph Terminology
> <!-- Multiline -->
> * **~={green}Connected Component=~**: A connected component of a graph is a group of nodes that are connected by edges.
> * **~={green}Indegree=~**: The number of edges that can be used to reach the node is the node's **indegree**.
> * **~={green}Outdegree=~**: The number of edges that can be used to leave the node is the node's **outdegree**.

>[!Note]- Adjacency List
> <!-- Multiline -->
> * An adjacency list is a vector that contains vectors. The index position of each vector, and its corresponding vector, tells you the neighbours of node $index$.
> * Use adjacency list for **~={red}large=~**, **~={green}sparse=~** (di)graphs for which we need to compute out-degree and add and/or delete vertices.
> * To form an adjacency list (undirected) from pairs of nodes:
> ```cpp
> unordered_map<int, vector<int​>> adjList;
> for (const vector<int​>& edge : edges) {
> 	// Add edge[0] -> edge[1]
> 	adjList[edge[0]].push_back(edge[1]);
> 	// Add edge[1] -> edge[0] (for undirected graph)
> 	adjList[edge[1]].push_back(edge[0]);
> }
>```

>[!Note]- Adjacency Matrix
> <!-- Multiline -->
> * An adjacency matrix is read from the Left -> Top. (i.e. Read the y axis, then the x axis first. If there is a 1 in that square, there means there is an edge from y->x)
> * Use adjacency matrix for **~={green}small=~**, **~={red}dense=~** (di)graphs for which we wish to test for the existence of arcs, find the in-degree of vertices, and/or delete arcs.
> * To convert a matrix `vector<vector<int>> matrix` to an adjacency list for traversal:
> ```cpp
> unordered_map<int, vector<int​>> adjList;
> int n = matrix.size(); // Number of nodes
> for (int i{0}; i < n; ++i) {
> 	for (int j{0}; j < n; ++j) {
> 		if (matrix[i][j] == 1) {
> 			adjList[i].push_back(j);
> 		}
> 	}
> }
>```

>[!Note]- How to use an Adjacency Matrix for in-degree and out-degree
> <!-- Multiline -->
> * **~={green}Indegree=~**: Sum of column $x$, = indegree of node $x$
> * **~={green}Outdegree=~**: Sum of row $x$, = outdegree of node $x$

>[!Note]- Are nodes "given" to us in graph problems?
> <!-- Multiline -->
> The nodes aren't exactly given to us. We are simply told that there exists some nodes numbered from 0 to n - 1, and we are given information regarding the edges. Thus, we treat the integers from $$[0, n - 1]$$ as the nodes. 

> [!Note]- Finding the Number of Connected Components in Undirected Graph from Adjacency List
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an **undirected graph** represented as an adjacency list, find the number of connected components in the graph.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* Each connected component in the graph can be identified by performing a **DFS** (or BFS) starting from an unvisited node.
>	* Each DFS will mark all nodes in the same connected component as visited.
>	* The total number of DFS calls corresponds to the number of connected components.
>
>* **Steps**:
>	1. Iterate through all nodes in the adjacency list.
>	2. For each unvisited node, perform a DFS and mark all reachable nodes as visited.
>	3. Increment a counter for every DFS call, as it identifies a new connected component.
>
> ![[Drawing 2025-01-01 20.57.25.excalidraw | center | 600]]
>
>**~={red}Code=~**:
>```cpp
>int findCircleNum(unordered_map<int, vector<int​>>& adjList) {
>    set<int​> visited;
>    int connectedComponents{0};
>
>    for (const auto& pair : adjList) {
> 	   // Perform DFS on only unvisited nodes to find
> 	   // connected components
>        if (visited.find(pair.first) == visited.end()) {
>            dfsConnectedComponent(adjList, visited, pair.first);
>            connectedComponents++;
>        }
>    }
>
>    return connectedComponents;
>}
>
>// (2) Standard DFS: We are passing by reference a visited set
>void dfsConnectedComponent(unordered_map<int, vector<int​>>& adjList, set<int​>& visited, int startingNode) {
>    deque<int​> stack;
>    stack.push_back(startingNode);
>    visited.insert(startingNode);
>
>    while (!stack.empty()) {
>        int node = stack.back();
>        stack.pop_back();
>
>        for (int neighbour : adjList[node]) {
>            if (visited.find(neighbour) == visited.end()) {
>                visited.insert(neighbour);
>                stack.push_back(neighbour);
>            }
>        }
>    }
>}
>```

> [!Note]- Bipartite Graph Check (Multiple Components)
> 
> ~={purple}Explanation=~
> 
> 1. **~={red}Purpose=~**: To determine if an **entire graph** is bipartite, including all connected components.
> 2. **~={green}Methodology=~**:
>     - Use a helper function (`getColouring`) to perform **iterative DFS** and check the bipartiteness of a single component.
>     - Iterate over all nodes and handle **disconnected components**.
> 3. **~={green}Approach=~**:
>     - Maintain a `colour` vector initialized to `-1` to track the colour (or unvisited status) of each node.
>     - For each unvisited node, invoke the `getColouring` function.
>     - If any component is not bipartite, the graph is not bipartite.
> 
> ~={purple}**Code: Helper Function for 2-Colouring**=~
> 
> ```cpp
> bool getColouring(int start, vector<vector<int​>>& adj, vector<int​>& colour) {
>     deque<int​> s;
>     s.push_back(start);
>     colour[start] = 0; // Start with colour 0
> 
>     while (!s.empty()) {
>         int node = s.back();
>         s.pop_back();
> 
>         // Iterate through all neighbours
>         for (int neighbour : adj[node]) {
>             if (colour[neighbour] == -1) {
>                 // Assign the opposite colour to the neighbour
>                 colour[neighbour] = 1 - colour[node];
>                 s.push_back(neighbour);
>             } else if (colour[neighbour] == colour[node]) {
>                 // If the neighbour has the same colour, 
>                 // the graph is not bipartite
>                 return false;
>             }
>         }
>     }
> 
>     return true; // The component is bipartite
> }
> ```
> 
> ~={purple}**Code: Handling Multiple Components**=~
> 
> ```cpp
> vector<int​> colour(n, -1); // Initialise the colour vector with -1 (unvisited)
> 
> bool isBipartite = true;
> for (int i = 0; i < n; ++i) {
>     if (colour[i] == -1) {
>         // Perform 2-colouring for each connected component
>         if (!getColouring(i, adj, colour)) {
>             isBipartite = false;
>             break;
>         }
>     }
> }
> ```

# Templates

>[!Info]- Graph DFS Template (Iterative)
><!-- Multiline -->
><u>**Explanation**</u>
>
>Here’s the text extracted so you can copy and paste it:
>1. **~={blue}Function Parameters=~**: Graph in the form of an adjacency list, as well as the starting node
>2. **~={blue}Stack and Set=~**:
>* Stack: Next node to traverse
>* Set: Track visited nodes
>
>Add the starting node to both.
>
>3. **~={blue}DFS=~**: While the stack is not empty:
>* We find the top node in the stack, and remove it
>* For every one of its **~={red}unvisited neighbours=~**, mark them as visited (set), and add them to the stack so their neighbours can be explored.
>
><u>**C++ Code**</u>
>```cpp
>int dfs(unordered_map<int, vector<int​>>& graph, int startNode) {
>	// (1) Stack and Set
>	deque<int​> stack; // To keep track of nodes to visit
>	unordered_set<int​> visited; // To keep track of visited nodes
>	
>	stack.push_back(startNode); // Add the starting node to the stack
>	visited.insert(startNode); // Mark the starting node as visited
>	
>	int result = 0;
>	
>	// (2) DFS Loop
>	while (!stack.empty()) {
>		int node = stack.back(); // Get the current node from the stack
>		stack.pop_back(); // Remove it from the stack
>		
>		// Do Some Logic (example: result += node or processing the node)
>		
>		// Explore all unvisited neighbors
>		for (int neighbor : graph[node]) {
>			// Check if not visited
>			if (visited.find(neighbor) == visited.end()) {
>				visited.insert(neighbor); // Mark neighbor as visited
>				stack.push(neighbor); // Add neighbor to the stack
>			}
>		}
>	}
>	return result;
>}
>```

>[!Info]- Graph DFS Template (Recursive)
><!-- Multiline -->
><u>**Explanation**</u>
>
>1. **~={blue}Function Parameters=~**: Graph in the form of an adjacency list, the current node, and a set to track visited nodes.
>2. **~={blue}Recursive DFS Logic=~**:
>* Mark the current node as visited.
>* Perform any required logic with the current node.
>* Recursively explore all unvisited neighbors of the current node.
>
><u>**C++ Code**</u>
>```cpp
>void dfs(unordered_map<int, vector<int​>>& graph, int node, unordered_set<int​>& visited) {
>    // Mark the current node as visited
>    visited.insert(node);
>
>    // Do Some Logic (example: processing the node)
>    // Example: cout << node << " ";
>
>    // Recur for all unvisited neighbors
>    for (int neighbor : graph[node]) {
>        if (visited.find(neighbor) == visited.end()) {
>            dfs(graph, neighbor, visited);
>        }
>    }
>}
>
>void traverseGraph(unordered_map<int, vector<int​>>& graph, int startNode) {
>    unordered_set<int​> visited; // To track visited nodes
>    dfs(graph, startNode, visited);
>}
>```

# Example Problems

> [!Question]- Number of Islands (Traversing a Grid)
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.
>* An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2025-01-01 22.43.47.excalidraw | center | 500]]
>
>**~={green}<u>Global Variables</u>=~**
>* `rows`: Number of rows in the grid
>* `cols`: Number of columns in the grid
>* `directions`: {change in row, change in col}
>	* **~={blue}{0, 1}=~**: Move right **~={blue}(row, col) -> (row, col + 1)=~**
>	* **~={blue}{1, 0}=~**: Move down **~={blue}(row, col) -> (row + 1, col)=~**
>	* **~={blue}{0, -1}=~**: Move left ~={blue}(row, col) -> (row, col - 1)=~
>	* **~={blue}{-1, 0}=~**: Move up **~={blue}(row, col) -> (row - 1, col + 1)=~**
>```cpp
>class Solution {
>private:
>	int rows;
>	int cols;
>	vector<vector<int​>> directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
>```
>* `valid()`: Helper function for DFS to check if the next node being visited is a valid square or not.
>	* We have to do it all at once, as doing it like this means we don't short circuit
>```cpp
>	bool valid(vector<vector<char​>>& grid, vector<vector<bool​>>& seen, int nextRow, int nextCol) {
>		bool inRowBound = (0 <= nextRow) && (nextRow < this->rows);
>		bool inColBound = (0 <= nextCol) && (nextCol < this->cols);
>		bool containsOne = (grid[nextRow][nextCol] == '1');
>		bool notSeen = !seen[nextRow][nextCol];
>		return inRowBound && inColBound && containsOne && notSeen;
>}
>```
>
>**~={green}<u>Iterating Through Every Square (Entry)</u>=~**
>* `seen`: A "set" which keeps track of what squares have already been visited such that we don't repeat traversals
>```cpp
>int numIslands(vector<vector<char​>>& grid) {
>	this->rows = grid.size();
>	this->cols = grid[0].size();
>	vector<vector<bool​>> seen(rows, vector<bool​>(cols, false));
>```
>Iterate through the entire grid, and if it an island ('1') square, and it hasn't been visited before, perform a DFS traversal.
>```cpp
>	int result{0};
>	
>	for (int i{0}; i < rows; ++i) {
>		for (int j{0}; j < cols; ++j) {
>			if (grid[i][j] == '1' && !seen[i][j]) {
>				result++;
>				dfs(grid, seen, i, j);
>			}
>		}
>	}
>	return result
>}
>```
>**~={green}<u>DFS</u>=~**
>* Rather than a `set<int> visited`, we will update our boolean 2d array
>```cpp
>void iterativeDFS(vector<vector<char​>>& grid, vector<vector<bool​>>& seen, int startRow, int startCol) {
>	deque<pair<int, int​>> stack;
>	stack.push_back({startRow, startCol});
>	seen[startRow][startCol] = true;
>```
>1. For every square/node we visit
>2. **~={blue}Check Up/Down/Left/Right=~**: Iterate through the directions global variable to see what square the current node will land on
>3. **~={green}If Valid=~**: Update our seen 2d array, and push it to the stack so it's neighbours can be xplored
>```cpp
>	while (!stack.empty()) {
>		pair<int, int​> square = stack.back();
>		row = square.first;
>		col = square.second;
>		stack.pop_back();
>	
>		for (vector<int​> direction : this->directions) {
>			int nextRow = row + direction[0];
>			int nextCol = col + direction[1];
>			if (valid(grid, seen, nextRow, nextCol)) {
>				seen[nextRow][nextCol] = true;
>				stack.push_back({nextRow, nextCol});
>			}
>		}
>	}
>```

> [!Question]- Minimum Number of Vertices to Reach All Nodes
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a **directed acyclic graph**, with `n` vertices numbered from `0` to `n-1`, and an array `edges` where `edges[i] = [fromi, toi]` represents a directed edge from node `fromi` to node `toi`.
>* Find _the smallest set of vertices from which all nodes in the graph are reachable_. It's guaranteed that a unique solution exists.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**: Return the number of source nodes / nodes with in-degree of zero
>
> ![[Drawing 2025-01-02 12.46.28.excalidraw | center | 200]]
>
>```cpp
>vector<int​> findSmallestSetOfVertices(int n, vector<vector<int​>>& edges) {
>	vector<int​> result;
>	// Create a vector to store the indegree count of each node
>	vector<int​> inDegree;
>	
>	// An edge [a, b] is a->b. Thus everytime b appears, we increment
>	// to count it's indegree
>	for (const vector<int​>& edge : edges) {
>		inDegree[edge[1]]++;
>	}
>	
>	for (int i{0}; i < n; ++i) {
>		if (inDegree[i] == 0) {
>			result.push_back(i);
>		}
>	}
>	return result;
>```

> [!Question]- Number of Reachable Nodes in a Restricted Graph
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a graph with `n` nodes (0 to `n-1`), edges represented as `edges`, and a list of restricted nodes, find the number of nodes that can be reached starting from node `0` while avoiding restricted nodes.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* We perform a **DFS** starting from node `0`.
>	* During traversal, skip nodes that are either:
>		* In the `restricted` list.
>		* Already visited.
>	* Use an adjacency list to represent the graph for efficient traversal.
>
>* **Steps**:
>	1. Create an adjacency list for the graph from the given `edges`.
>	2. Convert the `restricted` list into a set for quick lookup.
>	3. Use a **DFS** to traverse the graph:
>		* Keep track of visited nodes.
>		* Skip restricted or already visited nodes.
>	4. The size of the `visited` set gives the count of reachable nodes.
>
>**~={red}Code=~**:
>```cpp
>int reachableNodes(int n, vector<int​>& edges, vector<int​>& restricted) {
>    unordered_set<int​> restrictedSet{restricted.begin(), restricted.end()};
>
>    // Create an adjacency list
>    unordered_map<int​, vector<int​>> adjList;
>
>    for (vector<int​> edge : edges) {
>        adjList[edge[0]].push_back(edge[1]);
>        adjList[edge[1]].push_back(edge[0]);
>    }
>
>    // Perform DFS on node 0
>    return dfs(adjList, restrictedSet, 0);
>}
>
>int dfs(unordered_map<int​, vector<int​>> adjList, unordered_set<int​> restrictedSet, int startingNode) {
>    deque<int​> stack;
>    unordered_set<int​> visited;
>    stack.push_back(startingNode);
>    visited.insert(startingNode);
>
>    while (!stack.empty()) {
>        int node = stack.back();
>        stack.pop_back();
>
>        for (int neighbour : adjList[node]) {
> 	       // Do not visit restricted nodes, or priorly visited nodes
>            if (restrictedSet.find(neighbour) == restrictedSet.end() && visited.find(neighbour) == visited.end()) {
>                visited.insert(neighbour);
>                stack.push_back(neighbour);
>            }
>        }
>    }
>    return visited.size();
>}
>```

#flashcards/dsa/patterns/graphdfs
