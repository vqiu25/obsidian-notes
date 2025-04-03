> [!quote]+ Overview
> <!-- Multiline -->
>````col 
>```col-md 
> ![[bst.gif]]
>``` 
>```col-md 
>**Everything** to the left of a node is smaller, **everything** to the right of a node is greater. Each node at most has 2 children. This makes something a Binary Search Tree. If there are duplicate values, be consistent as to where you place them.
>* The **~={red}min=~** value is the very left
>* The **~={green}max=~** value is the very right
>
>The complexity will be the height of the tree. Balanced $O(\log(n))$, unbalanced is $O(n)$.
>``` 
>```` 
>

# Complexity

> [!note]- Complexities of Unbalanced and Balanced BST
> <!-- Multiline -->
> ![[Drawing 2025-03-29 12.47.36.excalidraw | center | 700]]

> [!note]- Are BST's Ordered?
> <!-- Multiline -->
> Yes they are. When iterating through a `map`, we are performing an inorder traversal on the BST.

> [!note]- How does a Self Balancing BST Stay Balanced
> <!-- Multiline -->
> **~={purple}Complexity=~**: $O(1)$
> **~={purple}Description=~**: When a BST becomes unbalanced, we may have to perform 1 or 2 rotations to make it self-balance. This is dependent on where we insert our value.
> * **~={green}Left of a left child=~**: 1 right rotation
> * **~={green}Right of a right child=~**: 1 left rotation
> * **~={green}Right of a left child=~**: Left then right rotation
> * **~={green}Left of a right child=~**: Right then left rotation
> 
> **~={red}Example=~**
> * **~={green}AVL Tree=~**: An AVL tree keeps the tree height balanced. Which means that the left and right subtree of any node differ by a height at most of 1.
> 
> ![[Drawing 2025-03-29 13.30.59.excalidraw | center | 500]]

https://how.dev/answers/common-avl-rotation-techniques

#flashcards/dsa/ds/bst