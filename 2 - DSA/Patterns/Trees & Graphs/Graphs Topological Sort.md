## Points of Interest

>[!Note]- What is a Topological Ordering
> <!-- Multiline -->
> ![[Drawing 2025-02-02 23.09.43.excalidraw | center | 350]]
>A topological ordering is an ordering of the nodes in a directed graph where for each directed edge from node A to node B, node A appears before node B in the ordering.

>[!Note]- When is a Topological Sort Applicable?
> <!-- Multiline -->
>The graph must be a **~={blue}Directed Acyclic Graph=~**. This is because we require:
>* **~={red}Dependencies=~**: Nodes to depend on another (hence directed)
>* **~={red}No cycles=~**: Otherwise we get stuck in a infinite cycle, which would result in no topological sort

>[!Note]- Are Topological Orderings Unique?
> <!-- Multiline -->
>No, we can have multiple topological orderings from a single graph

## Templates

>[!Info]- Topological Sort (Kahn's Algorithm)
><!-- Multiline -->
><u>**Explanation**</u>
>
>1. **~={blue}Build Adjacency List & Indegree List=~**:
>* We want to store the in degree of every node. For each edge in our graph, `u -> v`, we shall do `inDegree[v]++`
>
>```cpp
>vector<int​> topologicalSort(int n, vector<vector<int​>>& edges) {
>	unordered_map<int, int​> inDegree;
>	unordered_map<int, vector<int​>​> adjList;
>	for (const auto& edge : edges) {
>		int first = edge[0]; int second = edge[1];
>		adjList[first].push_back(second);
>		inDegree[second]++;
>		
>		// To ensure every node is in this map
>		if (inDegree.find(u) == inDegree.end()) inDegree[u] = 0;
>	}
>}
>```
>
>1. **~={blue}BFS=~**:
>* First add all nodes with in degree 0 to our queue
>* For every neighbour visited, we shall add nodes with in degree 0 until it is no longer possible
>
>```cpp
>	deque<int​> queue;
>	vector<int​> topOrder;
>	
>	// Push all nodes with in-degree 0 into the queue
>	for (const auto pair : inDegree) {
>		int node = pair.first;
>		int degree = pair.second;
>		if (degree == 0) queue.push_back(node);
>	}
>	
>	while (!queue.empty()) {
>		int topNode = queue.front();
>		queue.pop_front();
>		
>		// We have now visited a node
>		topOrder.push_back(topNode);
>		
>		for (int neighbour : adjList[topNode]) {
>			// When we visit a neighbour, as the top node is getting 
>			// removed, the indegree of the neighbour decreases
>			inDegree[neighbour]--;
>			// Add nodes with in degree 0 to the queue
>			if (inDegree[neighbour] == 0) queue.push_back(neighbour);
>		}
>	}
>	// If the topological order size = number of nodes, then
>	// we have found a topological ordering, otherwise, return an
>	// empty array
>	return (topoOrder.size() == inDegree.size()) 
>			? topoOrder : vector<int​>();
>```
>**~={blue}Complexity=~**
>* **~={green}Time Complexity=~**: $O(V + E)$
>* **~={green}Space Complexity=~**: $O(V + E)$

#flashcards/dsa/patterns/graphtopologicalsort