> [!QUOTE] Quick Notes
> Anytime you find your algorithm running if ... in ..., then consider using a hash map or set 

# Overview
## Recipe

>[!Note]- Counting Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Prefix Sums=~**: Whenever we have to determine the sum or product of subarrays efficiently.
>2. **~={purple}Prefix Sum Strategies=~**:
>* **~={blue}Subarray Sums=~**:
>	* **~={red}When to Use=~**: Need to find the sum of subarrays.
>	* **~={red}What to Note=~**:
>		* **~={pink}Define prefix[i]=~**: Define to contain everything before or, instead inclusive.
>		* **~={pink}Subquery Logic=~**: How to query the prefix sum. Usually is a range between index `i` and `j` from the original array.

# Example Problems

> [!Question]- Counting Elements
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an array of integers `nums` and an integer `k`. A continuous subarray is called **nice** if there are `k` odd numbers on it.
> * Return _the number of **nice** sub-arrays_.
>
>**~={red}Solution=~**:
>
>This question is similar to the template. If we iterate through our input array, and change:
>* **~={green}Even Numbers=~**: to 0
>* **~={red}Odd Numbers=~**: to 1
>
>A subarray that sums to k, will now be equivalent to having k odd numbers in it.

#flashcards/dsa/patterns/checkingforexistence
