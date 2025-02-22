> [!QUOTE] Quick Notes
> * Input is sorted, or would benefit from being sorted
> * Problem is asking for the max/min of something
> * Make the local optimal choice

# Overview
## Recipe

>[!Note]- Greedy Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Greedy**=~: Problem is asking for the max/min of something

## Time Complexity

>[!Note]- Time Complexity of Greedy
> <!-- Multiline -->
> Usually requires sorting, hence $O(nlogn)$

# Example Problems

> [!question]- Maximum Units on a Truck
> <!-- Multiline -->
> **~={red}Question=~**
> * You are assigned to put some amount of boxes onto **one truck**. You are given a 2D array `boxTypes`, where `boxTypes[i] = [numberOfBoxesi, numberOfUnitsPerBoxi]`:
> * `numberOfBoxesi` is the number of boxes of type `i`.
> * `numberOfUnitsPerBoxi` is the number of units in each box of the type `i`.
> * You are also given an integer `truckSize`, which is the **maximum** number of **boxes** that can be put on the truck. You can choose any boxes to put on the truck as long as the number of boxes does not exceed `truckSize`.
> * Return _the **maximum** total number of **units** that can be put on the truck._
> 
> **~={red}**Solution**=~**
> 1. **~={purple}Sorting Step=~**:
>    - Swap the number of boxes and units per box in each `boxType`, resulting in the format `(units per box, number of boxes)`.
>    - Sort the `boxTypes` in **ascending order** by units per box. That way we are retrieving boxes that come wth the most units locally.
> 2. **~={purple}Greedy Choice=~**:
>    - Start from the highest units per box (last element in the sorted list).
>    - Take as many boxes as possible from the current type, up to the remaining `truckSize`.
> 3. **~={purple}Stop Condition=~**:
>    - Stop when the truck is full (`truckSize == 0`).
>    
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> class Solution {
> public:
>     int maximumUnits(vector<vector<int​>>& boxTypes, int truckSize) {
>         for (vector<int​>& pair : boxTypes) {
>         // Swap to format (units per box, number of boxes)
>             swap(pair[0], pair[1]);
>         }
>
>         // Sort by units per box (ascending)
>         sort(boxTypes.begin(), boxTypes.end());
>
>         int currUnits{0};
>         for (int i = boxTypes.size() - 1; i >= 0; --i) {
> 	        // Take boxes up to truck size
>             int currBox = min(truckSize, boxTypes[i][1]);
>             // Add units from chosen boxes
>             currUnits += currBox * boxTypes[i][0]; 
>             // Reduce remaining truck space    
>             truckSize -= currBox;
>             // Stop if truck is full                        
>             if (truckSize == 0) break;                   
>         }
>         return currUnits;
>     }
> };
> ```
> 
> **<u>~={green}**Key Points**=~</u>**
> - **~={purple}Time Complexity=~**:
>   - Sorting: $O(n \log n)$.
>   - Iteration: $O(n)$.
>   - Total: $O(n \log n)$.
> - ~={purple}**Space Complexity**=~: $O(1)$ (in-place sort and calculations).

> [!question]- Number of Rescue Boats (Two Pointers)
> <!-- Multiline -->
> **~={red}Question=~**
> * You are given an array `people` where `people[i]` represents the weight of the $(i^{th})$ person, and an integer `limit` which is the weight limit of a boat.
> * Each boat can carry at most two people at the same time, provided the sum of their weights is at most `limit`.
> * Return _the minimum number of boats required to rescue everyone._
> 
> **~={red}Solution=~**
> 1. **~={purple}Sorting Step=~**:
>    - Sort the `people` array in **ascending order** to allow pairing the lightest and heaviest individuals.
> 2. **~={purple}Two-Pointer Approach=~**:
>    - Use two pointers:
>        - `left` starts at the lightest person.
>        - `right` starts at the heaviest person.
>    - If the sum of weights at `left` and `right` is within the `limit`, pair them and move both pointers inward.
>    - Otherwise, assign the `right` person their own boat and move only the `right` pointer inward.
> 3. **~={purple}Stop Condition=~**:
>    - Stop when `left` surpasses `right`, indicating all people are accounted for.
> 
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> class Solution {
> public:
>     int numRescueBoats(vector<int​>& people, int limit) {
>         sort(people.begin(), people.end()); // Sort weights in ascending order
>
>         int left{0};
>         int right = people.size() - 1;
>         int boat{0};
>		 
>		 // In the event there is only 1 person, <= handles  that case
>		 // allocating that 1 person a boat
>         while (left <= right) {
>             boat++; // Allocate a boat
>             if (people[left] + people[right] <= limit) { 
>                 left++; right--; // Pair the lightest and heaviest
>             } else {
>                 right--; // Give the heaviest their own boat
>             }
>         }
>
>         return boat;
>     }
> };
> ```
> 
> **<u>~={green}Key Points=~</u>**
> - **~={purple}Time Complexity=~**:
>    - Sorting: $O(n \log n)$.
>    - Two-pointer traversal: $O(n)$.
>    - Total: $O(n \log n)$.
> - **~={purple}Space Complexity=~**:
>    - $O(1)$ (in-place sorting and pointer operations).

