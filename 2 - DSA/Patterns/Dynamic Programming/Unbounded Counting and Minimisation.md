# Basic Counting and Minimisation

## Definition

>[!Note]- What are DP Counting & Minimisation Problems?
> <!-- Multiline -->
> Given a set of values `{a, b, c}`, and a target value `d`, where you can take an unlimited number of each value from the set:
> * How many ways are there to sum to `d` (combinations or permutations)
> * The max/min number of values to reach `d`

>[!Info]- How to tackle Unbounded Counting & Minimisation Problems
> <!-- Multiline -->
> * **~={purple}Step One=~**: Define what `dp[x]` is
> * ~={purple}Step Two=~: Identify how to transition from smaller subproblems `dp[x - y], dp[x - z]` can be used to reach `dp[x]`. Then derive base cases.

## Sample Problems

>[!Question]- **Minimisation**: Minimising Coins
> <!-- Multiline -->
> **~={red}Problem=~**: Given a set of coins `{a, b, c, ...}` and a desired sum `d`. What is the minimum number of coins required to reach `d` if possible. Otherwise output `-1`
> 
> **~={purple}Step One=~**: Define `dp[x]`
> `dp[x]` = minimum number of coins required to reach sum `x`
> 
> **~={purple}Step Two=~**: Recurrence Relation
> The minimum number of coins to reach `dp[x]` is:
> * `min(dp[x - a], dp[x - b], dp[x - c], ...) + 1`
> 
> We add 1 as we need another coin to reach `x`
>
> **~={purple}Step Three=~**: Base Case
> * `vector<int​> dp(targetSum + 1, INT_MAX);`
> * `dp[0] = 0`: You need 0 coins to reach a sum of 0
> 
> ```cpp
> vector<int​> coins(numCoins, 0);
> vector<int​> dp(targetSum + 1, INT_MAX);
> dp[0] = 0;
> 
> for (int i{1}; i <= targetSum; ++i) {
> 	for (int j{0}; j < numCoins; ++j) {
> 		int coinValue = coins[j];
> 		// Check if prior dp value is in bounds, and if it is valid
> 		if (i - coins[j] >= 0 && dp[i - coinValue] != INT_MAX) {
> 			dp[i] = min(dp[i], dp[i - coinValue] + 1);
> 		}
> 	}
> }
>```
>
>**~={blue}Time Complexity=~**: $O(targetSum \cdot numCoins)$

>[!Question]- **Minimisation**: Removing Digits
> <!-- Multiline -->
> **~={red}Problem=~**: Given an integer `n`, you may repeatedly subtract the largest decimal digit of the current number. What’s the minimum number of moves to reduce `n` to zero?
> 
> **~={purple}Step One=~**: Define `dp[x]`
> `dp[x]` = minimum number subtractions needed to reduce `x` to 0
> 
> **~={purple}Step Two=~**: Recurrence Relation
> The minimum number of coins to reach `dp[x]` is:
> * Let `d` be the largest digit of `x`
> * `dp[x - d] + 1`
> 
> We add 1 as we need another subtraction to reach `x - d`
>
> **~={purple}Step Three=~**: Base Case
> * `vector<int​> dp(n + 1, 0);`
> * `dp[0] = 0`: You need 0 subtractions from 0 to reach 0
> * `dp[1 ... 9]`: You need 1 subtraction from 1 to 9 to reach 0
> 
> ```cpp
> vector<int​> dp(n + 1, 0);
> 
> for (int i{1}; i <= n; ++i) {
> 	if (i <= 9) {
> 		dp[i] = 1;
> 	} else {
> 		// Get smallest value
> 		int currValue{i};
> 		int largest{INT_MIN};
> 		while (currValue != 0) {
> 			largest = max(largest, currValue % 10);
> 			currValue /= 10;
> 		}
> 		dp[i] = dp[i - largest] + 1;
> 	}
> }
>```
>
> ![[Drawing 2025-06-08 13.10.21.excalidraw | center | 550]]
>
>**~={blue}Time Complexity=~**: $O(n)$

>[!Question]- **Number of Combs to reach Target**: Coin Combinations I
> <!-- Multiline -->
> **~={red}Problem=~**: Given a set of coins `{a, b, c, ...}` and a desired sum `d`. count the number of _unordered_ combinations of coins (i.e., order doesn’t matter) that sum to `d`
> 
> **~={purple}Step One=~**: Define `dp[x]`
> `dp[x]` = number of ways to reach sum `x`
> 
> **~={purple}Step Two=~**: Recurrence Relation
> The minimum number of coins to reach `dp[x]` is:
> * `dp[x - a] + dp[x - b] + dp[x - c], ...`
>
> **~={purple}Step Three=~**: Base Case
> * `vector<int​> dp(targetSum + 1, 0);`
> * `dp[0] = 1`: Number of ways to reach a sum of 0 is 1 (we choose nothing)
> 
> ```cpp
> vector<int​> coins(numCoins, 0);
> vector<int​> dp(targetSum + 1, 0);
> dp[0] = 1;
> 
> for (int i{1}; i <= targetSum; ++i) {
> 	for (int j{0}; j < numCoins; ++j) {
> 		int coinValue = coins[j];
> 		// Check if prior dp value is in bounds, and if it is valid
> 		if (i - coinValue >= 0) {
> 			dp[i] += dp[i - coinValue];
> 		}
> 	}
> }
>```
>
>**~={blue}Time Complexity=~**: $O(targetSum \cdot numCoins)$

>[!Question]- **Number of Perms to reach Target**: Coin Combinations II
> <!-- Multiline -->
> **~={red}Problem=~**: Given a set of coins `{a, b, c, ...}` and a desired sum `d`. count the number of _unordered_ combinations of coins (i.e., order doesn’t matter) that sum to `d`
> 
> **~={purple}Step One=~**: Define `dp[x]`
> `dp[x]` = number of unique sequences to reach sum `x`
> 
> **~={purple}Step Two=~**: Recurrence Relation
> The minimum number of coins to reach `dp[x]` is:
> * `dp[x - a] + dp[x - b] + dp[x - c], ...`
>
> **~={purple}Step Three=~**: Base Case
> * `vector<int​> dp(targetSum + 1, 0);`
> * `dp[0] = 1`: Number of ways to reach a sum of 0 is 1 (we choose nothing)
> 
> ```cpp
> vector<int​> coins(numCoins, 0);
> vector<int​> dp(targetSum + 1, 0);
> dp[0] = 1;
> 
> for (int j{0}; j < numCoins; ++j) {
> 	for (int i{1}; i <= targetSum; ++i) {
> 		int coinValue = coins[j];
> 		// Check if prior dp value is in bounds, and if it is valid
> 		if (i - coinValue >= 0) {
> 			dp[i] += dp[i - coinValue];
> 		}
> 	}
> }
>```
>This works because we process each coin one at a time (the outer `j` loop), and for each coin we only build up sums in increasing order of `i`. By never revisiting earlier coins after moving on, we count permutations rather than combinations.
>
>**~={blue}Time Complexity=~**: $O(targetSum \cdot numCoins)$

