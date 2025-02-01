# Overview
## Points of Interest

> [!Info]- **O(1) Majority Element: Boyer-Moore Voting Algorithm**
> 
> **~={purple}Explanation=~**
> 
> 1. ~={green}**Problem Statement**=~:
>     - Given an array of integers, find the **majority element**, which appears **more than ⌊n/2⌋ times**.
> 2. ~={green}**Key Insight**=~:
>     - If an element appears more than half the time, **it will always remain in the majority when votes cancel out**.
> 3. ~={blue}**Algorithm Process**=~:
>     - Maintain a `candidate` (potential majority element) and a `counter` (net count of votes).
>     - Iterate through the array:
>         - If `counter == 0`, assume the current element is the new candidate.
>         - If the current element matches `candidate`, increment `counter`.
>         - Otherwise, decrement `counter` (vote cancellation).
>     - The final `candidate` is guaranteed to be the majority element.
> 
>  ![[Drawing 2025-01-28 20.43.07.excalidraw | center | 450]]
> 
> ~={purple}**Code: Boyer-Moore Voting Algorithm**=~
> 
> ```cpp
> class Solution {
> public:
>     int majorityElement(vector<int​>& nums) {
>         int counter{1};
>         int candidate{nums[0]};
> 
>         for (int i{1}; i < nums.size(); ++i) {
>             if (counter == 0) {
>                 candidate = nums[i];
>             }
>             if (nums[i] == candidate) {
>                 counter++;
>             } else {
>                 counter--;
>             }
>         }
>         return candidate;
>     }
> };
> ```
> ~={purple}**Why It Works**=~
> 
> - ~={red}**Cancellation Effect**=~:
>     - The majority element has **more than half the votes**, meaning that even when votes cancel out, it remains the last standing candidate.
> - ~={blue}**Mathematical Justification**=~:
>     - At worst, the majority element is **canceled out by all other elements** but will **always have at least one occurrence left**.
>     - Since it appears more than **⌊n/2⌋ times**, it cannot be fully eliminated.
> 
> ~={purple}**Complexity Analysis**=~
> 
> - ~={green}**Time Complexity**=~: $O(n)$ (Single pass through the array)
> - ~={green}**Space Complexity**=~: $O(1)$ (Only two variables used: `candidate` and `counter`)

#flashcards/dsa/patterns/greedy/voting