> [!question]- Find Maximised Capital
> <!-- Multiline -->
> **~={red}Question=~**
> * You are given `k` projects, an initial capital `w`, and two arrays:
>   - `profits[i]`: The profit from the $(i^{th})$ project.
>   - `capital[i]`: The minimum capital required to start the $(i^{th})$ project.
> * You can only select one project at a time, and your capital increases by the profit of the selected project.
> * Return _the maximum capital you can have after completing at most `k` projects._
> 
> **~={red}Solution=~**
> 1. **~={purple}Sorting Step=~**:
>    - Create a list of `(capital, profit)` pairs and sort it by capital in **ascending order**.
>    - This ensures that projects are processed in the order of capital requirements.
> 2. **~={purple}Heap for Profits=~**:
>    - Use a max-heap to store profits of all projects that can be started with the **~={green}current capital=~**.
>    - Add projects to the heap as long as their capital requirement is within the current capital `w`.
> 3. **~={purple}Greedy Choice=~**:
>    - At each iteration, select the project with the highest profit from the heap to maximise capital gain.
> 4. **~={purple}Stop Condition=~**:
>    - Stop when no more projects can be started or when `k` projects have been completed.
> 
> **<u>~={green}C++ Code=~</u>**
> ```cpp
> class Solution {
> public:
>     int findMaximizedCapital(int k, int w, vector<int​>& profits, vector<int​>& capital) {
>         vector<pair<int, int>> capitalAndProfit;
>
>         for (int i{0}; i < profits.size(); ++i) {
> 	        // Pair capital and profits
>             capitalAndProfit.push_back({capital[i], profits[i]}); 
>         }
>
>         // Sort by capital
>         sort(capitalAndProfit.begin(), capitalAndProfit.end());
>
>         int left{0};
>         priority_queue<int​> maxProfits; // Max-Heap for profits
>
>         for (int i = 0; i < k; ++i) {
>             // Add all projects within current capital to the heap
>             while (left < capitalAndProfit.size() && 
> 		           capitalAndProfit[left].first <= w) {
>                 maxProfits.push(capitalAndProfit[left].second);
>                 left++;
>             }
>
>             // If no projects are available, return current capital
>             if (maxProfits.empty()) return w;
>
>             // Pick the most profitable project
>             int mostProfitable = maxProfits.top();
>             maxProfits.pop();
>             w += mostProfitable; // Increase capital by the profit
>         }
>
>         return w; // Return final capital
>     }
> };
> ```
> 
> **<u>~={green}Key Points=~</u>**
> - **~={purple}Time Complexity=~**:
>   - Sorting: $O(n \log n)$, where \(n\) is the number of projects.
>   - Heap operations: $O(k \log n)$ for adding/removing from the heap.
>   - Total: $O(n \log n + k \log n)$.
> - **~={purple}Space Complexity=~**:
>   - $O(n)$ for the heap.

