> [!QUOTE] Quick Notes
> Traverse a tree

# Overview
## Recipe

>[!Note]- BT DFS Recipe
> <!-- Multiline -->
>1. ~={purple}**Why DFS on Binary Tree**=~:
> * **~={green}Subcomponents Exist=~**: A window can be used to slide across the data structure unidirectionally, where subcomponents can be processed.

## Time Complexity

>[!Note]- Time Complexity of DFS on BT
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: We will visit every node on the tree once, hence, $O(n)$, where $n$ is the number of nodes in the tree
> * **~={purple}Space Complexity=~**:
> 	* **~={green}Balanced=~**: The depth of the tree would be $O(logn)$, hence the recursion depth/stack will be $O(logn)$
> 		* This is because the number of nodes in a BST is $n=2^h-1$, where $h$ is the height of the tree.
> 		* Thus, the depth/height will be $n+1=2^h$, which will give $log(n+1)=hlog(2)$, giving $h=1/(log2)\cdot log(n+1)$ which is $O(logn)$
> 	* **~={red}Skewed=~**: The depth of the tree is $O(n)$ in the worst case

## Points of Interest

>[!Note]- BT Terminology
> <!-- Multiline -->
> **~={purple}Diagram=~**
> 
> ![[Drawing 2024-12-27 23.28.01.excalidraw | center | 350]]
> 
> **~={purple}Properties=~**
> * **~={green}Height=~**: The **height of the subtree rooted at that node**, which is the number of edges to the furthest leaf below it.
> * **~={green}Diameter=~**: 
> 	* **Length** of the longest path between _any two nodes within the tree_.
> 	* The maximum among the sums of the left height and right height of the nodes in the tree.

>[!Note]- How Recursion Stack Works (1 Call)
> <!-- Multiline -->
>For the following function:
>* **~={blue}Blue=~** = Function Call
>* **~={red}Red=~** = Return
>````col
>```col-md
>```cpp
>void fn(i) {
>	// Base Case
>	if i > 3 {
>		// (1) Return 1
>		return;
>	}
>	
>	print(i)
>	
>	// Recursive Case
>	fn(i + 1);
>	
>	print(End of call i)
>	
>	// (2) Return 2
>	return;
>}
>```
>```col-md
> ![[Drawing 2024-12-27 23.30.18.excalidraw | center | 150]]
>```
>````

>[!Note]- How Recursion Stack Works (2 Calls)
> <!-- Multiline -->
>For the following function:
>* **~={blue}Blue=~** = First Function Call
>* **~={green}Green=~** = Second Function Call
>* **~={red}Red=~** = Return
>````col
>```col-md
>```cpp
>int fn(n) {
>	// Base Case
>	if n <= 1 {
>		// (1) Return 1
>		return n;
>	}
>	// Recursive Case
>	oneBack = fn(n - 1);
>	twoBack fn(n - 2);
>	
>	// (2) Return 2
>	return (oneBack + twoBack)
>}
>```
>```col-md
> ![[Drawing 2024-12-27 23.51.59.excalidraw | center]]
>```
>````

