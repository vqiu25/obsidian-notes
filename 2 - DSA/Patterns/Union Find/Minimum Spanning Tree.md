# Points of Interest

>[!Note]- How to change Min Span Tree to Max Span Tree
> <!-- Multiline -->
>Rather then sorting the weights from ascending order, change it to descending order
>```cpp
>sort(connections.begin(), connections.end(), [](auto& a, auto& b) { 
>return a[2] > b[2]; // Sort by cost in descending order 
>});
>```

>[!Note]- Minimum Spanning Tree Algorithm Comparisons
> <!-- Multiline -->
> ![[Drawing 2025-04-21 14.42.33.excalidraw | center | 700]]
# Templates

> [!Info]- Kruskal’s Algorithm
> 
> **~={blue}Explanation=~**
> 
> 1. **~={purple}Sort the Edges=~**: Sort edges in **ascending order** based on weight. Edges are in the form (Node X, Node Y, Weight)
> 2. **~={purple}Union Find Structure=~**: Use **Union-Find** to track which components are connected.
> 3. **~={purple}Construct MST=~**: Iterate through the sorted edges:
>     - If the two endpoints **belong to different components**, merge them using `unite()`.
>     - Stop once **(V - 1) edges** are added to the MST.
> 
> ~={green}**C++ Code**=~
> 
> ```cpp
> vector<vector<int​>> kruskalMST(int n, vector<vector<int​>> &edges) {
> 
>     sort(edges.begin(), edges.end(), 
>     [](const vector<int> &a, const vector<int> &b) {
>         return a[2] < b[2]; // Sort edges by weight
>     });
>     
>     UnionFind uf(n);
>     vector<vector<int​>> mst;
>     int numEdges{0};
>     
>     for (const auto &edge : edges) {
>         int u = edge[0], v = edge[1], weight = edge[2];
>         // If we can add the 2 nodes to the same bag
>         if (uf.unite(u, v)) {
>             mst.push_back({u, v, weight});
>             numEdges++;
>             // Stop when n - 1 edges have been used as that means
>             // our MST of nodes is complete
>             if (numEdges == n - 1) break;
>         }
>     }
>     return mst;
> }
> ```
> 
> **~={green}Time Complexity=~**
> 
> - **Sorting the edges:** O(Elog⁡E)
> - **Union-Find operations:** O(Eα(V))
> - **Overall Complexity:** O(Elog⁡E)

# Sample Problems

> [!Question]- Minimum Cost to Connect All Points (Kruskal's Algorithm)  
> <!-- Multiline -->  
> ~={red}**Question**=~:  
> * Given `n` points on a 2D plane, connect all points using the **minimum total cost**.  
> * The **cost** to connect two points is the **Manhattan distance**:  
>   - `distance = |x1 - x2| + |y1 - y2|`.  
> * You can **only connect points directly**.  
> * Return the **minimum cost** to connect all points.  
>  
> ~={red}**Solution (Kruskal’s Algorithm)**=~:  
> 1. ~={blue}**Generate All Edges**=~:  
>    - Compute the **Manhattan distance** between every pair of points.  
>    - Store edges in a list as `{distance, {point1, point2}}`.  
> 2. ~={blue}**Sort Edges by Cost**=~:  
>    - This ensures we **consider the shortest edges first**.  
> 3. ~={blue}**Use Union-Find (Disjoint Set)**=~:  
>    - Iterate through sorted edges and connect two points **if they are in different components**.  
>    - Stop when **n-1 edges** have been added (Minimum Spanning Tree condition).  
> 4. **Return Total Cost**.  
>  
> ~={green}**Code (Kruskal's Algorithm)**=~:  
> ```cpp  
> class Solution {  
> public:  
>     int minCostConnectPoints(vector<vector<int​>>& points) {  
>         int n = points.size();  
>         vector<pair<int, pair<int, int>>> edges;  
>  
>         // (1) Compute all pairwise Manhattan distances  
>         for (int i{0}; i < n; ++i) {  
>             int pointOneX = points[i][0];  
>             int pointOneY = points[i][1];  
>             for (int j{i + 1}; j < n; ++j) {  
>                 int pointTwoX = points[j][0];  
>                 int pointTwoY = points[j][1];  
>                 int distance = abs(pointOneX - pointTwoX) + abs(pointOneY - pointTwoY);  
>                 // Edge = Node I to Node J  
>                 edges.push_back({distance, {i, j}});  
>             }  
>         }  
>  
>         // (2) Sort edges by cost  
>         sort(edges.begin(), edges.end());  
>  
>         // (3) Kruskal's Algorithm: Add edges while avoiding cycles  
>         UnionFind uf{n};  
>         int minCost{0};  
>  
>         for (int i{0}; i < edges.size(); ++i) {  
>             int firstNode = edges[i].second.first;  
>             int secondNode = edges[i].second.second;  
>  
>             if (uf.unite(firstNode, secondNode)) {  
>                 minCost += edges[i].first;  
>             }  
>         }  
>         return minCost;  
>     }  
> };  
> ```  
>  
> ~={green}**Key Points**=~:  
> * ~={blue}**Time Complexity**=~:  
>   - **$O(n^2 \log n)$**:  
>     - Generating edges: **$O(n^2)$** (all pairwise distances).  
>     - Sorting edges: **$O(n^2 \log n)$**.  
>     - Union-Find operations: **$O(n \log n)$**.  
> * ~={blue}**Space Complexity**=~:  
>   - **$O(n^2)$** for storing all edges.  
