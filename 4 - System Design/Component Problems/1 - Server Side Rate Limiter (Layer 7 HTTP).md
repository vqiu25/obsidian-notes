> [!quote]+ Overview
>````col 
>```col-md 
> **~={blue}Description=~**:
> * A rate limiter is used to **~={red}control the traffic=~** sent by a client or service. In the HTTP world, a rate limiter limits the number of client requests allowed to be **~={red}sent over a specified period=~**. If the API request count exceeds the threshold defined by the rate limiter, all the excess calls are **~={red}blocked=~**.
>``` 
>```col-md 
>**~={green}Benefits=~**:
>* **~={green}Prevent DDOS=~**: Reduce servers from being overload.
>* **~={green}Reduce Cost=~**: If you using 3rd party api, API are charged by API usage.
>``` 
>```` 

# High Level Design

> [!Note]- Client vs Server Side Rate Limiter
> <!-- Multiline -->
>````col 
>```col-md 
>~={red}Client Side Rate Limiter=~
>* Not a great place to put a rate limiter, as we may not even have control over the rate limiter, and can be modified by malicious actors
>
>``` 
>```col-md 
>~={green}Server Side Rate Limiter=~
>* Better
>``` 
>```` 

> [!note]- High Level Design
> <!-- Multiline -->
>````col 
>```col-md 
>~={purple}Server Side Rate Limiter=~
>* Put the rate limiter code on the server. 
>* **~={green}Pros=~**: Simpler, **~={red}Cons=~**: Less Modular
> ![[Drawing 2025-02-10 08.05.35.excalidraw]]
>
>``` 
>```col-md 
>~={purple}Middleware Rate Limiter=~
>* Have the rate limiter code on a seperate server (API gateway)
>* **~={green}Pros=~**: Scalable, **~={red}Cons=~**: Higher Latency
> ![[Drawing 2025-02-10 08.13.39.excalidraw]]
>``` 
>```` 
>

# Algorithms for Rate Limiting

>[!Note]- Token Bucket
> <!-- Multiline -->
>**~={purple}Description=~**
>* **~={green}Definition=~**: Maintains a bucket with a fixed capacity of tokens, where each request consumes a token, and tokens are replenished at a constant rate
>* **~={blue}Number of Buckets=~**:
>	* We usually have a bucket per API endpoint. 
>	* We can have a global bucket if the intention is to set a global limit of X requests per second. 
>	* If we want throttle per IP address, then each IP address needs a bucket.
>
>**~={green}Inputs=~**:
>* **~={purple}Bucket Size=~**: Max number of tokens in a bucket
>* **~={purple}Refill Rate=~**: Number of tokens put into the bucket every second
>
>**~={blue}Considerations=~**
>* **~={green}Pros=~**: Easy to implement and memory efficient
>* **~={red}Cons=~**: Setting the bucket size and refill rate is difficult
>
> ![[Drawing 2025-02-10 08.23.58.excalidraw | center | 450]]
> 
> **~={red}Example=~**
>  ![[Drawing 2025-02-10 08.29.09.excalidraw | center | 700]]

>[!Note]- Leaking Bucket
> <!-- Multiline -->
>**~={purple}Description=~**
>* **~={green}Definition=~**: Similar to Token Bucket, but we have a queue rather than tokens. Requests are added to the queue, and acted on at a constant rate.
>* **~={blue}Number of Buckets=~**:
>	* We usually have a bucket per API endpoint. 
>	* We can have a global bucket if the intention is to set a global limit of X requests per second. 
>	* If we want throttle per IP address, then each IP address needs a bucket.
>
>**~={green}Inputs=~**:
>* **~={purple}Bucket Size=~**: Max number of tokens in a bucket
>* **~={purple}Outflow Rate=~**: Defines how many requests can be processed at a fixed rate, usually in seconds.
>
>**~={blue}Considerations=~**
>* **~={green}Pros=~**: Easy to implement and memory efficient (one queue). Good if stable outflow rate is required.
>* **~={red}Cons=~**: Tuning input parameters is hard. If a congestion of old requests stuff up the queue, recent requests will be rate limited immediately.
>
> ![[Drawing 2025-02-10 18.56.50.excalidraw | center | 550]]