> [!Question]- Minimum Total Time to Complete Book Reading
> <!-- Multiline -->
> **~={red}Question=~**:
> - Given `numBooks` and an array `times` representing the time required to read each book, determine the minimum total time needed to finish all books when:
>   - Books can be read in parallel.
>   - A book cannot be interrupted once started.
> 
> **~={red}Solution=~**:
> 1. **~={purple}Key Insight=~**:  
>    - If the largest reading time (`maxElement`) exceeds the combined reading time of all other books (`totalSum - maxElement`), it means there will be idle time while waiting for the longest book to finish.  
>    - In such a case, the total time is `maxElement * 2`.
>    - Otherwise, all books can be scheduled without gaps, so the total time is simply `totalSum`.
> 
> 
> **~={green}<u>Code</u>=~**:
> ```cpp
>
> using namespace std;
>
> int main() {
>     ios::sync_with_stdio(false);
>     cin.tie(nullptr);
>
>     long long numBooks;
>     cin >> numBooks;
>
>     vector<long long​> times(numBooks);
>     for (int i{0}; i < numBooks; ++i) cin >> times[i];
>
>     long long maxElement = *max_element(times.begin(), times.end());
>     long long totalSum = accumulate(times.begin(), times.end(), 0LL);
>
>     if (maxElement > totalSum - maxElement) cout << maxElement * 2;
>     else cout << totalSum;
> }
> ```
> 
> **~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**:  
>   - $O(n)$: Single pass for reading input and computing sum and max element.
> - **~={purple}Space Complexity=~**:  
>   - $O(1)$: Only a vector for storing times and a few scalar variables are used.
> 
> **~={purple}Why the Logic Works=~**:
> - If the longest task (book) exceeds the total time of the others combined, the minimum total time equals twice the longest reading time (since the rest cannot fully parallelise the longest one).
> - Otherwise, the reading can be scheduled such that all books finish exactly when the total reading time ends.

> [!Question]- Increasing Triplet Subsequence
> <!-- Multiline -->
> **~={red}Question=~**:
> - Given an integer array `nums`, determine whether there exists a subsequence of three increasing elements (i.e., `nums[i] < nums[j] < nums[k]` with `i < j < k`).
> - Return `true` if such a triplet exists, otherwise return `false`.
>
> **~={green}Solution=~**:
> 1. **~={purple}Key Idea=~**:
>    - We want to track two values while iterating:
>      - `currMin`: The smallest number encountered so far.
>      - `currSecMin`: The smallest number greater than `currMin` encountered so far.
>    - If we find a number greater than `currSecMin`, we have identified an increasing triplet.
>
> 1. **~={purple}Approach=~**:
>    - **~={blue}Step 1=~**: Initialize `currMin` and `currSecMin` to `INT_MAX`.
>    - ~={blue}**Step 2**=~: Iterate through the array:
>       - Update `currMin` if a smaller element is found.
>       - Otherwise, if the current element is greater than `currMin`, attempt to update `currSecMin`.
>       - If the current element is greater than `currSecMin`, return `true`.
>    - **Step 3**: Return `false` if no valid triplet is found.
>
> **~={purple}Code=~**:
> ```cpp
> class Solution {
> public:
>     bool increasingTriplet(vector<int​>& nums) {
>         int currMin = INT_MAX;
>         int currSecMin = INT_MAX;
>
>         for (int num : nums) {
>             if (num <= currMin) {
>                 currMin = num; // Update smallest value
>             } else if (num <= currSecMin) {
>                 currSecMin = num; // Update second smallest value
>             } else {
>                 return true; // Found a number greater than currSecMin
>             }
>         }
>         return false;
>     }
> };
> ```
>
> ~={green}**Key Points**=~:
> - ~={purple}**Time Complexity**=~:  
>   - $O(n)$: Single pass through the array.
> - ~={purple}**Space Complexity**=~:  
>   - $O(1)$: Only constant extra space used.
>
> ~={purple}**Why the Logic Works**=~:
> - The algorithm ensures that if a valid increasing triplet exists, we will encounter it by continuously narrowing down the smallest (`currMin`) and second smallest (`currSecMin`) candidates. Once a third number greater than `currSecMin` appears, the condition is satisfied immediately.

#flashcards/dsa/patterns/greedy/greedy