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

## Methods

# Sets
## Initialisation

## Methods


