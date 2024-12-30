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

#flashcards/dsa/patterns/queue
