> [!QUOTE] Quick Notes
> Traverse a graph

# Overview
## Recipe

>[!Note]- Graph DFS Recipe
> <!-- Multiline -->
>1. ~={purple}**Why DFS on Graphs**=~:

## Time Complexity

>[!Note]- Time Complexity of DFS on Graph
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: We will visit every node on the tree once, hence, $O(n)$, where $n$ is the number of nodes in the tree

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
> * Use adjacency matrix for **~={green}small=~**, **~={red}dense=~** (di)graphs for which we wish to test for the existence of arcs, find the lin-degree of vertices, and/or delete arcs.
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

# Templates

>[!Info]- BT DFS Template (Iterative)
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
>3.**~={blue}DFS=~**: While the stack is not empty:
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

>[!Info]- BT DFS Template (Recursive)
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

> [!Question]- Root to Leaf Path Min Sum (Increment in Return)
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the **root** of a binary tree, explore all possible **root-to-leaf paths**, compute the sum of values along each path, and return the **minimum sum.**
>* A **leaf node** is a node with no children.
>
>**~={red}Solution=~**:
>
> ![[Drawing 2024-12-30 21.35.40.excalidraw | center | 700]]
>
>```cpp
>int minRootToLeafSum(TreeNode* root) {
>	// Base Case: If we end up at a parent, where it has 1 child, and
>	// 1 null child, if we traverse to the null child, we return
>	// INT_MAX, such that it gets ignored
>	if (root == nullptr) return INT_MAX;
>		
>	// Base Case: If we reach a leaf node, we return that node's
>	// value. This allows us to effectively, never visit the 
>	// leaf child's null children
>	if (root->left == nullptr && root->right ==nullptr) {
>		return root.val;
>	}
>	
>	int leftMinSum = minRootToLeafSum(root->left);
>	int rightMinSum = minRootToLeafSum(root->right);
>	
>	// Recursive Case: From the perspective of the parent node,
>	// we return the sum of the parent + the sum of it's smallest
>	// child subtree, propagating it up
>	return root->val + min(leftMinSum, rightMinSum)l
>}
>```

#flashcards/dsa/patterns/graphdfs
