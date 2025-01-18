> [!QUOTE] Quick Notes
> When we need to explore all possible solutions to a problem

# Overview
## Recipe

>[!Note]- Backtracking Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Backtracking**=~: When we need to explore all possible solutions to a problem

## Time Complexity

>[!Note]- Time Complexity of Backtracking
> <!-- Multiline -->
>

## Points of Interest

>[!Note]- Backtracking Traversal Process?
> <!-- Multiline -->
>We use Recursive DFS

>[!Note]- When do we introduce an `index` variable?
> <!-- Multiline -->
>For problems where we don't want to visit every combination, i.e. for something like `1, 2, 3, 4`:
>
>* **~={blue}From=~** $1$: Visit 2, 3, 4
>* **~={blue}From=~** $2$: Visit 3, 4
>* **~={blue}From=~** $4$: Visit n/a
>
>Rather than have everything visit `1, 2, 3, 4`

# Templates

>[!Info]- Backtracking Template
><!-- Multiline -->
><u>**Explanation**</u>
>1. **~={purple}Base Case=~**: Identify the condition at which the recursion should stop. This is typically when the current solution (`curr`) is complete or invalid. At this point, update the result or increment the answer.
>2. **~={purple}Explore Options=~**:
>   - Iterate over the possible choices at the current state.
>   - Modify the current solution (`curr`) to reflect the choice being made.
>   - Recursively call the backtracking function to explore further with the modified state.
>3. **~={purple}Backtrack=~**:
>   - Undo the modification to `curr` to restore the previous state. This ensures no side effects remain for the next iteration.
>
><u>**C++ Code**</u>
>```cpp
>void backtrack(vector<int​>& curr, vector<int​>& input, vector<vector<int​>>& result) {
>    // (1) Base Case
>    if (BASE_CASE_CONDITION) {
>        result.push_back(curr); // Add the solution to the result
>        return;
>    }
>
>    // (2) Explore Options
>    for (int i = 0; i < input.size(); ++i) {
>        curr.push_back(input[i]); // Modify curr to include the current choice
>        backtrack(curr, input, result); // Recur to explore further
>        curr.pop_back(); // (3) Undo the modification (Backtrack)
>    }
>}
>```

# Example Problems

## Foundational

> [!Question]- Permutations
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an array nums of distinct integers, return all the possible permutations . You can return the answer in any order.
>
>**~={red}Solution=~**:
>
>**~={blue}Permute=~**
>* **~={green}CurrState=~**: Stores the current permutation as it's being built.
>* **~={green}Used=~**: A boolean vector that tracks whether each element in `nums` has been added to
>* **~={green}Result=~**: A collection of all permutations.
>
>```cpp
>vector<vector<int​>> permute(vector<int​>& nums) {
>	vector<vector<int​>> result;
>	vector<int​> currState;
>	vector<bool​> used(nums.size(), false);
>	backtrack(currState, used, nums, result);
>	return result;
>}
>```
>
>**~={blue}Backtrack=~**
>
>![[perms.png | center | 450]]
>
>1. **~={green}Base Case=~**: A permutation must be the same length as the original "string". As such, our leaf nodes represent a valid permutation. When the size of `currState` equals `nums.size()`, a complete permutation is formed. Add it to `result`.
>2. **~={green}Recursive Case=~**:
>* Iterate through all indices in `nums`
>* Skip indices where `used[i]` is `true`.
>* Otherwise, mark the number as used, add it to `currState`, and recursively call `backtrack`.
>* After recursion, backtrack by resetting `used[i]` to `false` and removing the last element from `currState`. **~={red}Note that when we POP, we aren't necessarily exiting a function call.=~** For each function call, we iterate from $i=0,...nums.length()$.
>
>```cpp
>void backtrack(vector<int​>& currState, vector<int​>& nums,
>			   vector<bool​>& used,
>			   vector<vector<int​>>& result) {
>			   
>	// Base Case
>	if (currState.size() == nums.size()) {
>		result.push_back(currState);
>		return;
>	}
>	
>	// Recursive Case
>	for (int i{0}; i < nums.size(); ++i) {
>		if (used[i] == false) {
>			currState.push_back(nums[i]);
>			used[i] = true;
>			
>			backtrack(currState, nums, used, result);
>			
>			currState[i] = false;
>			currState.pop_back();
>		}
>	}
>}
>```
>
>**~={blue}Visualisation=~**
>
> ![[Drawing 2025-01-11 20.41.56.excalidraw | center | 700]]
>
>**~={blue}Complexity=~**
>* **~={green}Time Complexity=~**: $O(n \cdot n!)$. There are $n!$ permutations, and for each permutation and it takes roughly $O(n)$ per permutation.
>* **~={green}Space Complexity=~**: $O(n)$ for the recursion stack and `used` vector

