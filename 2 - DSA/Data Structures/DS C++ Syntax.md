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

|                Operation                 | Syntax                                                                              | Complexity | Comments |
| :--------------------------------------: | ----------------------------------------------------------------------------------- | ---------- | -------- |
|                1D Access                 | `v.at(index) / v[index]`<br><br>`v.front() / v.back()`                              | $O(1)$     |          |
|                2D Access                 | `v.at(rowIndex).at(colIndex) /`<br><br>`v[rowIndex][colIndex]`                      | $O(1)$     |          |
|                  Insert                  | `v.insert(v.begin() + index, value)`                                                | $O(n)$     |          |
|              Insert at End               | `v.push_back(value)`                                                                | $O(1)^A$   |          |
| Erase an Element or<br>Range of Elements | `v.erase(v.begin() + index)` <br><br>`v.erase(v.begin() + index, v.begin() + index` | <br>$O(n)$ |          |
|          Remove and Return End           | `v.pop_back()`                                                                      | $O(1)^A$   |          |
|                   Sort                   | `sort(v.begin(), v.end())`                                                          | $O(nlogn)$ |          |
|                 Reverse                  | `reverse(v.begin(), v.end())`                                                       | $O(n/2)$   |          |
|                 Min/Max                  | `*min_element(v.begin(), v.end())`                                                  | $O(n)$     |          |
|                   Swap                   | `swap(v[i], v[j])`                                                                  | $O(1)$     |          |

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

|                     Operation                      | Syntax                      | Comments |
| :------------------------------------------------: | --------------------------- | -------- |
|              Access Value<br>from Key              | `m.at[key]`<br><br>`m[key]` |          |
|                        Find                        | `m.find(key)`               |          |
|                       Insert                       | `m.insert({key, value})`    |          |
|                    Erase a Pair                    | `m.erase(key)`              |          |
| Count Number of Elements<br>Associated with<br>Key | `m.count(key)`              |          |

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

|                     Operation                      | Syntax            | Comments                    |
| :------------------------------------------------: | ----------------- | --------------------------- |
|                        Find                        | `s.find(value)`   | if(s.find(x) != s.end()) {} |
|                       Insert                       | `s.insert(value)` |                             |
|                    Erase a Pair                    | `s.erase(value)`  |                             |
| Count Number of Elements<br>Associated with<br>Key | `s.count(value)`  |                             |


