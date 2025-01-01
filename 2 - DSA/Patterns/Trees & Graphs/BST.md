> [!QUOTE] Quick Notes
> Traverse a tree level by level

# Overview

## Time Complexity

>[!Note]- Time Complexity of DFS on BT
> <!-- Multiline -->
> * Big O is the same, but it can be better by a constant factor, which can be significant on a larger scale.

## Points of Interest

>[!Note]- Properties of BST
> <!-- Multiline -->
> * **~={purple}Unique=~**: All values in BST are unique
> * **~={purple}Case Dependent=~**: 
> 	* If we had a huge tree, and we were looking for a value in the right subtree, using DFS we would have to explore the entire left subtree first. This is avoided in BFS.
> 	* If we had a value at the bottom, BFS would take ages.

>[!Note]- Inorder Traversal Property
> <!-- Multiline -->
> * Inorder traversal will visit the nodes in ascending order

# Templates

>[!Info]- BST DFS Template (Recursive)
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

> [!Question]- Minimum Absolute Difference in BST
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the `root` of a binary search tree (BST), return the minimum absolute difference between the values of any two nodes in the tree.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* The inorder traversal of a BST gives the values of the nodes in a sorted order.
>	* By traversing the BST in this way, we can store the node values in an array.
>	* The minimum difference between any two nodes can be found by iterating through this array and calculating the difference between consecutive elements.
>
> ![[Drawing 2025-01-01 16.22.24.excalidraw | center | 500]]
>
>```cpp
>public:
>    void dfs(TreeNode* root, vector<int​>& array) {
>        if (root == nullptr) return;
>
>        dfs(root->left, array);
>        // Inorder traversal
>        array.push_back(root->val);
>        dfs(root->right, array);
>        return;
>    }
>
>    int getMinimumDifference(TreeNode* root) {
>        vector<int​> array{};
>        dfs(root, array);
>        int minDiff{INT_MAX};
>
>        // Look for consecutive smallest
>        for (int i{0}; i < array.size() - 1; ++i) {
>            minDiff = min(minDiff, array[i + 1] - array[i]);
>        }
>        return minDiff;
>    }
>```

> [!Question]- Insert into a Binary Search Tree
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given the `root` node of a binary search tree (BST) and a value to insert into the tree. Insert the value into the BST and return its root. The BST must remain valid after the insertion.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* Use a helper function to navigate the tree and locate the correct position for the new value.
>	* Insert the new value as a leaf node, ensuring that the BST property is maintained.
>		* **~={green}Traverse=~**: Maintain a parent and a child pointer. Traverse:
>			* **~={blue}Left=~**: If our value is is smaller than the current node
>			* **~={blue}Right=~**: If our value is greater than the current node
>		* **~={red}Set=~**: Once we've found the parent that we want to put the node under, check whether the value is larger or smaller than the parent.
>			* **~={blue}Left=~**: If smaller
>			* **~={blue}Right=~**: If larger
>	* Pass the root by reference such that if we have an empty tree, we can set the root from the helper function (otherwise it would be a local change)
>
> ![[Drawing 2025-01-01 16.30.30.excalidraw | center | 700]]
>
>```cpp
>public:
>    void add(TreeNode*& root, int val) {
>        if (root == nullptr) {
>            root = new TreeNode{val};
>            return;
>        }
>		
>		// Traverse
>        TreeNode* parent = nullptr;
>        TreeNode* node = root;
>        while (node != nullptr) {
>            parent = node;
>            if (val > parent->val) {
>                node = node->right;
>            } else {
>                node = node->left;
>            }
>        }
>
>        // Set
>        TreeNode* newNode = new TreeNode{val};
>        if (val > parent->val) {
>            parent->right = newNode;
>        } else {
>            parent->left = newNode;
>        }
>    }
>
>    TreeNode* insertIntoBST(TreeNode* root, int val) {
>        add(root, val);
>        return root;
>    }
>```

> [!Question]- Closest Value in a BST
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the `root` of a binary search tree (BST) and a target value, find the value in the BST that is closest to the target.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* The goal is to traverse the BST and find the node whose value is closest to the target.
>	* Use the BST property to decide whether to move left or right during traversal:
>		* **~={green}Left=~**: If the target is less than or equal to the current node's value, move left.
>		* **~={red}Right=~**: If the target is greater than the current node's value, move right.
>	* Keep track of the closest value encountered during traversal.
>
> ![[Drawing 2025-01-01 16.41.17.excalidraw | center | 400]]
>
>```cpp
>int closestValue(TreeNode* root, double target) {
>    int newTarget = round(target);
>    if (target - floor(target) == 0.5) newTarget -= 1;
>
>    int closest = root->val;
>    TreeNode* node = root;
>
>    while (node != nullptr) {
> 	   // Update closest value
>        if (abs(node->val - newTarget) < abs(closest - newTarget)) {
> 	       closest = node->val;
>        } 
>	
>        // Traverse left or right depending on if the target value
>        // is less than or greater than the current node's value
>        if (newTarget > node->val) {
>            node = node->right;
>        } else {
>            node = node->left;
>        }
>    }
>    return closest;
>}
>```

> [!Question]- Range Sum of BST
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given the `root` of a binary search tree (BST), and two integers `low` and `high`, return the sum of values of all nodes with a value in the inclusive range `[low, high]`.
>
>**~={red}Solution=~**:
>* **~={purple}Intuition=~**:
>	* Use a bottom-up approach to traverse the tree:
>		* Calculate the sum for the left and right subtrees first.
>		* Combine the results with the current node's value only if it lies within `[low, high]`.
>	* This ensures that the computation happens after exploring the subtrees.
>
> ![[Drawing 2025-01-01 16.57.53.excalidraw | center | 600]]
>
>```cpp
>int rangeSumBST(TreeNode* root, int low, int high) {
>    if (root == nullptr) return 0;
>
>    // Calculate the sum for the left and right subtrees
>    int leftSum = rangeSumBST(root->left, low, high);
>    int rightSum = rangeSumBST(root->right, low, high);
>
>    // Combine the results with the current node's value if it's in range
>    if (low <= root->val && root->val <= high) {
>        return leftSum + rightSum + root->val;
>    } else {
>        return leftSum + rightSum;
>    }
>}
>```

#flashcards/dsa/patterns/binarysearchtree