> [!Question]- Subsets
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given an integer array `nums` of unique elements, return all subsets in any order without duplicates.
>* For example, given `nums = [1, 2, 3]`, return `[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`
>
>**~={red}Solution=~**:
>
> ![[Drawing 2025-01-07 23.08.13.excalidraw | center | 400]]
>
>**~={blue}Subsets=~**
>
>* **~={green}CurrState=~**: Stores the current subset as it's being built.
>* **~={green}Result=~**: A collection of all subsets.
>* **~={green}Start Index=~**: Tracks the starting index to ensure subsets are built sequentially without duplicates.
>
>```cpp
>vector<vector<int​>> subsets(vector<int​>& nums) {
>	vector<vector<int​>> result;
>	vector<int​> currState;
>	int startIndex{0};
>	backtrack(currState, nums, result, startIndex);
>	return result;
>}
>```
>
>**~={blue}Backtrack=~**
>
>1. **~={green}Base Case=~**: **~={red}This is actually never called as the function just ends.=~**
>2. **~={green}Recursive Case=~**:
>* Start with an empty `currState`.
>* Use recursion (backtracking) to explore subsets:
>* Add `currState` to `result` at every recursion step.
>* Backtrack by removing the last element added to `currState`.
>
>```cpp
>void backtrack(vector<int​>& currState, vector<int​>& nums,
>			   vector<vector<int​>>& result, int startIndex) {
>	
>	// Base Case
>	if (startIndex > nums.size()) return;
>	
>	// Recursive Case
>	result.push_back(currState);
>	for (int i{startIndex}; i < nums.size(); ++i) {
>		currState.push_back(nums[i]);
>		backtrack(currState, nums, result, i + 1);
>		currState.pop_back();
>	}
>}
>```
>**~={blue}Visualisation=~**
>
> ![[Drawing 2025-01-11 18.24.13.excalidraw | center | 700]]
>
>**~={blue}Complexity=~**
>* **~={green}Time Complexity=~**: $O(n \cdot 2^n)$. There are $2^n$ subsets, and for each subset, we copy `currState` to `result`, which is at worst size $n$.
>* **~={green}Space Complexity=~**: $O(n)$ for the recursion stack and `currState` vector

> [!Question]- Combinations
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers chosen from the range_ `[1, n]`.
>* You may return the answer in **any order**.
>
>**~={red}Solution=~**:
>
>**~={blue}Combinations=~**
>
>* **~={green}CurrState=~**: Stores the current combination as it's being built.
>* **~={green}Result=~**: A collection of all combinations.
>* **~={green}Start Index=~**: Tracks the starting index to ensure combinations are built sequentially without duplicates.
>
>```cpp
>vector<vector<int​>> combinations(int n, int k) {
>	vector<vector<int​>> result;
>	vector<int​> currState;
>	int startIndex{0};
>	vector<int​> nums;
>	for (int i{1}; i <= n; ++i) {
>		nums.push_back(i);
>	}
>	backtrack(currState, nums, k, result, startIndex);
>	return result;
>}
>```
>
>**~={blue}Backtrack=~**
>
>* Basically, we only retrieve the subsets that are size $k$
>
>1. **~={green}Base Case=~**: **~={red}This is actually never called as the function just ends.=~**
>2. **~={green}Recursive Case=~**:
>* Start with an empty `currState`.
>* Use recursion (backtracking) to explore combinations:
>* Add `currState` to `result` at every recursion step.
>* Backtrack by removing the last element added to `currState`.
>
>```cpp
>void backtrack(vector<int​>& currState, vector<int​>& nums, int k,
>			   vector<vector<int​>>& result, int startIndex) {
>	
>	// Base Case
>	if (currState.size() == k) {
>		result.push_back(currState);
>		return;
>	}
>	
>	// Recursive Case
>	for (int i{startIndex}; i < nums.size(); ++i) {
>		currState.push_back(nums[i]);
>		// We pass in i + 1 so that the current value can't be reused
>		backtrack(currState, nums, result, i + 1);
>		currState.pop_back();
>	}
>}
>```
>**~={blue}Visualisation=~**
>
> ![[Drawing 2025-01-11 22.11.20.excalidraw | center | 700]]
>
>**~={blue}Complexity=~**
>* **~={green}Time Complexity=~**: $O(n \cdot 2^n)$. There are $n!/{((n-k)!k!)}$ combinations, where we are copying the $k$ sized combination into our result array, hence $k \cdot n!/{((n-k)!k!)} = O(kn!/{((n-k)!k!)})$
>* **~={green}Space Complexity=~**: $O(k)$ our combinations are size $k$, hence our recursion stack is $O(k)$

