# Structs
## Initialisation

* By default, all attributes and methods are **~={green}public=~**

```cpp
struct N {
	int x, y;
	
	// Comparison Functions
	// (Sort by X, then Y)
	bool operator<(const N &n) {
		if (x != n.x) return x < n.x;
		else return y < n.y;
	}
}
```

If we were sorting a `vector` of structs of type `N`. If we had the two `structs`:

**<font color="#b4befe">Primary Condition</font>**

- If `x` values are not equal (`x != n.x`), the comparison is based on `x` values:
    - `return x < n.x` ensures sorting by the `x` attribute in **ascending order**.
        - <font color="#86b0f9">Explanation</font>
            - `x` refers to the `x` value of the <font color="#a6e3a1">first</font> struct (`this` instance).
            - `n.x` refers to the `x` value of the <font color="#f38ba8">second</font> struct (the one being compared).
            - If `x < n.x` is `true`, the first struct (`this`) comes before the second struct (`n`) in the sorted order.
    - **Why `<` gives ascending order**: Using `<` means smaller values are placed earlier in the sequence, resulting in an order from smallest to largest (ascending).
    - 

**<font color="#b4befe">Secondary Condition</font>

- If `x` values are equal, the comparison moves to `y`:
    - `return y < n.y` ensures sorting by the `y` attribute when `x` values are identical.
        - **<font color="#86b0f9">Explanation</font>**:
            - `y` refers to the `y` value of the <font color="#a6e3a1">first</font> struct (`this` instance).
            - `n.y` refers to the `y` value of the <font color="#f38ba8">second</font> struct.
            - If `y (first struct) < n.y (second struct)` is `true`, the first struct comes before the second struct in the sorted order.

## Custom Sorting of Strings and Vectors

```cpp
bool comp (string a, string b) {
	// Primary Sort: Length
	if (a.size() != b.size()) return a.size() < b.size();
	// Secondary Sort: Lexicographic
	return a < b;
}

int main() {
	sort(v. begin(), v.end(), comp);
}
```

## Lambda Sorting

```cpp
// If connections was vector<vector<int>> type
// Sort in desending order depending on index 2 values

sort(connections.begin(), connections.end(), [&](auto &a, auto &b) {
// Index 2 smaller value goes first
return a[2] < b[2];

});
```

# Array
## Initialisation
## Methods

# Vector
## Initialisation
### 1D Vector

| Operation              | Syntax                                                                | Comments                                      |
| ---------------------- | --------------------------------------------------------------------- | --------------------------------------------- |
| Declaration            | `vector<DT> v`                                                        |                                               |
| Uniform Initialisation | `vector<DT> v{1, 2, 3}`                                               |                                               |
| Direct Initialisation  | `vector<DT> v(3) = {0, 0, 0}`<br><br>`vector<DT> v(3, 1) = {1, 1, 1}` | Right side is <br>what it would <br>look like |
### 2D Vector

| Operation              | Syntax                                        | Comments          |
| ---------------------- | --------------------------------------------- | ----------------- |
| Declaration            | `vector<vector<DT>> v`                        |                   |
| Uniform Initialisation | `vector<vector<DT>> v{{1, 2}, {2, 3}, {3,4}}` | 1,2<br>2,3<br>3,4 |
| Direct Initialisation  | `vector<vector<int>> v(n, vector<int>(n, 0))` | n rows<br>m cols  |

## Methods

|                Operation                 | Syntax                                                                              | Complexity | Comments     |
| :--------------------------------------: | ----------------------------------------------------------------------------------- | ---------- | ------------ |
|                1D Access                 | `v.at(index) / v[index]`<br><br>`v.front() / v.back()`                              | $O(1)$     |              |
|                2D Access                 | `v.at(rowIndex).at(colIndex) /`<br><br>`v[rowIndex][colIndex]`                      | $O(1)$     |              |
|                  Insert                  | `v.insert(v.begin() + index, value)`                                                | $O(n)$     |              |
|              Insert at End               | `v.push_back(value)`                                                                | $O(1)^A$   |              |
| Erase an Element or<br>Range of Elements | `v.erase(v.begin() + index)` <br><br>`v.erase(v.begin() + index, v.begin() + index` | <br>$O(n)$ |              |
|          Remove and Return End           | `v.pop_back()`                                                                      | $O(1)^A$   |              |
|                   Sort                   | `sort(v.begin(), v.end())`                                                          | $O(nlogn)$ |              |
|                 Reverse                  | `reverse(v.begin(), v.end())`                                                       | $O(n/2)$   |              |
|                 Min/Max                  | `*min_element(v.begin(), v.end())`                                                  | $O(n)$     |              |
|                   Swap                   | `swap(v[i], v[j])`                                                                  | $O(1)$     |              |
|             Check if Sorted              | `is_sorted(v.begin(), v.end())`                                                     | $O(n)$     | Returns bool |

