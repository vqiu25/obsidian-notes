# Example Problems

> [!Question]- Isomorphic Strings
> <!-- Multiline -->
> **~={red}Question=~**:
> * Two strings `s` and `t` are **isomorphic** if **each character in `s` can be mapped to exactly one character in `t`**, and vice versa.
> * No two characters may map to the same character, but a character **can** map to itself.
> * Given two strings `s` and `t`, return `true` if they are **isomorphic**, otherwise return `false`.
>
>**~={red}Solution=~**:
>
>1. **~={purple}Use Two Hash Maps=~**:
>   - One map (`sToT`) tracks mappings from `s[i] → t[i]`.
>   - Another map (`tToS`) ensures the mapping is **one-to-one** by tracking `t[i] → s[i]`.
>
>2. **~={purple}Iterate Through the Strings=~**:
>   - If `s[i]` has not been mapped yet (`sToT[s[i]] == 0`), store `s[i] → t[i]` in `sToT` and `t[i] → s[i]` in `tToS`.
>   - If a mapping exists but does not match `t[i]`, return `false` (as this violates the one-to-one rule).
>
>3. **~={purple}Return True if All Mappings are Valid=~**:
>   - If all characters are mapped correctly, return `true`, meaning `s` and `t` are isomorphic.
> 
> ![[Drawing 2025-01-28 22.48.00.excalidraw | center | 450]]
>
>```cpp
>class Solution {
>public:
>    bool isIsomorphic(string s, string t) {
>        unordered_map<char, char> sToT{};
>        unordered_map<char, char> tToS{};
>
>        for (int i{0}; i < s.size(); ++i) {
>            // If no mapping exists, create it
>            if (sToT[s[i]] == 0 && tToS[t[i]] == 0) {
>                sToT[s[i]] = t[i];
>                tToS[t[i]] = s[i];
>            } 
>            // If a mapping exists but does not match, return false
>            else if (sToT[s[i]] != t[i] || tToS[t[i]] != s[i]) {
>                return false;
>            }
>        }
>        return true;
>    }
>};
>```
>
>**~={green}Key Points=~**:
> - **~={purple}Time Complexity=~**: $O(n)$ - We iterate through the strings once.
> - **~={purple}Space Complexity=~**: $O(1)$ - At most 52 key-value pairs (`a-z` and `A-Z`) in the hash maps.

> [!Question]- Happy Number
> <!-- Multiline -->
> **~={red}Question=~**:
> * Write an algorithm to determine if a number `n` is a "happy number".
> * A **happy number** is defined as follows:
>   - Starting with any positive integer, replace the number by the sum of the squares of its digits.
>   - Repeat the process until the number equals `1` (it will stay `1`), or it loops endlessly in a cycle that does not include `1`.
>   - If a number ends in `1`, it is a "happy number."
> 
> **~={red}Solution=~**:
> 1. **~={purple}Handle 3 Cases=~**:
>    - **Case 1**: The number reaches `1`, and it is a happy number.
>    - **Case 2**: The number enters a cycle (repeats a previously seen value).
>    - **Case 3**: The number grows infinitely large (**this cannot happen**; see below).
> 2. **~={purple}Steps=~**:
>    - Use a set to store numbers already seen to detect a cycle.
>    - Compute the sum of squares of digits repeatedly.
>    - If a number repeats or the value becomes `1`, stop.
>
> **~={red}Code=~**
> ```cpp
> bool isHappy(int n) {
>     unordered_set<int​> repeats;
>     while (n != 1) {
>         int tempVal = n;
>         n = 0;
>         while (tempVal != 0) {
>             int lastDigit = tempVal % 10;
>             n += lastDigit * lastDigit;
>             tempVal /= 10;
>         }
>         if (repeats.find(n) != repeats.end()) return false;
>         repeats.insert(n);
>     }
>     return true;
> }
> ```
> 
> **~={green}Why Infinite Growth is Impossible (Case 3)=~**:
> 1. **Sum of Squares Reduces the Number**:
>    - For any integer $n$, the sum of the squares of its digits is **always smaller than $n$** for most values:
>      - E.g., for $9999$, the sum is $9^2 + 9^2 + 9^2 + 9^2 = 324$, much smaller than $9999$.
> 2. **Finite Range**:
>    - All numbers eventually reduce to a range between $1$ and $243$ (e.g., $243 = 9^2 \cdot 3$ for three digits).
>    - Once the number enters this range, it either:
>      - Reaches $1$ (happy number).
>      - Falls into a cycle (infinite loop).
>
> **~={green}Time Complexity Derivation=~**:
> 1. **Digit Reduction**:
>    - For a number with $d$ digits, the sum of squares operation runs in $O(9^2 \cdot d) = O(d)$.
>    - For $n$, the number of digits $d = \lfloor \log_{10} n \rfloor + 1$, so each step computes in $O(\log n)$.
> 2. **Finite Range**:
>    - After reducing to the range $[1, 243]$, the process takes at most a constant number of steps to detect a cycle or reach $1$.
> 3. **Overall Time Complexity**:
>    - **$O(\log n)$**: Proportional to the number of digits in $n$.
>
> **~={green}Space Complexity=~**:
> - $O(\log n)$: Space for the unordered set storing previously seen numbers.



#flashcards/dsa/patterns/hashing/adhoc