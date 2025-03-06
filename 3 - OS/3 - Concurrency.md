# Race Condition and Critical Section

>[!Note]- What is a Race Condition?
> <!-- Multiline -->
> Occurs when multiple threads or processes access and modify shared data concurrently, and the final outcome depends on the order of execution.

>[!Note]- What is a Critical Section?
> <!-- Multiline -->
> A section of code that accesses shared data (or a shared resource) that must not be accessed by more than one thread at the same time. The idea of a critical section can be implemented using locks.

# Locks and Synchronisation

## Creating Threads

````col
```col-md
**~={purple}Creating a Singe Thread=~**
```cpp
#include <thread>

void workerFunc(int x) {
	cout << x << "\n";
}

int main() {
	
	thread worker{workerFunc, 25};
	worker.join();

	return 0
}
```

```col-md
**~={purple}Creating Multiple Threads=~**
```cpp
#include <thread>

void workerFunc(int x) {
	cout << x << "\n";
}

int main() {

	int num{5};
	vector<thread> threads;
	
	for (int i{0}; i < num; ++i) {
		threads.push_back
		(thread{workerFunc, 5});
	}

	for (auto &t : threads) {
		t.join();
	}

	return 0
}
```
````

## How to Prevent Race Condition

### **~={purple}Mutex=~**

* **~={green}Definition=~**: A mutex is a locking mechanism that will **~={red}block=~** other threads from entering the critical section if the mutex is unavailable to be acquired. Only 1 thread can acquire a mutex at a time.
	* `mutex`: Type for a mutex
	* `lock_guard<mutex>`: Locks the mutex when it is created and automatically unlocks it when it goes out of scope.

```cpp
#include <mutex> 
#include <thread>

mutex mtx;
int sharedData{0};

void workerFunc() {
	for (int i{0}; i < 100; ++i) {
		lock_guard<mutex> guard{mtx}; // 
		// Critical Section Entered
		++sharedData;
		// Leave Critical Section as we move to next Iteration
	}
}

int main() {
	thread t1{workerFunc};
	thread t2{workerFunc};
	
	t1.join();
	t2.join();
}

```

**~={purple}Manually Locking and Unlocking=~**

```cpp
mutex mtx;
int sharedData = 0;

void workerFunc() {
  for (int i = 0; i < 100; ++i) {
    mtx.lock();
    // Critical section
    sharedData++;
    mtx.unlock();
  }
}
int main() {
  thread t1(workerFunc);
  sthread t2(workerFunc);
  
  t1.join();
  t2.join();
}
```

### **~={purple}Condition Variables=~**

* **~={green}Definition=~**: is used for thread synchronisation, allowing threads to wait for certain conditions to be true before proceeding

Waiting to Proceed:

```cpp
mutex mtx;
unique_lock<mutex> lock{mtx};
cv.wait(lock, []{ return ready; }); // Wait until `ready` is true

// If unique_lock goes out of scope, it automically releases the lock
```

What to do after proceeding and releasing the lock:

```cpp
cv.notify_one(); // Wake up one waiting thread
cv.notify_all(); // Wake up all waiting threads
```

#### **~={purple}C++ Lambdas=~**

```cpp
[capture](parameters) -> return_type { body; };
```

* **~={blue}capture=~**: Variables from outer scope.
* **~={blue}parameters=~**: Function parameters (like normal functions).
* **~={blue}return_type=~**: (Optional) Type of the return value.
* **~={blue}body=~**: The function code.

~={purple}**Capture Values**=~

````col
```col-md
* `[ ]` - Captures nothing.
* `[=]` - Captures all outer variables by value (copy)
* `[&]` - Captures all outer variables **by reference**.
```
```col-md
* `[this]` - Captures the current object (this pointer).
* `[x]` - Captures variable x by value
* `[&x]` - Captures variable x by reference
```
````

**~={red}Examples=~**

1. Sorting a Vector in Decreasing Order

```cpp
sort(nums.begin(), nums.end(), [](int a, int b) {
	return a > b;
});
```

2. Square a Vector

```cpp
for_each(nums.begin(), nums.end(), [](int &n) {
    n = n * n;
});
```

3. Condition Variable
* Forces `first` to be run before `second`

```cpp
mutex mtx;
condition_variable cv;
int count{0};

void first(function<void()> printFirst) {
	lock_guard<mutex> guard{mtx}
	count++;
	printFirst(); 
	// Notifies all other active threads
	cv.notify_all();
	// Releases lock
}

void second(function<void()> printSecond) {
	unique_lock<mutex> guard{mtx};
	// Once the thread gets here, it can only progress 
	cv.wait(lock, [](){ return count == 1; });
	count++;
	printSecond();
	
	cv.notify_all();
}
```

### **~={purple}Semaphore=~**

* **~={green}Definition=~**: A semaphore is a locking mechanism that will **~={red}block=~** other threads from entering the critical section if the semaphore is unavailable to be acquired. Only $n$ threads can acquire the semaphore at a time.
	* `counting_semaphore<n>`: Type for semaphore, and specifies max number of threads allowed in the critical section

