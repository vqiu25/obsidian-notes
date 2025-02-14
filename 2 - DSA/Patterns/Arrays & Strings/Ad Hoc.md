# Example Problems

> [!Question]- Maximum Subarray (Kadane’s Algorithm)
> 
> ~={red}Question=~:
> 
> - Given an integer array `nums`, find the **subarray** with the maximum sum and return its sum.
> 
> ~={red}Solution=~:
> 
> 1. ~={blue}**Keep Track of Current Subarray Sum (`currSum`)**=~:
>     - Iterate through `nums[]`, adding each element to `currSum`.
>     - If `currSum` becomes negative, reset it to `0`, as a negative prefix sum **can never contribute positively** to a future subarray.
> 2. ~={blue}**Update Global Maximum (`maxSum`)**=~:
>     - At each step, store the maximum subarray sum encountered so far.
>     - This ensures we always return the highest sum found.
>  
> ![[Drawing 2025-02-09 21.20.08.excalidraw | center | 400]]
> 
> ~={green}**C++ Code**=~:
> 
> ```cpp
> class Solution {
> public:
>     int maxSubArray(vector<int​>& nums) {
>         int maxSum{INT_MIN};
>         int currSum{0};
>         
>         for (int i{0}; i < nums.size(); ++i) {
>             currSum += nums[i];  // (1) Expand subarray
>             maxSum = max(maxSum, currSum);  // (2) Update max sum found
> 
>             if (currSum < 0) currSum = 0;  // (3) Reset if sum is negative
>         }
>         
>         return maxSum;
>     }
> };
> ```
> 
> ~={green}**Key Points**=~:
> - **~={blue}Time Complexity=~**:
>     - **$O(n)$**: Single pass through `nums[]`.
> - **~={blue}Space Complexity=~**:
>     - **$O(1)$**: Uses only a few integer variables.
> - **Why Reset `currSum`?**
>     - If `currSum` is **negative**, then extending it further will only decrease future sums. Resetting allows us to start a fresh subarray.
