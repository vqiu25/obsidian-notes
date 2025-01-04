> [!QUOTE] Quick Notes
> You need to find the largest, smallest, or most frequent K elements

# Overview
## Recipe

>[!Note]- Top K Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Top K**=~: Use Top-K over a normal heap when you only need the largest, smallest, or most relevant K elements, making it more efficient when k < n.

## Time Complexity

>[!Note]- Time Complexity of Top K
> <!-- Multiline -->
> * **~={purple}Time Complexity=~**: $O(nlogk)$
> 	* Each of the the $n$ elements is pushed into a heap of size $k$, where insertion and deletion take $O(logk)$
> * **~={purple}Space Complexity=~**: $O(k)$
> 	* The heap maintains at most $k$ elements, hence $O(k)$ space

# Templates

> [!Info]- Top K Elements Using Min-Heap Template
> <!-- Multiline -->
> **<u>Explanation</u>**
> 1. **~={purple}Building the Heap=~**: Iterate through the array and add each element to a min-heap.
> * **~={purple}Maintain Size K=~**: After adding an element, check if the heap size exceeds `k`. If it does, remove the smallest element (top of the heap).
> 2. **~={purple}Extract K Largest=~**: Once all elements are processed, the heap will contain the `k` largest elements. Extract them and return the result.
>
> **<u>C++ Code</u>**
> ```cpp
> vector<int​> fn(vector<int​>& arr, int k) {
> 	// Min-Heap to track k largest elements
>     priority_queue<int​, vector<int​>, greater<int​>> heap;
>     
>     for (int num : arr) {
>         heap.push(num); // Add element to the heap
>         if (heap.size() > k) {
>             heap.pop(); // Remove smallest element if heap size exceeds k
>         }
>     }
>
>     vector<int​> ans;
>     while (heap.size() > 0) {
>         ans.push_back(heap.top()); // Extract elements from the heap
>         heap.pop();
>     }
>     
>     return ans; // Return k largest elements
> }
> ```
>

# Example Problems

> [!Question]- Find K Closest Elements
> <!-- Multiline -->
> **<u>Explanation</u>**
> 1. **~={purple}Calculate Distances=~**: For each element in the array, calculate its distance from `x` and store it as a pair of `(element, distance)`.
> 	* **~={purple}Maintain Size K=~**: Use a max-heap to track the `k` closest elements to `x`. If the heap size exceeds `k`, remove the farthest element (largest distance). We achieve this by storing the pair in the opposite direction `(distance, element)` so the furthest elements will be popped first
> 2. **~={purple}Return Sorted Result=~**: Extract elements from the heap, sort them, and return the result.
>
> **<u>C++ Code</u>**
> ```cpp
> vector<int​> findClosestElements(vector<int​>& arr, int k, int x) {
>    // Create a map that maps how close element a is to x
>    // !Warning: Don't use a map if duplicate inputs exist
>    vector<pair<int​, int​>> distanceToX;
>
>    for (int element : arr) {
>         int distance = abs(x - element);
>         distanceToX.push_back({element, distance});
>     }  
>
>     priority_queue<pair<int​, int​>> maxHeap; // Max-Heap based on distance
>
>     for (const auto& pair : distanceToX) {
>         int element = pair.first;
>         int distanceAway = pair.second;
>         maxHeap.push({distanceAway, element});
>         if (maxHeap.size() > k) {
>             maxHeap.pop(); // Remove farthest element if heap exceeds size k
>         }
>     }
>
>     vector<int​> result;
>     while (!maxHeap.empty()) {
>         int closest = maxHeap.top().second;
>         maxHeap.pop();
>         result.push_back(closest);
>     }
>     
>     sort(result.begin(), result.end()); // Sort the k closest elements
>     return result;
> }
> ```
>
> **<u>**Key Points**</u>**
> * ~={purple}**Heap Usage**=~: The max-heap ensures that the `k` closest elements are efficiently tracked.
> * ~={purple}**Sorting**=~: Sorting the result is necessary for the output to be in ascending order.
> * ~={purple}**Time Complexity**=~:
>   - Building the heap: $O(n \log k)$, where $n$ is the size of the array.
>   - Sorting the result: $O(k \log k)$.
>   - Total: $O(n \log k + k \log k)$.
> * ~={purple}**Space Complexity**=~: $O(k)$ for the heap.

#flashcards/dsa/patterns/heap/topk