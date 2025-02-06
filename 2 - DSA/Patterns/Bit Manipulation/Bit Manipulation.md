# Points of Interest

>[!Note]- Bit Masking
> <!-- Multiline -->
>We can use a bit mask to check if **~={green}certain bits are 0 or 1=~**, or **~={red}toggle a particular bit=~**:
>
> ![[Drawing 2025-02-04 22.12.35.excalidraw | center | 600]]

>[!Note]- XOR Properties
> <!-- Multiline -->
>**~={blue}If you XOR yourself=~**:
>* 0 XOR 0 = 0
>* 1 XOR 1 = 0
>
>-> You get 0
>
>**~={blue}If you XOR with 0=~**:
>* 0 XOR 0 = 0
>* 1 XOR 0 = 0
>
>-> You get the same number

>[!Note]- Number of Bits for a Number in Base 10
> <!-- Multiline -->
>The number of bits required to represent a base 10 number in base 2 is:
>$$\log_2(n) + 1$$
>This is because for $k$ bits, we can represent $2^k$ numbers.

# Example Problems

> [!Question]- Hamming Weight (Number of 1 Bits)  
> <!-- Multiline -->  
> **~={blue}Question=~**:  
> * Given an integer `n`, return the **number of 1 bits** in its binary representation.  
> * This is also known as the **Hamming Weight** of `n`.  
>  
> **~={blue}Solution=~**:  
> 1. ~={purple}**Bitwise AND with a Mask**=~:  
>    - Use a `mask = 1` to check each bit of `n`.  
>    - `(n & mask) != 0` checks if the least significant bit is `1`.  
> 2. ~={purple}**Shift the Mask Left**=~:  
>    - Move the mask left by one (`mask = mask << 1`) to check the next bit.  
> 3. ~={purple}**Iterate Over All Bits**=~:  
>    - The number of bits in `n` is `log2(n) + 1`.  
>    - Loop from `1` to `numBits`, counting set bits.  
> 4. **~={purple}Clarifying Order of Operations=~**:  
>    - `n & mask` must be enclosed in parentheses: `(n & mask) != 0`.  
>    - **~={red}Without parentheses, bitwise AND has lower precedence than comparison operators, leading to incorrect behaviour=~**.  
>  
> ~={blue}**Code**=~:  
> ```cpp  
> class Solution {  
> public:  
>     int hammingWeight(int n) {  
>         int mask{1};  
>         int count{0};  
>  
>         int numBits = log2(n) + 1;  
>  
>         for (int i{1}; i <= numBits; ++i) {  
>             if ((n & mask) != 0) ++count;  // Parentheses ensure correct precedence  
>             mask = mask << 1;  
>         }  
>         return count;  
>     }  
> };  
> ```  
>  
> **~={green}Key Points=~**:  
> * **Time Complexity**:  
>   - **O(log n)**: We iterate through the number of bits in `n`.  
> * **Space Complexity**:  
>   - **O(1)**: Only a few integer variables are used.  
