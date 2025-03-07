## Points of Interest

>[!Note]- What is Dijkstra's Algorithm?
> <!-- Multiline -->
> * Dijkstra's algorithm focuses on one **source** node, and will find the **~={green}shortest distance to every other node in the graph from the source=~**.
> * It differs from BFS where it can handle **~={blue}weighted edges=~**
> * It cannot handle **~={red}negative weights=~**
> 
>  ![[Drawing 2025-03-07 11.49.30.excalidraw | center | 700]]

## Template

> [!Info]- Dijkstra’s Algorithm (Shortest Path)
> <!-- Multiline -->
> **~={red}Question=~**:
> - Given a weighted graph with `n` nodes and `m` edges, find the shortest path from a given `source` node to all other nodes.
> - If a node is unreachable, its shortest path should remain **infinity**.
>
> ~={green}**Solution Overview**=~:
> 1. ~={purple}**Use a Min-Heap (Priority Queue)**=~:
>    - The **priority queue** stores pairs **(current distance, node)**.
>    - We always expand the **closest unvisited node** first.
> 2. ~={purple}**Maintain a `distances[]` Array**=~:
>    - `distances[i]` stores the **shortest known distance** from `source` to `i`.
>    - Initially, set all values to **infinity**, except `distances[source] = 0`.
> 3. ~={purple}**Iterate Through Neighbors**=~:
>    - If traversing an edge results in a **shorter path** than previously recorded, update `distances[neighbor]` and push `(new distance, neighbor)` to the heap.
>
> ~={green}**Code Implementation**=~:
> ```cpp
> vector<int​> dijkstra(int n, vector<vector<pair<int, int>>>& adj, int source) {
>     vector<int​> distances(n, INT_MAX); // Initialise distances to "infinity"
>     distances[source] = 0;
>     
>     priority_queue<pair<int, int>, 
> 				   vector<pair<int, int>>, 
> 				   greater<pair<int, int>>> minHeap;
>     minHeap.push({0, source}); // {distance, node}
>     
>     while (!minHeap.empty()) {
> 	    pair<int, int> topNode = minHeap.top();
> 	    int currDist = topNode.first;
> 	    int node = topNode.second;
>         minHeap.pop();
> 
>         // Optimisation: If we found a shorter distance before, skip this one
>         if (currDist > distances[node]) continue;
>         
>         // Iterate over neighbors
>         for (auto& [neighbor, weight] : adj[node]) {
>             int newDist = currDist + weight;
>             
>             // If a shorter path is found, update and push to heap
>             if (newDist < distances[neighbor]) {
>                 distances[neighbor] = newDist;
>                 minHeap.push({newDist, neighbor});
>             }
>         }
>     }
>     
>     return distances;
> }
> ```
>
> ~={blue}**Key Points**=~:
> - **Min-Heap** ensures we always expand the **nearest** unvisited node first.
> - ~={green}**Time Complexity**=~:
>   - **$O((n + m) \log n)$**: Each edge and node is processed with logarithmic heap operations.
> - ~={green}**Space Complexity**=~:
>   - **$O(n + m)$**: Storing distances and adjacency list.

#flashcards/dsa/patterns/graphdijkstra