# Strings
## Initialisation

| Operation              | Syntax              | Comments        |
| ---------------------- | ------------------- | --------------- |
| Declaration            | `string s`          |                 |
| Uniform Initialisation | `string s{"test"};` |                 |
| Direct Initialisation  | `string s(n, 'a')`  | n values of 'a' |

## Methods

|                     Operation                      | Syntax                                                                                                                           | Complexity | Comments                                                                                  |
| :------------------------------------------------: | -------------------------------------------------------------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------------- |
|                     Access/Set                     | = `s[x]`<br>`s[x]` =                                                                                                             | $O(1)$     |                                                                                           |
|                        Add                         | `s += "x"`                                                                                                                       | $O(m)$     | $m$ = length of string being appended                                                     |
|                       Insert                       | `s.insert(pos, str)`                                                                                                             | $O(n)$     |                                                                                           |
|      Erase an Element or<br>Range of Elements      | `s.erase(x, y)`                                                                                                                  | <br>$O(n)$ | Erase $y$ characters from index $x$                                                       |
| Move all of one character to the end of the String | `remove(s.begin(), s.end(), '0');`<br><br>// Moves all 0s to end of the string and returns an iterator pointing to the first '0' |            | We can erase all 0's by doing<br><br>`s.erase(remove(s.begin(), s.end(), '0'), s.end());` |
|                 Extract Substring                  | `string sub = s.substr(6, 5)`<br><br>// (Start Index, Length)                                                                    | $O(m)$     | $m$ = substring length                                                                    |
|                   Find Substring                   | `size_t pos = s.find("word")`<br><br>`if (pos != string::npos) { // "word found" }`                                              | $O(nm)$    | Check $n$ positions for $m$ length                                                        |
|                 Replace Substring                  | `s.replace(7, 5, "C++")`<br><br>// (Start Index, Length of What is Being Removed, "String that is being put there")              | $O(n + m)$ | Shifting $n$ elements and inserting $m$ elements                                          |
|            String to Integer/Long Long             | `stoi("x")`<br><br>`stoll("2342342")`                                                                                            | $O(1)$     |                                                                                           |
|                 Integer to String                  | `to_string(5)`                                                                                                                   | $O(1)$     |                                                                                           |
|                 Lexicographic Sort                 | `sort(s.begin(), s.end())`                                                                                                       | $O(nlogn)$ |                                                                                           |
## URI

Finds:
* http://<font color="#f38ba8">news.yahoo.com</font>/news

```cpp
string getHostname(const string& url) {
	int start = url.find("http://") + 7; // Start after "http://"
	int end = url.find("/", start); // Find first '/' after "http://"
	return url.substr(start, end - start);

}
```
# Characters

## Techniques and Methods

|            Operation             | Syntax                                                        | Complexity | Comments                                               |
| :------------------------------: | ------------------------------------------------------------- | ---------- | ------------------------------------------------------ |
|       Convert Char to Int        | '5' - '0' -> 5                                                | $O(1)$     |                                                        |
|       Convert Int to Char        | 5 + '0' -> '5'                                                | $O(1)$     | `to_string('c')`                                       |
| Wrap an Integer Between (0 - 9)  | (x + 10) % 10<br><br>// X = integer                           | $O(1)$     | The + 10, helps with wrapping -1 to 9                  |
| Convert Char to Lower/Upper Case | `toupper(char)`<br><br>`tolower(char)`<br><br>`isupper(char)` | $O(1)$     |                                                        |
|    If a Char is Alphanumeric     | `isalpha(char`                                                | $O(1)$     | Checks if char is a number or a letter of the alphabet |
# Maps
## Initialisation
### Unordered Map

| Operation              | Syntax                                        | Comments         |
| ---------------------- | --------------------------------------------- | ---------------- |
| Declaration            | `unordered_map<DT1, DT2> m`                   |                  |
| Uniform Initialisation | `unordered_map<DT1, DT2> m{{1,'a'}, {2, 'b}}` | 1 -> a<br>2 -> b |

## Methods

* **~={purple}Unordered Map=~**:
	* **~={green}Average Case=~**: $O(1)$
	* **~={red}Worst Case=~**: $O(n)$

|        Operation         | Syntax                      | Comments                |
| :----------------------: | --------------------------- | ----------------------- |
| Access Value<br>from Key | `m.at[key]`<br><br>`m[key]` |                         |
|           Find           | `m.find(key)`               |                         |
|          Insert          | `m.insert({key, value})`    |                         |
|       Erase a Pair       | `m.erase(key)`              |                         |
|   Check if key exists    | `m.count(key)`              | Returns a bool to check |

# Sets
## Initialisation

### Unordered Set

