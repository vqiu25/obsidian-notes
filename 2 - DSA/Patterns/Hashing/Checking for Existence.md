> [!QUOTE] Quick Notes
> Anytime you find your algorithm running if ... in ..., then consider using a hash map or set 

# Example Problems

> [!Question]- Counting Elements
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given an integer array `arr`, count how many elements `x` there are, such that `x + 1` is also in `arr`. If there are duplicates in `arr`, count them separately.
> 
> ![[Drawing 2024-12-23 21.36.48.excalidraw | center | 250]]
>
>**~={red}Solution=~**:
>
>1. **~={blue}Store in Set=~**: Store all the elements in the array in a set
>2. **~={blue}Iterate through Input=~**: Iterate through input, if at value $x$ and value $x+1$ exists in set, increment `result`
>
>```cpp
>int countElements(vector<int​>& arr) {
>	// (1) Store in Set
>	unordered_set<int​> s{arr.begin(), arr.end()};
>	int result{0};
>
>	// (2) Iterate through Input
>	for (int a : arr) {
>		if (s.find(a + 1) != s.end()) {
>			result++;
>		}
>	}
>	return result;
>}
>```

#flashcards/dsa/patterns/hashing/checkingforexistence