>[!Note]- Fixed Window Counter
> <!-- Multiline -->
>**~={purple}Description=~**
>* **~={green}Definition=~**: Works as follows:
>	1. The algorithm divides the timeline into fix-sized time windows and assign a counter for each window.
>	2. Each request increments the counter by one.
>	3. Once the counter reaches the pre-defined threshold, new requests are dropped until a new window starts
>
>* **~={green}Inputs=~**:
>	* **~={purple}Time Units=~**: How we break down time (seconds).
>	* **~={purple}Max Num Of Requests Per Time Unit=~**: How many requests we allow per second. 
>
>**~={blue}Considerations=~**
>* **~={green}Pros=~**: Memory efficient. Resetting quota per time window has its use cases. (i.e. API only allows it X number of times per time window)
>* **~={red}Cons=~**: Spike in traffic at edges could allow more requests then allowed.
>
> ![[Drawing 2025-02-10 20.43.21.excalidraw | center | 450]]
> 
> **~={red}Problem=~**
> * Edges of time windows can allow more requests to pass.
> * If we allow only 5 requests per minute, between X:00:30 and X:01:30, would accept 10 requests, which is **~={red}twice=~** as many as allowed.
> 
> ![[Drawing 2025-02-10 20.42.41.excalidraw | center | 450]]
> 

>[!Note]- Sliding Window Log
> <!-- Multiline -->
>**~={purple}Description=~**
>* **~={green}Definition=~**: Works as follows:
>	1. The algorithm keeps track of request timestamps. Timestamp data is usually kept in cache.
>	2. When a new request comes in, remove all the outdated timestamps. Outdated timestamps are defined as those older than the start of the current time window.
>	3. Add timestamp of the new request to the log.
>	4. If the log size is the same or lower than the allowed count, a request is accepted. Otherwise, it is rejected.
>
>* **~={green}Inputs=~**:
>	* **~={purple}Time Units=~**: How we break down time (minute).
>	* **~={purple}Max Num Of Requests Per Time Unit=~ (Log Size)**: How many requests we allow per minute. 
>
>**~={blue}Considerations=~**
>* **~={green}Pros=~**: Super accurate rate limiting
>* **~={red}Cons=~**: Storing logs require a lot of memory, as even if a request is rejected, it's still stored in the log
>
> ![[Drawing 2025-02-11 08.06.19.excalidraw | center | 700]]
> 

>[!Note]- Sliding Window Counter
> <!-- Multiline -->
>* **~={green}Inputs=~**:
>	* **~={purple}Time Units=~**: How we break down time (minute).
>	* **~={purple}Max Num Of Requests Per Time Unit=~ (Log Size)**: How many requests we allow per minute. 
>
>**~={blue}Considerations=~**
>* **~={green}Pros=~**: It smooths out spikes in traffic, preventing, Fixed Window Counter Spike issue.
>* **~={red}Cons=~**: May not be accurate. Assumes the distribution of requests in the previous minute window is evenly distributed.
>
> ![[Drawing 2025-02-11 08.17.04.excalidraw | center | 550]]
> 

# Rate Limiting in a Distributed Environment

>[!Note]- Race Condition
> <!-- Multiline -->
> **~={purple}Problem=~**
> 1. We read from the **~={red}Rate Limiter cache=~** as to how many requests have been sent so far in this current time frame
> 2. If we are under the limit, we'll increment that counter. However this is susceptible to race condition
> 
> ![[Drawing 2025-02-12 08.04.44.excalidraw | center | 350]]
> 
> **~={purple}Solution=~**
> 1. **~={green}Locks=~**: Feasible, but will significantly slow down the system
> 2. **~={green}Lua Script=~**: Lua Scripts can be used to turn the `read` and `increment` into one **~={red}atomic operation=~**. The rate limiter cache stores the Lua Script, and execute the script on an API request.

>[!Note]- Synchronisation
> <!-- Multiline -->
> **~={purple}Problem=~**
> * Because the servers are **~={red}stateless=~**, this means that if we wanted to rate limit on a client by client basis, if we have multiple rate limiters, and clients connect to multiple rate limiters, they'll be incrementing counters all over the place.
> 
> **~={purple}Solution=~**
> * **~={green}Fix each client to 1 rate limiter=~**: However this isn't very flexible or scalable
> * **~={green}Shared data store=~**: Have the counters stored in a shared cache/datastore.

# Detailed Design

>[!Note]- Detailed Design
> <!-- Multiline -->
>**~={purple}Explanation=~**
>* **~={green}Rules=~**: Such as **~={red}number of requests allowed per minute=~**. Stored on disk, and can be updated by worker servers. They frequently look at the disk, and put the latest rules in it's cache.
>* ~={green}Rate Limiter Parameters=~: Such as last request timestamp, and a counter for how many requests received so far. Stored in in-memory of cache of the rate limiter.
>* **~={blue}Steps=~**:
>	1. Client sends requests through rate limiter
>		1. **~={green}Success=~**: Forwarded to API servers
>		2. **~={red}Failure=~**: Receive 429 Too Many Requests HTTP response
>
> ![[Drawing 2025-02-11 17.18.10.excalidraw | center | 700]] 