> [!Question]- Combination Sum
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a list of candidate numbers and a target value, find all unique combinations of candidates where the chosen numbers sum to the target.
> * Each number in `candidates` can be used an unlimited number of times.
> * The solution should return a list of unique combinations.
> * Note:
>   - The numbers in `candidates` are not necessarily sorted.
>   - The same number may appear multiple times in a valid combination.
>
> **~={red}Solution=~**:
> * **~={green}Key Idea=~**: Use backtracking to explore all possible combinations of candidates that sum to the target.
> 
> * ~={green}**Steps**=~:
>   1. **~={green}Base Case=~**:
>      - If the sum of `currState` equals `target`, add the combination to the result.
>      - If the sum exceeds the target, terminate the current branch of recursion.
>   2. **~={green}Recursive Case=~**:
>      - Iterate over each candidate starting from the current index (`startIndex`).
>      - Add the candidate to `currState`.
>      - Recurse with the updated combination.
>      - After recursion, backtrack by removing the last added candidate.
>
> **~={red}Code=~**:
> ```cpp
> class Solution {
> public:
>     vector<vector<int​>> combinationSum(vector<int​>& candidates, int target) {
>         vector<vector<int​>> result;
>         vector<int​> currState;
>         int startIndex{0};
>         backtrack(currState, startIndex, result, candidates, target);
>         return result;
>     }
> 
>     void backtrack(vector<int​>& currState, int startIndex, vector<vector<int​>>& result, vector<int​>& candidates, int target) {
>         // Base Case: If the sum of the current combination is equal 
>         // to the target
>         if (accumulate(currState.begin(), currState.end(), 0) == target) {
>             result.push_back(currState);
>             return;
>         }
> 
>         // Base Case: If the sum exceeds the target, stop exploring 
>         // this path
>         if (accumulate(currState.begin(), currState.end(), 0) > target) return;
> 
>         // Recursive Case: Explore combinations
>         for (int i{startIndex}; i < candidates.size(); ++i) {
>             currState.push_back(candidates[i]);
>             // Allow reuse of candidates[i] as we pass i, not i + 1
>             // This however results in a strict order that elements
>             // closer to index 0, will always appear more first
>             // in each "combination" / path, hence not allowing for
>             // both [3, 2] and [2, 3] to show up as they are
>             // equivalent
>             backtrack(currState, i, result, candidates, target);  
>             currState.pop_back();
>         }
>     }
> };
> ```

## Other

