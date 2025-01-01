> [!QUOTE] Quick Notes
> Traverse a tree level by level

# Overview
## Recipe

>[!Note]- BT BFS Recipe
> <!-- Multiline -->
>1. ~={purple}**Why BFS on Binary Tree**=~: If we want to traverse level by level

## Time Complexity

>[!Note]- Time Complexity of DFS on BT
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: We will visit every node on the tree once, hence, $O(n)$, where $n$ is the number of nodes in the tree
> * **~={purple}Space Complexity=~**: BFS uses a queue to traverse the tree level by level, so the space complexity depends on the maximum number of nodes stored in the queue at any time, which corresponds to the maximum width of the tree
> 	* **~={green}Balanced=~**:
> 		* In a balanced binary tree, the maximum number of nodes in a level (the width of the tree) is approximately $n/2$, occurring at the last level.
> 			* Number of nodes in a perfect binary tree, $n=2^{h+1}-1$
> 				* $h=log_2(n+1)-1$
> 			* Number of nodes in a layer is $2^h$, where h is the level
> 			* Hence $2^h = 2^{log_2(n+1)-1}=2^{log_2(n+1)}/2={n+1}/2$
> 		* However, since we are storing at most one level of nodes at a time, the space complexity is $O(n)$ for the queue in the worst case.
> 	* **~={red}Skewed=~**: In a skewed tree (all nodes are either left or right children), the queue at most holds one node at a time, so the space complexity is $O(1)$.

## Points of Interest

>[!Note]- DFS vs BFS Drawbacks
> <!-- Multiline -->
> * **~={purple}Case Dependent=~**: 
> 	* If we had a huge tree, and we were looking for a value in the right subtree, using DFS we would have to explore the entire left subtree first. This is avoided in BFS.
> 	* If we had a value at the bottom, BFS would take ages.

# Templates

>[!Info]- BT BFS Template (Iterative)
><!-- Multiline -->
><u>**Explanation**</u>
>
>Left priority BFS.
>
><u>**C++ Code**</u>
>```cpp
>void printAllNodes(TreeNode* root) {
>	queue<TreeNode*> queue;
>	queue.push(root);
>	
>	while (!queue.empty()) {
>		int nodesInCurrentLevel = queue.size();
>		
>		// Do Some Logic for the Current Level
>		// Here the queue contains every node on that level
>		
>		for (int i = 0; i < nodesInCurrentLevel; i++) {
>			TreeNode* node = queue.front();
>			queue.pop();
>			
>			// Do Some Logic on the Current Node in this Level
>			cout << node->val << endl;
>			
>			// Put the next level (children of current node) 
>			// onto the queue
>			if (node->left) queue.push(node->left);
>			if (node->right) queue.push(node->right);
>		}
>	}
>}
>```
><u>**Visual Explanation**</u>
> ![[Drawing 2024-12-31 15.38.46.excalidraw | center | 400]]

# Example Problems

> [!Question]- Binary Tree Right Side View
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.
>
>![[Pasted image 20241231183009.png | center | 300]]
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* We traverse level by level using BFS
>	* Hence, the last value in our queue, in every iteration, will be on the very right. Hence we store these values, and return it,
>
>```cpp
>vector<int​> rightSideView(TreeNode* root) {
>	// (1) If we pass in an empty tree, then return an empty array
>	vector<int​> result{};
>	if (root == nullptr) return result;
>	
>	// (2) Perform BFS
>	queue<TreeNode*​> queue;
>	queue.push(root);
>	
>	while (!queue.empty()) {
>		int queueSize = queue.size();
>		// If we are at the end of the queue, push it to result
>		result.push_back(queue.back()->val);
>		
>		for (int i{0}; i < queueSize; ++i) {
>			TreeNode* node = queue.front();
>			queue.pop();
>			
>			if (node->left) queue.push(node->left);
>			if (node->right) queue.push(node->right);
>		}
>	}
>	return result;
>}
>```

> [!Question]- Binary Tree Zigzag Level Order Traversal
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).
>
> ![[Pasted image 20241231183528.png | center | 250]]
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>
> ![[Drawing 2024-12-31 20.20.18.excalidraw | center | 600]]
>
>```cpp
>vector<vector<int​>> zigzagLevelOrder(TreeNode* root) {
>	// (1) If we pass in an empty tree, then return an empty array
>	vector<vector<int​>> result{};
>	if (root == nullptr) return result;
>	
>	// (2) Perform BFS
>	deque<TreeNode*> deque;
>	bool parity = true;
>	deque.push_back(root);
>	
>	while (!deque.empty()) {
>		int dequeSize = deque.size();
>		vector<int​> levelResult;
>		
>		for (int i = 0; i < dequeSize; ++i) {
>			TreeNode* node;
>			if (parity) {
>				// Process from the front (left-to-right)
>				node = deque.front();
>				deque.pop_front();
>				levelResult.push_back(node->val);
>				if (node->left) deque.push_back(node->left);
>				if (node->right) deque.push_back(node->right);
>			} else {
>				// Process from the back (right-to-left)
>				node = deque.back();
>				deque.pop_back();
>				levelResult.push_back(node->val);
>				if (node->right) deque.push_front(node->right);
>				if (node->left) deque.push_front(node->left);
>			}
>		}
>		result.push_back(levelResult);
>		// Flip the parity so we cycle between processing from
>		// front and back
>		parity = !parity;
>	}
>	return result;
>}
>```

#flashcards/dsa/patterns/binarytreebfs
