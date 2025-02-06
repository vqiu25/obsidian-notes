# Example Problems

> [!Question]- Addition Mod: Divisibility Array  
> <!-- Multiline -->  
> **~={red}Question=~**:  
> * Given a numeric string `word`, construct an array `result` where:  
>   - `result[i] = 1` if the number formed by `word[0]...word[i]` is divisible by `m`.  
>   - Otherwise, `result[i] = 0`.  
> * Return the `result` array.  
>  
> ~={red}**Solution**=~:  
> 1. **Use Modulo Properties**:  
>    - Instead of constructing and checking large numbers, use the property that:  
>      - `(A * 10 + B) % m = ((A % m) * 10 + B) % m`.  
>    - This allows checking divisibility **incrementally** without overflow.  
> 2. **Iterate Through `word`**:  
>    - Convert each character to its numeric value.  
>    - Update `currVal` using the modulo formula.  
>    - Append `1` to `result` if `currVal % m == 0`, otherwise append `0`.  
>  
> ~={green}**Code**=~:  
> ```cpp  
> class Solution {  
> public:  
>     vector<int​> divisibilityArray(string word, int m) {  
>         long long currVal{0};  
>         vector<int​> result;  
>  
>         for (char letter : word) {
> 	        // If we want to check if the addition of  
> 	        // 2 values is % m, we can % at any time in the process
>             currVal = (currVal * 10 + (letter - '0')) % m;  
>             result.push_back(currVal % m == 0 ? 1 : 0);  
>         }  
>         return result;  
>     }  
> };  
> ```  
>  
> ~={green}**Key Points**=~:  
> * **Time Complexity**:  
>   - **O(n)**: We iterate through the string once.  
> * **Space Complexity**:  
>   - **O(n)**: Output array stores `n` elements.  

#flashcards/dsa/patterns/modulararithmitec