>[!Note]- Preorder vs Inorder vs Postorder Traversal
> <!-- Multiline -->
> For the following tree:
> 
>  ![[Drawing 2024-12-28 08.59.14.excalidraw | center | 200]]
> 
> ````col
>```col-md
>**~={purple}Preorder=~**
>* Logic is done on the current node before moving to the children
>* Root -> Left -> Right
>* **~={green}Order=~**: `0, 1, 3, 4, 2`
> 
> ![[Drawing 2024-12-28 08.41.56.excalidraw]]
>```cpp
>void dfs(Node* node)
>  if (node == null) {
>	return;
>  }
>  // PRE
>  print(node->val);	
>  dfs(node->left);
>  dfs(node->right);
>  return;
>}
>```
>```col-md
>**~={purple}Inorder=~**
>* First recursively call the left child, then perform logic on the current node, and then recursively call right
>* No logic/printing will occur until we reach a node without a left child / a visited left child
>* Left -> Root -> Right
>* Order: `3, 1, 4, 0, 2`
> 
> ![[Drawing 2024-12-28 08.41.56.excalidraw]]
> 
> 
>```cpp
>void dfs(Node* node)
>  if (node == null) {
>	return;
>  }
>	
>  dfs(node->left);
>  // IN
>  print(node->val);
>  dfs(node->right);
>  return;
>}
>```
>```col-md
>**~={purple}Postorder=~**
>* Recursively call on the children first and then perform logic on the current node
>* No logic/printing will occur until we reach a leaf node
>* Left -> Right -> Root
>* **~={green}Order=~**: `3, 4, 1, 2, 0`
> 
> ![[Drawing 2024-12-28 08.41.56.excalidraw]]
>```cpp
>void dfs(Node* node)
>  if (node == null) {
>	return;
>  }
>	
>  dfs(node->left);
>  dfs(node->right);
>  // POST
>  print(node->val);
>  return;
>}
>```
>````

>[!Note]- How to think about recursion
> <!-- Multiline -->
> * Think about recursion, where at any node $x$, that is a state 
> * To avoid an internal stack overflow in your head, think about the states that have at least either the left or right side be `nullptr` such that it reaches a base case
> * Consider the following cases:
> 	* A node has 2 nullptr as children
> 		* (`(if (node == nullptr)`)
> 	* A node has 1 nullptr as a child 
> 		* (`if (node->left == nullptr || node->right == nullptr)`)
> 	* A node has 0 nullptr as children
> 		* `return...`

>[!Note]- What cases to think about
> <!-- Multiline -->
> ````col
>```col-md
>**~={blue}2 Child=~**
>* Think how we go from middle parent to left child to right child to upper parent
>
> ![[Drawing 2024-12-30 12.36.33.excalidraw | center | 200]]
>```
>```col-md
>**~={blue}1 Child=~**
>* Think how we go from middle parent to left child to upper parent
>
> ![[Drawing 2024-12-30 12.37.01.excalidraw | center | 200]]
>```
>````

# Templates

>[!Info]- BT DFS Template
><!-- Multiline -->
><u>**Explanation**</u>
>
>Left priority DFS:
>1. **~={blue}Base Case=~**: If the current node is a leaf (no children), return.
>2. **~={blue}Recursive Case=~**:
>* **~={red}dfs(node->left)=~**: First, traverse the left subtree. If a leaf node is reached, backtrack.
>* **~={green}dfs(node->right)=~**: Next, traverse the right subtree after completing the left.
>
><u>**C++ Code**</u>
>```cpp
>void dfs(TreeNode* node) {
>	if (node == nullptr) {
>		return;
>	}
>	
>	dfs(node->left);
>	dfs(node->right);
>	return;
>}
>```
><u>**Visual Explanation**</u>
>* **~={blue}Blue=~** = First Function Call (Traverse Left)
>* **~={green}Green=~** = Second Function Call (Traverse Right)
>* **~={red}Red=~** = Return
>
> ![[Drawing 2024-12-28 00.08.32.excalidraw | center | 600]]

# Example Problems

> [!Question]- Maximum Depth of Binary Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the `root` of a binary tree, return _its maximum depth_.
>* A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.
>
>**~={red}Solution=~**:
>* **~={purple}Base Case=~**: If we reach a leaf node, return a depth of 0
>* **~={purple}Recursive Case=~**:
>	* For each other node, compute it's left and right depth recursively, and take the max
>	* The +1 is added at each level of the recursion to count the current node. This ensures that the depth accumulates as we move up the tree from the leaves back to the root. (Also why we return 0 as the base case, as that doesn't trinkle through/up)
>
> ![[Drawing 2024-12-28 12.57.25.excalidraw | 500 | center]]
>
>```cpp
>int maxDepth(TreeNode* root) {
>	if (root == nullptr) return 0;
>	
>	int leftDepth = maxDepth(root->left);
>	int rightDepth = maxDepth(root->right);
>	
>	return max(leftDepth, rightDepth) + 1;
>}
>```

> [!Question]- Minimum Depth of Binary Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a binary tree, find its minimum depth.
>* The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
>
>**~={red}Solution=~**:
>Recursion intuition comes from the fact that given any subtree, from the perspective of the root, the min depth of the subtree is 1 + the min depth of the left or right side (with the 1 child exception).
>* **~={purple}Base Case=~**: If we reach a leaf node, return a depth of 0
>* **~={purple}Recursive Case=~**:
>	* **~={red}If a node has 1 child=~**: The side with no children, will automatically return 0, making the min that side. But there are no nodes there, hence we include another case, to take the maximum of the left and right side, should that be the case.
>	* **~={green}If a node has 2 children=~**: Take the min depth from the left and right side
>
> ![[Drawing 2024-12-29 21.09.19.excalidraw | center | 650]]
>
>```cpp
>int minDepth(TreeNode* root) {
>	// Base Case
>	if (root == nullptr) return 0;
>	
>	int leftDepth = minDepth(root->left);
>	int rightDepth = minDepth(root->right);
>	
>	// Recursive Case (2 options)
>	// If a node has only 1 child
>	if (root->left == nullptr || root→>right = nullptr) {
>		return max(leftDepth, rightDepth) + 1;
>	}
>	
>	// If node has both children
>	return max(leftDepth, rightDepth) + 1;
>}
>```

> [!Question]- Invert Binary Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given the root of a binary tree `root`. Invert the binary tree and return its root.
>
> ![[Pasted image 20241230120855.png | center | 400]]
>
>**~={red}Solution=~**:
>* **~={purple}Base Case=~**: If we reach a leaf node, return nullptr
>* **~={purple}Recursive Case=~**:
>	* Once we've traversed the left and right tree, we return to the parent node. At that parent node, we need to swap it's left and right child.
>	* We then return the parent node to continue the recursion, passing it on as an upper parent's left or child child.
>
>```cpp
>TreeNode* invertTree(TreeNode* root) {
>	if (root == nullptr) return nullptr;
>	
>	TreeNode* leftTree = invertTree(root->left);
>	TreeNode* rightTree = invertTree(root->right);
>	
>	swap(root->left, root->right);
>	
>	return root;
>}
>```

## Use of "Global Variable"

> [!Question]- Diameter of Binary Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* The diameter of a binary tree is defined as the length of the longest path between any two nodes within the tree. The path does not necessarily have to pass through the root.
>
> ![[Drawing 2024-12-30 14.25.17.excalidraw | center | 400]]
> 
>**~={red}Solution=~**:
>First note that the diameter is the sum of the max left depth + max right depth, meaning:
> 
> ![[Drawing 2024-12-30 14.32.16.excalidraw | center | 200]]
> 
>* **~={purple}Base Case=~**: If we reach a leaf node, return 0
>* **~={purple}Recursive Case=~**:
>	* Once we've traversed the left and right tree, we return to the parent node. At that parent node, we calculate the diameter as the sum of the left and right subtree heights and update the current maximum diameter (`currMax`) if necessary.
>	* We then return the height (which is the max of the left and right depth - same as max depth question) of the current node, which is `1 + max(leftHeight, rightHeight)`, to the parent node for further calculations.
>
>```cpp
>int diameterOfBinaryTreeHelper(TreeNode* root, int& currMax) {
>	if (root == nullptr) return 0;
>	
>	int leftHeight = diameterOfBinaryTreeHelper(root->left, currMax);
>	int rightHeight = diameterOfBinaryTreeHelper(root->right, currMax);
>	
>	currMax = max(currMax, leftHeight + rightHeight);
>	
>	return 1 + max(leftHeight, rightHeight);
>}
>
>int diameterOfBinaryTree(TreeNode* root) {
>	int currMax = 0;
>	diameterOfBinaryTreeHelper(root, currMax);
>	return currMax;
>}
>```

> [!Question]- Diameter of Binary Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* The diameter of a binary tree is defined as the length of the longest path between any two nodes within the tree. The path does not necessarily have to pass through the root.
>
> ![[Drawing 2024-12-30 14.25.17.excalidraw | center | 400]]
> 
>**~={red}Solution=~**:
>First note that the diameter is the sum of the max left depth + max right depth, meaning:
> 
> ![[Drawing 2024-12-30 14.32.16.excalidraw | center | 200]]
> 
>* **~={purple}Base Case=~**: If we reach a leaf node, return 0
>* **~={purple}Recursive Case=~**:
>	* Once we've traversed the left and right tree, we return to the parent node. At that parent node, we calculate the diameter as the sum of the left and right subtree heights and update the current maximum diameter (`currMax`) if necessary.
>	* We then return the height (which is the max of the left and right depth - same as max depth question) of the current node, which is `1 + max(leftHeight, rightHeight)`, to the parent node for further calculations.
>
>```cpp
>int diameterOfBinaryTreeHelper(TreeNode* root, int& currMax) {
>	if (root == nullptr) return 0;
>	
>	int leftHeight = diameterOfBinaryTreeHelper(root->left, currMax);
>	int rightHeight = diameterOfBinaryTreeHelper(root->right, currMax);
>	
>	currMax = max(currMax, leftHeight + rightHeight);
>	
>	return 1 + max(leftHeight, rightHeight);
>}
>
>int diameterOfBinaryTree(TreeNode* root) {
>	int currMax = 0;
>	diameterOfBinaryTreeHelper(root, currMax);
>	return currMax;
>}
>```

> [!Question]- Balanced Binary Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* The diameter of a binary tree is defined as the length of the longest path between any two nodes within the tree. The path does not necessarily have to pass through the root.
>
> ![[Drawing 2024-12-30 14.25.17.excalidraw | center | 400]]
> 
>**~={red}Solution=~**:
>First note that the diameter is the sum of the max left depth + max right depth, meaning:
> 
> ![[Drawing 2024-12-30 14.32.16.excalidraw | center | 200]]
> 
>* **~={purple}Base Case=~**: If we reach a leaf node, return 0
>* **~={purple}Recursive Case=~**:
>	* Once we've traversed the left and right tree, we return to the parent node. At that parent node, we calculate whether there is a difference greater than 1 at the left and right subtree.
>	* We then return the height (which is the max of the left and right depth - same as max depth question) of the current node, which is `1 + max(leftHeight, rightHeight)`, to the parent node for further calculations.
>
>```cpp
>int isBalancedHelper(TreeNode* root, bool& balanced) {
>	if (root == nullptr) return 0;
>	
>	int leftHeight = isBalancedHelper(root->left, balanced);
>	int rightHeight = isBalancedHelper(root->right, balanced);
>	
>	if (abs(leftHeight - rightHeight) > 1) balanced = false;
>	
>	return 1 + max(leftHeight, rightHeight);
>}
>
>bool isBalanced(TreeNode* root) {
>	bool balanced = true;
>	diameterOfBinaryTreeHelper(root, currMax);
>	return balanced;
>}
>```

#flashcards/dsa/patterns/binarytreedfs