```cpp
#include <sempahore> 
#include <thread>

counting_semaphore<2> sem{2}; // 2 threads allowed
int sharedData{0};

void workerFunc() {
	for (int i{0}; i < 100; ++i) {
		sem.acquire(); // Decrement permit, might block if 0
		// Critical Section Entered
		++sharedData;
		sem.release() // Increment permit
		// Leave Critical Section as we move to next Iteration
	}
}

int main() {
	thread t1{workerFunc};
	thread t2{workerFunc};
	thread t3{workerFunc};
	
	t1.join();
	t2.join();
	t3.join();
}

```

## Others

>[!Question]- Producer and Consumer Problem
> <!-- Multiline -->

# Deadlock & Livelock

>[!Note]- Conditions for Deadlock
> <!-- Multiline -->
> **~={purple}What is Deadlock?=~**
>  A circle of processes each holding a resource wanted by another process, causing an endless loop of waiting.
> 
> **~={purple}What is Starvation?=~**
> While a resources may be available, the process never gets its turn to use them due to scheduling or allocation policies that repeatedly favour other processes.
> 
> ![[Drawing 2025-02-23 15.39.10.excalidraw | center | 250]]
> 
> Deadlock can only arise if all 4 conditions hold:
> 1. **~={blue}Hold & Wait=~**: A process can hold a resource while requesting another simultaneously
> 2. **~={blue}Mutual Exclusion=~**: Only one thread can use a resource at a time
> 3. **~={blue}No Pre-Emption=~**: Only the owner can release the resource/the process can release it's resource.
> 4. **~={blue}Circular Wait=~**: There is a circular list of processes each wanting a resource owned by another in the list.

>[!Note]- How to Prevent Deadlock
> <!-- Multiline -->

>[!Note]- How to Recover From Deadlock
> <!-- Multiline -->

>[!Note]- What is Livelock?
> <!-- Multiline -->

>[!Question]- Dining Philosophers (Deadlock Prevention)
> <!-- Multiline -->
> If we had $x$ diners, where between each diner is a shared fork. In order to eat, a diner needs 2 forks.
> * The value of the fork to the left is the same as the diner
> * The value of the fork to the right is (D Value + 1) % Num people
> 
>  ![[Drawing 2025-03-01 01.17.16.excalidraw | center | 250]]
>  
> **~={purple}When Deadlock Occurs=~**
> If we simply design it so that, when a diner wants to eat, they:
> 1. Request for left fork
> 2. Request for right fork
> 
> If each diner grabs their left fork, before any diner can grab their right fork (assume diners are facing inwards):
> 
> ![[Drawing 2025-03-01 07.37.10.excalidraw | center | 250]]
> 
> Then no diner will be able to grab their right fork, and thus no one will be able to eat, as they are all waiting for another diner to forego their fork, and hence everyone would starve.
> 
> **~={purple}How to Prevent Deadlock=~**
> Currently every diner tries to obtain the left fork first. We will break the Circular Wait requirement of deadlock by making it so one of the diners will request the right fork first.
> 
> ![[Drawing 2025-03-01 16.15.13.excalidraw | center | 400]]
> 
> This works as:
> ````col
>```col-md
>**~={red}Before=~**
>1. D0 picks F0 (Waits for F1)
>2. D1 picks F1 (Waits for F2)
>3. D2 picks F2 (Waits for F3)
>4. D3 picks F3 (Waits for F0)
>
>D0 -> D1 -> D2 -> D3 -> D0 (Each diner is waiting for another diner to forego their lock)
>```
>```col-md
>**~={green}After=~**
>1. D0 picks F1 (Waits for F0)
>2. D1 tries to pick F1 (Must wait for D0)
>3. D0 picks F0 -> Eats -> Foregoes both fork
>4. ...
>
>At least one philosopher (D0, in this case) will eventually acquire both forks and eat, which breaks the dependency chain.
>```
>````
>
>**~={purple}Code=~**
>
>```cpp
>const int numPhilosophers = 5;
>// Each fork is a mutex
>mutex forks[numPhilosophers];
>
>void dine(int philosopher) {
>	int left = philosopher;
>	int right = (philosopher + 1) % numPhilosopher;
>	
>	// Last philosopher picks up the right fork first instead
>	if (philosopher == 5) {
>		forks[right].lock();
>		forks[left].lock();
>	} else {
>		// Pickup left then right fork
>		forks[left].lock();
>		forks[right].lock();
>	}
>	
>	// Eat
>	
>	// Finished eating
>	forks[left].unlock();
>	forks[right].unlock();
>}
>
>int main() {
>	thread philosophers[numPhilosophers];
>	
>	for (int i = 0; i < numPhilosophers; ++i) {
>		philosophers[i] = thread(dine, i);
>	}
>	
>	for (int i = 0; i < numPhilosophers; ++i) {
>		philosophers[i].join();
>	}
>}
>```

>[!Question]- Banker's Algorithms (Deadlock Avoidance)
> <!-- Multiline -->

# Lock Free Concurrency

Atomic Operations

CAS (Compare and Swap)

Spin Lock

# Parallelism vs Concurrency


#flashcards/os/concurrency