| Operation              | Syntax                      | Comments |
| ---------------------- | --------------------------- | -------- |
| Declaration            | `unordered_set<DT> s`       |          |
| Uniform Initialisation | `unordered_set<DT> s{1, 2}` |          |

## Methods

* **~={purple}Unordered Map=~**:
	* **~={green}Average Case=~**: $O(1)$
	* **~={red}Worst Case=~**: $O(n)$

|                     Operation                      | Syntax                                | Comments                    |
| :------------------------------------------------: | ------------------------------------- | --------------------------- |
|                        Find                        | `s.find(value)`                       | if(s.find(x) != s.end()) {} |
|                       Insert                       | `s.insert(value)`                     |                             |
|                    Erase a Pair                    | `s.erase(value)`                      |                             |
| Count Number of Elements<br>Associated with<br>Key | `s.count(value)`                      |                             |
|                 Convert to Vector                  | `vector<int>(set.begin(), set.end())` | $O(n)$                      |

# Stack
## Initialisation

| Operation              | Syntax              | Comments |
| ---------------------- | ------------------- | -------- |
| Declaration            | `stack<DT> s`       |          |
| Uniform Initialisation | `stack<DT> s{1, 2}` |          |

## Methods

| Operation  | Syntax          | Complexity | Comments |
| :--------: | --------------- | ---------- | -------- |
|  Find Top  | `s.top()`       | $O(1)$     |          |
| Add to Top | `s.push(value)` | $O(1)$     |          |
| Remove Top | `s.pop(value)`  | $O(1)$     |          |
# Queue
## Initialisation

| Operation              | Syntax              | Comments |
| ---------------------- | ------------------- | -------- |
| Declaration            | `queue<DT> q`       |          |
| Uniform Initialisation | `queue<DT> q{1, 2}` |          |

## Methods

|  Operation   | Syntax          | Complexity | Comments |
| :----------: | --------------- | ---------- | -------- |
|  Find Front  | `q.front()`     | $O(1)$     |          |
|  Find Back   | `q.back()`      | $O(1)$     |          |
| Add to Back  | `q.push(value)` | $O(1)$     |          |
| Remove Front | `q.pop()`       | $O(1)$     |          |
# Deque

## Initialisation

| Operation              | Syntax              | Comments |
| ---------------------- | ------------------- | -------- |
| Declaration            | `deque<DT> d`       |          |
| Uniform Initialisation | `deque<DT> q{1, 2}` |          |
|                        |                     |          |

## Methods

|     Operation     | Syntax                                    | Complexity | Comments |
| :---------------: | ----------------------------------------- | ---------- | -------- |
|    Find Front     | `d.front()`                               | $O(1)$     |          |
|     Find Back     | `d.back()`                                | $O(1)$     |          |
|   Add to Front    | `d.push_front(value)`                     | $O(1)$     |          |
|    Add to Back    | `d.push_back(value)`                      | $O(1)$     |          |
|   Remove Front    | `d.pop_front()`                           | $O(1)$     |          |
|    Remove Back    | `d.pop_back()`                            | $O(1)$     |          |
|      Access       | `d[index]`                                | $O(1)$     |          |
| Convert to Vector | `vector<int>(deque.begin(), deque.end())` | $O(n)$     |          |
# Priority Queue

## Initialisation

| Operation              | Syntax                                          | Comments |
| ---------------------- | ----------------------------------------------- | -------- |
| Declaration (Max Heap) | `priority_queue<DT> h`                          |          |
| Declaration (Min Heap) | `priority_queue<DT, vector<DT>, greater<DT>> h` |          |
| Uniform Initialisation | `priority_queue<DT> h{1, 2}`                    |          |

## Methods

|   Operation    | Syntax      | Complexity  | Comments      |
| :------------: | ----------- | ----------- | ------------- |
|  Find Max/Min  | `h.top()`   | $O(1)$      |               |
|     Insert     | `h.push(x)` | $O(log(n))$ | Calls Heapify |
| Remove Max/Min | `h.pop()`   | $O(log(n))$ | Calls Heapify |


# Universal STL Methods

|      Operation      | Syntax      | Complexity | Comments                          |
| :-----------------: | ----------- | ---------- | --------------------------------- |
| Remove all Elements | `v.clear()` | $O(n)$     | Calls destructor on every element |
|        Size         | `v.size()`  | $O(1)$     | Stored variable                   |
|       Empty?        | `v.empty()` | $O(1)$     | Check stored variable             |

~={purple}**Vectors, Strings and Deque**=~

```cpp
// Vector Usage
vector<int> vec = {1, 2, 3, 4, 5};
vector<int> newVec;
newVec.assign(vec.begin(), vec.begin() + 3); // Copies {1, 2, 3}

// Stack Usage
string s;
deque<char> stack;
s.assign(stack.begin(), stack.end()) // Copies the Stack to the String

```
