> [!QUOTE] Quick Notes
>When we recognise a FIFO pattern

# Overview
## Recipe

>[!Note]- Queue Recipe
> <!-- Multiline -->
>1. ~={purple}**Why Queue=~**: Whenever we recognise a FIFO pattern.

# Example Problems

> [!Question]- Number of Recent Calls (Data Stream)
> <!-- Multiline -->
> **~={red}Question=~**:
> * You have a `RecentCounter` class which counts the number of recent requests within a certain time frame.
> 	* `RecentCounter()` Initialises the counter with zero recent requests.
> 	* `int ping (int t)` Adds a new time request where `t` represents some time in milliseconds, and returns the number of requests that has happened in the past `3000` milliseconds (including the new request). **~={red}(Basically, return the number of requests within 3000 ms from t)=~**
>
>**~={red}Solution=~**:
>1. **~={purple}Queue Logic=~**:
> * **~={green}Add to Queue=~**: Always add to the queue
> * **~={red}Check the front of the queue=~**: if the element is not within 3000ms from the current time, pop() from the front, until the front is within 3000ms (Because of FIFO, all subsequent timestamps in the queue will be valid)
>
> ![[Drawing 2024-12-27 11.16.26.excalidraw | 700 | center]]
>```cpp
>class RecentCounter {
>private:
>	queue<int​> q;
>	
>public:
>	RecentCounter() :
>		q{} {}
>	
>	int ping(int t) {
>		q.push(t);
>		while (!q.empty() && q.front < t - 3000) {
>			q.pop();
>		}
>		return q.size();
>	}
>}
>```

> [!Question]- Moving Average from Data Stream (Data Stream)
> <!-- Multiline -->
> **~={red}Question=~**:
> * Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.
>
>**~={red}Solution=~**:
>1. **~={purple}Queue Logic=~**:
> * **~={green}Add to Queue=~**: Always add to the queue. Utilise a variable to keep track of the current sum in the queue.
> * **~={red}Check the size of the queue=~**: If the queue size exceeds the specified `size`, immediately pop.
>
> ![[Drawing 2024-12-27 11.56.51.excalidraw | 500 | center]]
>```cpp
>class MovingAverage {
>private:
>	queue<int​> q;
>	int size;
>	double sum;
>	
>public:
>	MovingAverage(int size) :
>		q{},
>		size{size},
>		sum{0} {}
>	
>	double next(int val) {
>		q.push(val);
>		sum += val;
>		
>		// If we exceed the "average window" size, pop from front
>		if (q.size() > size) {
>			sum -= q.front();
>			q.pop();
>		}
>		return sum / q.size();
>	}
>}
>```

> [!Question]- Double Queue (Monotonic): Dota2 Senate  
> <!-- Multiline -->  
> **~={red}Question=~**:  
> * You are given a string `senate`, where each character is:  
>   - **'R'** (Radiant senator).  
>   - **'D'** (Dire senator).  
> * Each senator can **ban** another senator from the opposite party.  
> * The **order** of banning is based on their positions in the string.  
> * The process continues until only one party remains.  
> * Return the **winning party** as `"Radiant"` or `"Dire"`.  
>  
> ~={red}**Solution**=~:  
>  ![[Drawing 2025-02-06 11.12.09.excalidraw | center | 500]]
> 1. ~={blue}**Store Senator Positions in Queues**=~:  
>    - Use two queues (`rQueue` for Radiant, `dQueue` for Dire).  
>    - Store the **index** of each senator.  
> 2. ~={blue}**Simulate the Voting Process**=~:  
>    - Pop the **earliest senator** from both queues.  
>    - The senator with the **earlier index bans the later one**.  
>    - The surviving senator is added **back to their queue**, but their new index is **shifted forward** by `n` (cycle continuation).  
> 3. ~={blue}**Continue Until One Party Remains**=~:  
>    - The process repeats until one queue is empty.  
> 4. ~={blue}**Return the Winner**=~:  
>    - If the `rQueue` is empty, `"Dire"` wins; otherwise, `"Radiant"` wins.  
>  
> **~={green}Code=~**:  
> ```cpp  
> class Solution {  
> public:  
>     string predictPartyVictory(string senate) {  
>         int n = senate.size();  
>         deque<int​> rQueue, dQueue;  
>  
>         // (1) Store senator indices in queues  
>         for (int i{0}; i < senate.size(); ++i) {  
>             if (senate[i] == 'R') rQueue.push_back(i);  
>             else dQueue.push_back(i);  
>         }  
>  
>         // (2) Process eliminations  
>         while (!rQueue.empty() && !dQueue.empty()) {  
>             int earliestR = rQueue.front();  
>             int earliestD = dQueue.front();  
>             rQueue.pop_front();  
>             dQueue.pop_front();  
>  
>             // (3) The earlier senator bans the later one  
>             if (earliestR < earliestD) {  
>                 earliestR += n;  // Move to next round  
>                 rQueue.push_back(earliestR);  
>             } else {  
>                 earliestD += n;  
>                 dQueue.push_back(earliestD);  
>             }  
>         }  
>  
>         return rQueue.empty() ? "Dire" : "Radiant";  
>     }  
> };  
> ```  
>  
> ~={green}**Key Points**=~:  
> * ~={blue}**Time Complexity**=~:  
>   - **O(n)**: Each senator is processed once per round.  
>   - **Worst case**: It runs `O(n)` rounds, leading to **O(n²)**.  
> * ~={blue}**Space Complexity**=~:  
>   - **O(n)**: Two queues store senator indices.  

#flashcards/dsa/patterns/queue
