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
> The time complexity of checking every subarray (contiguous) is $O(n^2)$, because for a $n$ length array:
> ````col
>```col-md
>![[Drawing 2024-12-10 08.17.02.excalidraw | center | 300]]
>```
>```col-md
>There are:
>* ~={purple}Length 1=~: 4 / ($n$)
>* ~={green}Length 2=~: 3 / ($n-1$)
>* ~={red}Length 3=~: 2 / ($n-2$)
>* ~={orange}Length 4=~: 1 / ($n-3$)
>
>And the sum from $1$ to $n$ is $\frac{n(n+1)}{2}$ making it $O(n^2)$
>```
>````
>Where in Sliding Windows:
>* The left pointer, will only ever shift $n$ places
>* The right pointer will only ever shift $n$ places
>
>Thus the complexity will be $O(n) + O(n) = O(2n) = O(n)$

## Points of Interest

>[!Note]- Backtracking Traversal Process?
> <!-- Multiline -->
>We use Recursive DFS

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

#flashcards/dsa/patterns/backtracking