> [!Question]- All Paths From Source to Target
> <!-- Multiline -->
> **~={red}Question=~**:
>* Given a directed acyclic graph (**DAG**) of `n` nodes labeled from `0` to `n - 1`, find all possible paths from node `0` to node `n - 1` and return them in **any order**.
>* The graph is given as follows: `graph[i]` is a list of all nodes you can visit from node `i` (i.e., there is a directed edge from node `i` to node `graph[i][j]`).
>
>**~={red}Solution=~**:
>
>**~={blue}All Paths=~**
>
>* **~={green}CurrState=~**: Stores the current path as it's being built.
>* **~={green}Result=~**: A collection of all paths.
>
>```cpp
>vector<vector<int​>> combinations(vector<vector<int​>>& graph) {
>	vector<vector<int​>> result;
>	// Initially Start at Index 0
>	vector<int​> currState = {0};
>	backtrack(currState, graph, result);
>	return result;
>}
>```
>
>**~={blue}Backtrack=~**
>
>* Basically, we only retrieve the subsets that are size $k$
>
>1. **~={green}Base Case=~**: When the path we have traversed has encountered the $n-1th$ node. Thus we have reached the end of this traversal.
>2. **~={green}Recursive Case=~**:
>* Explore every possible neighbour } For every node we explore, we will explore all it's neighbours (We apply for loop on every node/recursive call)
>
>```cpp
>void backtrack(vector<int​>& currState, vector<vector<int​>>& graph,
>			   vector<vector<int​>>& result) {
>	
>	// Base Case
>	if (currState[currState.size() - 1] == graph.size() - 1) {
>		result.push_back(currState);
>		return;
>	}
>	
>	// Recursive Case
>	int currNode = currState.back();
>	for (int neighbour : graph[currNode]) {
>		currState.push_back(neighbour);
>		backtrack(currState, graph, result);
>		currState.pop_back()
>	}
>}
>```
>**~={blue}Visualisation=~**
>
> ![[Drawing 2025-01-12 14.34.41.excalidraw | center | 450]]
>
>**~={blue}Complexity=~**
>* **~={green}Time Complexity=~**: $O(n \cdot 2^n)$
>* **~={green}Space Complexity=~**: $O(n)$

> [!Question]- Letter Combinations of a Phone Number
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.
> * A mapping of digit to letters (just like on a telephone keypad) is provided below:
>   - `2`: `a, b, c`
>   - `3`: `d, e, f`
>   - `4`: `g, h, i`
>   - `5`: `j, k, l`
>   - `6`: `m, n, o`
>   - `7`: `p, q, r, s`
>   - `8`: `t, u, v`
>   - `9`: `w, x, y, z`
> * Return the combinations in **any order**. If the input is empty, return an empty list.
>
> **~={red}Solution=~**:
> * **Key Idea**: Use backtracking to explore all combinations of letters.
> * **Steps**:
>   1. **Input Validation**:
>      - If the input string is empty, return an empty list.
>   2. **Recursive Backtracking**:
>      - Start with an empty string as the current combination.
>      - For each digit in the string:
>        - Retrieve its corresponding letters from the mapping.
>        - Append one letter at a time to the current combination.
>        - Move to the next digit.
>      - Once all digits are processed, store the combination.
>   3. **Termination**:
>      - Stop recursion when the current combination length equals the input digit string length.
>
> **~={red}Code Implementation=~**:
> ```cpp
> class Solution {
> private:
>     unordered_map<char, vector<char​>> numToChar = {
>         {'2', {'a', 'b', 'c'}}, 
>         {'3', {'d', 'e', 'f'}},
>         {'4', {'g', 'h', 'i'}},
>         {'5', {'j', 'k', 'l'}},
>         {'6', {'m', 'n', 'o'}},
>         {'7', {'p', 'q', 'r', 's'}},
>         {'8', {'t', 'u', 'v'}},
>         {'9', {'w', 'x', 'y', 'z'}}
>     };
>     
> public:
>     vector<string​> letterCombinations(string digits) {
>         if (digits.empty()) return {};
>         
>         vector<string​> result;
>         string currCombination;
>         backtrack(0, digits, currCombination, result);
>         return result;
>     }
> 
>     void backtrack(int index, string& digits, string& 
> 				   currCombination, vector<string​>& result) {
>         if (index == digits.size()) {
>             result.push_back(currCombination);
>             return;
>         }
>         
>         for (char letter : numToChar[digits[index]]) {
>             currCombination.push_back(letter);
>             backtrack(index + 1, digits, currCombination, result);
>             currCombination.pop_back();
>         }
>     }
> };
> ```
>
> **~={red}Complexity Analysis=~**:
> * **Time Complexity**: $O(3^M \cdot 4^N)$, where $M$ is the number of digits with 3 letters (e.g., `2-6, 8`) and $N$ is the number of digits with 4 letters (`7, 9`).
> * **Space Complexity**: $O(L)$, where $L$ is the length of the input digits, for the recursion stack and temporary storage.

#flashcards/dsa/patterns/backtracking
