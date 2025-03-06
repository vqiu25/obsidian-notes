> [!Note]- Trie (Prefix Tree) Implementation
> <!-- Multiline -->
> **~={red}Question=~**:
> - Implement a **Trie** data structure with the following methods:
>   - `insert(word)`: Inserts a word into the trie.
>   - `search(word)`: Returns `true` if the word is in the trie (`endOfWord == true`), else `false`.
>   - `startsWith(prefix)`: Returns `true` if there is any word in the trie that starts with the given prefix.
>
> ~={green}**Solution Overview**=~:
> 1. ~={blue}**Trie Structure**=~:
>    - The trie consists of nodes (`TrieNode`), where each node:
>      - Has a `children` map that links characters to the corresponding child node.
>      - Tracks if it is the `endOfWord`.
>
> 1. ~={blue}**Key Operations**=~:
>    - ~={red}**Insert**=~:
>      - Iterate through each character in the word.
>      - For each character, check if it exists in the current node's `children`.
>      - If not, create a new `TrieNode` using `make_unique`.
>      - Move the pointer to the child node after processing each character.
>      - Mark the last node's `endOfWord` as `true`.
> 
>    - **~={red}Search=~**:
>      - Iterate through each character in the word.
>      - If a character does not exist in `children`, return `false`.
>      - After processing all characters, return `true` only if the last node is marked `endOfWord`.
>
>    - **~={red}startsWith=~**:
>      - Similar to `search`, but return `true` after processing the prefix, regardless of `endOfWord`.
>
> ~={blue}**Code Implementation**=~:
> ```cpp
> class Trie {
> private:
>     class TrieNode {
>     public:
>         unordered_map<char, unique_ptr<TrieNode​>> children;
>         bool endOfWord;
>         TrieNode() : endOfWord(false) {}
>     };
>
>     unique_ptr<TrieNode​> root;
>
> public:
>     Trie() : root(make_unique<TrieNode​>()) {}
>     
>     void insert(string word) {
>         TrieNode* currNode = root.get();
>         for (char letter : word) {
>             if (currNode->children.find(letter) == currNode->children.end()) {
>                 currNode->children[letter] = make_unique<TrieNode​>();
>             }
>             currNode = currNode->children[letter].get();
>         }
>         currNode->endOfWord = true;
>     }
>     
>     bool search(string word) {
>         TrieNode* currNode = root.get();
>         for (char letter : word) {
>             if (currNode->children.find(letter) == currNode->children.end()) return false;
>             currNode = currNode->children[letter].get();
>         }
>         return currNode->endOfWord;
>     }
>     
>     bool startsWith(string prefix) {
>         TrieNode* currNode = root.get();
>         for (char letter : prefix) {
>             if (currNode->children.find(letter) == currNode->children.end()) return false;
>             currNode = currNode->children[letter].get();
>         }
>         return true;
>     }
> };
> ```
>
> ~={blue}**Complexity Analysis**=~:
> - ~={green}**Time Complexity**=~:
>   - `insert(word)`: $O(n)$ per word, where $n$ is the length of the word.
>   - `search(word)`: $O(n)$ for searching each character sequentially.
>   - `startsWith(prefix)`: $O(n)$, similar to search but without the `endOfWord` check at the end.
> - ~={green}**Space Complexity**=~:
>   - $O(n \times m)$, where $n$ is the total number of words inserted and $m$ is the average length of each word. Each node can have multiple children.
>
> ~={blue}**Key Insights**=~:
> - Tries provide efficient prefix-based operations compared to hash maps or binary search trees.
> - Using `unique_ptr` reduces memory management overhead, ensuring proper cleanup when nodes are no longer needed.

> [!Note]- DFS on Trie: MapSum (Prefix Key-Value Sum)
> <!-- Multiline -->
> **~={red}Question=~**:
> - Design a data structure that supports two operations:
>   1. `insert(key, val)`: Insert a key-value pair into the trie.
>   2. `sum(prefix)`: Return the sum of all values of keys that start with the given prefix.
>
> ~={green}**Solution Overview**=~:
> 1. ~={blue}**Trie Structure**=~:
>    - Each node (`MapSumNode`) contains:
>      - A `children` map linking characters to child nodes.
>      - A `value` representing the value of the word ending at that node.
>      - A `bool endOfWord` to mark valid keys.
>
> 1. ~={blue}**Key Operations**=~:
>    - ~={red}**Insert**=~:
>      - Traverse the trie character by character.
>      - Create new nodes if the character does not exist in `children`.
>      - After reaching the end, mark `endOfWord` as `true` and set the node’s `value`.
>
>    - ~={red}**Sum**=~:
>      - Traverse the trie to the node representing the prefix.
>      - Perform a **DFS** from that node, summing the `value` of all reachable nodes with `endOfWord == true`.
>      - ~={red}**DFS Logic**=~:
>        - Use a stack (iterative DFS) as the trie is acyclic.
>        - Push each child node onto the stack for exploration.
>        - Accumulate values at nodes where `endOfWord == true`.
>
> ~={green}**Code Implementation**=~:
> ```cpp
> class MapSum {
> private:
>     class MapSumNode {
>     public:
>         unordered_map<char, unique_ptr<MapSumNode​>> children;
>         bool endOfWord;
>         int value;
>         MapSumNode() : endOfWord(false), value(0) {}
>     };
>
>     unique_ptr<MapSumNode​> root;
>
> public:
>     MapSum() : root(make_unique<MapSumNode​>()) {}
>     
>     void insert(string key, int val) {
>         MapSumNode* currNode = root.get();
>         for (char letter : key) {
>             if (currNode->children.find(letter) == currNode->children.end()) {
>                 currNode->children[letter] = make_unique<MapSumNode​>();
>             }
>             currNode = currNode->children[letter].get();
>         }
>         currNode->endOfWord = true;
>         currNode->value = val;
>     }
>     
>     int sum(string prefix) {
>         MapSumNode* currNode = root.get();
>         for (char letter : prefix) {
>             if (currNode->children.find(letter) == currNode->children.end()) return 0;
>             currNode = currNode->children[letter].get();
>         }
>         
>         int totalSum = 0;
>         deque<MapSumNode*> stack;
>         stack.push_back(currNode);
>         
>         // DFS Traversal of the Trie from Prefix Node
>         while (!stack.empty()) {
>             MapSumNode* topNode = stack.back();
>             stack.pop_back();
>
>             if (topNode->endOfWord) totalSum += topNode->value;
>
>             for (auto& pair : topNode->children) {
>                 stack.push_back(pair.second.get());
>             }
>         }
>         return totalSum;
>     }
> };
> ```
>
> ~={blue}**Complexity Analysis**=~:
> - ~={green}**Time Complexity**=~:
>   - `insert(key, val)`: $O(n)$, where $n$ is the length of the key.
>   - `sum(prefix)`: $O(m + k)$, where $m$ is the length of the prefix and $k$ is the number of nodes explored in the DFS.
>
> - ~={green}**Space Complexity**=~:
>   - $O(n \times m)$ for storing all inserted keys, where $n$ is the number of keys and $m$ is the average key length.
>
> ~={blue}**Key Insights**=~:
> - DFS is leveraged to sum values of all keys under a given prefix node.
> - Since tries represent unique prefixes, DFS ensures complete exploration without cycles.
> - Using `unique_ptr` simplifies memory management and prevents leaks.
