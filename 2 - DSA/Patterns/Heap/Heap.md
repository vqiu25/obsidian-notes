> [!QUOTE] Quick Notes
> Whenever we want to compute the max or min frequently

# Overview
## Recipe

>[!Note]- Heap Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Heaps**=~: Whenever we want to compute the max or min frequently

## Time Complexity

>[!Note]- Time Complexity of Heaps
> <!-- Multiline -->
> Everytime we add or remove from the heap, that is an $O(logn)$ action. Often we may have to perform this action $n$ times, making it $O(nlogn)$

## Points of Interest

> [!info]- Find Median from Data Stream (2 Heaps)
> <!-- Multiline -->
> **~={red}Question=~**:
>* Design a data structure that supports adding numbers from a data stream and finding the median efficiently at any point.
>* Implement the following:
>	1. `addNum(int num)`: Adds a number to the data structure.
>	2. `findMedian()`: Returns the median of all numbers added so far.
>
>**~={red}Solution=~**:
>* **~={purple}Approach=~**:
>	1. **~={purple}Use two heaps to maintain the data=~**:
>		- `maxHeap`: Stores the **~={green}smaller=~** half of the numbers (as a max-heap).
>		- `minHeap`: Stores the **~={red}larger=~** half of the numbers (as a min-heap).
>	2. **~={purple}Keep the heaps balanced such that=~**:
>		- `maxHeap` always contains the middle element when the total number of elements is odd.
>		- The sizes of `maxHeap` and `minHeap` differ by at most 1.
>
>**~={green}<u>Heap Balancing</u>=~**:
>
> ![[Drawing 2025-01-03 20.59.15.excalidraw | center | 600]]
>
>* **~={purple}When adding a number=~**:
>	1. Always push it first into the `maxHeap`.
>	2. Move the largest element from `maxHeap` to `minHeap` to ensure all elements in `maxHeap` are smaller than those in `minHeap`.
>	3. If `minHeap` becomes larger than `maxHeap`, move its smallest element back to `maxHeap`.
>```cpp
>class MedianFinder {
>private:
>    priority_queue<int​> maxHeap;
>    priority_queue<int​, vector<int​>, greater<int​>> minHeap;
>
>public:
>    MedianFinder() {}
>    
>    void addNum(int num) {
>        maxHeap.push(num);
>        int topOfMax = maxHeap.top();
>        maxHeap.pop();
>        minHeap.push(topOfMax);
>        if (minHeap.size() > maxHeap.size()) {
>            int bottomOfMin = minHeap.top();
>            minHeap.pop();
>            maxHeap.push(bottomOfMin);
>        }
>    }
>```
>
>**~={green}<u>Finding the Median</u>=~**:
>
> ![[Drawing 2025-01-03 20.55.39.excalidraw | center | 600]]
>
>* **~={purple}To find the median=~**:
>	1. If the total number of elements is odd, the median is the top of `maxHeap`.
>	2. If the total number of elements is even, the median is the average of the tops of `maxHeap` and `minHeap`.
>```cpp
>    double findMedian() {
>        if (maxHeap.size() == minHeap.size()) {
>            return (maxHeap.top() + minHeap.top()) / 2.0;
>        } else {
>            return maxHeap.top();
>        }
>    }
>};
>```
>
>**~={green}<u>Key Points</u>=~**:
>* **~={purple}Time Complexity=~**:
>	- `addNum(int num)`: $O(logn)$, due to heapify
>	- `findMedian()`: $O(1)$, as it simply retrieves the top elements of the heaps.
>* **~={purple}Space Complexity=~**:
>	- $O(n)$, where $n$ is the total number of elements added, as all elements are stored in the heaps.

# Example Problems

> [!Question]- Halve Array Sum
> <!-- Multiline -->
> **~={red}Question=~**:
>* You are given an array `nums`. In one operation, you can halve the value of any element.
>* Return the minimum number of operations required to reduce the sum of the array by at least half.
>
>**~={red}Solution=~**:
>1.  Compute the `totalSum` of the array and add all elements to a max-heap.
>2. While `currentSum > totalSum / 2`:
>	- Remove the largest element from the heap.
>	- Halve its value and add it back to the heap.
>	- Update `currentSum` and increment the operation counter.
>3. Return the number of operations performed.
>
>**~={green}<u>Code</u>=~**:
>```cpp
>class Solution {
>public:
>    int halveArray(vector<int​>& nums) {
>        priority_queue<double​> h;
>        double totalSum{0};
>
>        for (int num : nums) {
>            h.push(num);
>            totalSum += num;
>        }
>
>        double currentSum = totalSum;
>        int numOps{0};
>
>        while (currentSum > (totalSum / 2)) {
>            double largest = h.top();
>            double readded = largest / 2;
>            h.pop(); 
>            h.push(readded);
>            currentSum = currentSum - largest + readded;
>            numOps++;
>        }
>        return numOps;
>    }
>};
>```
>
>**~={green}<u>Key Points</u>=~**:
>* **~={purple}Time Complexity:=~** The worst approach would be to half each element in the array, so that would be $O(nlogn)$, where heapify is called $n$ times
>* **~={purple}Space Complexity:=~** $O(n)$ for the heap.

#flashcards/dsa/patterns/heap/heap