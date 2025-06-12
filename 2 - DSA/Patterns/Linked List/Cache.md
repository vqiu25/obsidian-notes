# Example Problems

> [!Question]- LRU Cache
> <!-- Multiline -->
> **~={red}Question=~**:
> * Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.
> 	* `LRUCache(int capacity)` Initialise the LRU cache with **positive** size `capacity`.
> 	* `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
> 	* `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.
> 
> ![[Drawing 2025-05-27 17.28.01.excalidraw | center | 400]]
> 
>**~={red}Solution=~**:
>1. **~={blue}Design=~**:
>* `list<pair<int, int>>`: Is the cache itself, where every element is a `(key, value)` pair. 
>	* **~={red}Head=~**: Least recently used, Tail: **~={green}Most=~** recently used
>	* We use a linked list as we may need to move an existing key value pair to the back of the linked list in O(1)
>* `unordered_map<int, list<pair<int, int>>::iterator>`: Key to the location of where the element is in the doubly linked list.
>
>```cpp
>class LRUCache {
>private:
>	int capacity;
>	list<pair<int, int​>​> cache;
>	unordered_map<int, list<pair<int, int​>​>::iterator​> keyToLoc;
>
>public:
>	LRUCache(int capacity) : capacity{capacity} {}
>	
>	int get(int key) {
>		auto it = keyToLoc.find(key);
>		if (it == keyToLoc.end()) return -1;
>	
>		// Now that we've found it, we need to:
>		// Retrieves the iterator from the map, then the second value
>		// from the pair
>		int valueToReturn = it->second->second;
>		// 1. Update to most recent
>		cache.erase(it->second);
>		cache.push_back({key, valueToReturn});
>		keyToLoc[key] = prev(cache.end());
>		
>		// 2. Return it's value
>		return valueToReturn;
>	}
>	
>	int put(int key, int value) {
>		// 1. Check if it already exists, if so update to most recent
>		auto it = keyToLoc.find(key);
>		if (it != keyToLoc.end()) {
>			cache.erase(it->second);
>			keyToLoc.erase(it);
>		}
>		
>		// 2. If the cache is already full, evict
>		if (cache.size() == capacity) {
>			auto cacheFront = cache.begin();
>			keyToLoc.erase(cacheFront->first);
>			cache.pop_front();
>		}
>		
>		cache.push_back({key, value});
>		keyToLoc[key] = prev(cache.end());
>	}
>}
>```


#flashcards/dsa/patterns/linkedlist/cache