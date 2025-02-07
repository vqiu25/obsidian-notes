![[Pasted image 20250208090359.png]]

>[!Info]- (0) Overall System
> <!-- Multiline -->
> 1. A user gets the IP address of the load balancer from DNS.
> 2. A user connects the load balancer with this IP address.
> 3. The HTTP request is routed to either Server 1 or Server 2.
> 4. A web server reads user data from a slave database.
> 5. A web server routes any data-modifying operations to the master database. This includes write, update, and delete operations.
> 6. An HTTP request is then sent from the web server, through the load balancer, back to the user.

>[!Info]- (1, 2) DNS and IP Address
> <!-- Multiline -->
> ![[Pasted image 20250201215542.png | center | 500]]
> 7. **~={green}DNS=~**: Translating domain names to IP addresses.
> 	* **~={red}Example=~**: website.com -> 192.168.1111
> 8. **~={green}IP Address=~**: Unique identifier for each device on a network. 
> 	* **~={red}Types=~**: IPv4 = 32 bit, IPv6 = 128 bit
> 	* ~={red}Public vs Private=~: Public are unique across the entire internet. Private is unique within a local network, and only accessible by devices within the same network.
>
> Once the IP address is obtained, HTTP requests are sent to our server, where the server can respond with the webpage (HTML) or JSON responses.

>[!Info]- (3, 4) Load Balancers
> <!-- Multiline -->
> A load balancer **~={purple}evenly distributes=~** incoming traffic among web servers that are defined in a load-balanced set.
> 
> ![[Pasted image 20250206204849.png | center | 200]]
> 
> * **~={green}How to Connect to Load Balancer=~**: Users connect to the public IP of the load balancer directly. With this setup, web servers are unreachable directly by clients anymore. 
> * **~={green}Security=~**: Private IPs are used for communication between servers. The load balancer communicates with web servers through private IPs.
> * **~={orange}Improves Availability=~**: 
> 	1. If a server goes down, the load balancer will reroute traffic to a different server.
> 	2. If all servers are not enough to handle the current traffic, another server can be added, and the load balancer can distribute the traffic among the servers.

>[!Info]- (5.1) Data Centres
> <!-- Multiline -->
> ![[Pasted image 20250201221002.png | center | 650]]
> * **~={purple}Description=~**: We may have data centres across the globe. Users are geo routed to the closest data centre as dictated by the DNS, which is determined by the location of a user.
> * **~={red}Data Synchronisation=~**: In the event any data centres encounter outage, traffic can be directed to other data centres. However, users from different regions could use different local databases, thus a common strategy is to **~={green}replicate data across multiple data centres=~**.

>[!Note]- Vertical vs Horizontal Scaling
> <!-- Multiline -->
> **~={purple}Vertical Scaling=~**
> * Improving hardware of servers (CPU, GPU, RAM).
> ````col
>```col-md
>**~={green}Advantage=~**
>* **~={green}Simple=~**: If traffic is low, it is ideal as it simplistic to implement
>
>```
>```col-md
>**~={red}Disadvantage=~**
>* **~={red}Limit=~**: Has a hard limit, there is only so much CPU and RAM you can add to a single server. Also **~={red}expensive=~**.
>* **~={red}Lack of Redundancy=~**: Does not have redundancy, and thus if a single server fails, the entire applications falls with it 
>```
>````
> 
> **~={purple}Horizontal Scaling=~**
> * Adding more servers to distribute workload
> ````col
>```col-md
>**~={green}Advantage=~**
>* Redundancy & Fault Tolerant & Scalable
>```
>```col-md
>**~={red}Disadvantage=~**
>* ~={red}**Complexity**=~: Requires load balancing, distributed databases, and synchronisation between servers, and data consistency.
>
>```
>````

>[!Info]- (5.2) Servers and Database
> <!-- Multiline -->
> With a larger user base, to scale the business logic, and data logic independently, we break down our servers into:
> * **~={green}Application Tier (Server)=~**: These servers handle:
> 	* HTTP Requests
> 	* Processes Business Logic (User Authentication, Payment Processing, Algorithms)
> 	* Communicates with the **~={green}data servers=~** to fetch or update data
> * **~={green}Data Tier (Database)=~**: These servers, store data in a SQL or NoSQL, and will handle **~={blue}read=~** and **~={blue}write=~** operations requested by the **~={green}application servers=~**.
>
> This serparation means:
> * **~={red}If more users visit a site=~**: We can scale up application servers
> * **~={red}If queries increase=~**: Database can be scaled up

>[!Info]- (6) Cache
> <!-- Multiline -->
> * ~={green}Cache=~: A cache is a temporary storage area that stores the result of expensive responses or frequently accessed data in memory so that subsequent requests are served more quickly.
> 	* ~={orange}Purposes=~: Better system performance, reducing database workloads, and allowing us to scale the cache tier independently.
> 
> **~={blue}Caching Considerations=~**
> * **~={purple}When to use cache=~**: For data that is frequently read, but modified infrequently. 
> * **~={purple}Expiration Policy=~**: Data should be removed from cache. **~={red}Too quickly=~** is bad as we don't want too reload from the database too frequently. **~={red}Too much time=~**, and data becomes stale.
> * ~={purple}**Consistency**=~: Ensuring data is cache is consistent with data in database is challenging.
> * **~={purple}Mitigating Failures=~**: Should always have more than 1 cache server to prevent a single point of failure.
> * **~={purple}Eviction Policy=~**: Once the cache is full, any requests to add items to the cache might cause existing items to be removed. This is called cache eviction. **~={green}LRU is the most common=~**.
> 
> **~={red}Caching Strategies=~**
> * If Stale Data or Data Loss Ok = **~={blue}Better Latency=~**
> * Read and write strategies are typically paired together <3
> 
> 1. **~={red}Cache Aside=~**:
> * **~={blue}Good For=~**: Read Heavy + Stale Data Ok
> * **~={blue}What Happens on Cache Miss/Who Loads Cache=~**: Web Server fetches from DB and caches it
> 
> ![[Pasted image 20250207090032.png | center | 300]]
> 
> 2. **~={red}Read Through Caching=~**:
> * **~={blue}Good For=~**: Read Heavy + Stale Data not Ok
> * **~={blue}What Happens on Cache Miss=~**: Cache server fetches from DB
> 
> ![[Pasted image 20250207090016.png | center | 500]]
>
> 3. **~={red}Write Through Caching=~**:
> * **~={blue}Good For=~**: Write Heavy + Stale Date not Ok
> * **~={blue}What Happens on Cache Miss=~**: N/A Cache always is up to date. Though if never accessed priorly, the cache server fetches from the DB.
> 
> ![[Pasted image 20250207090352.png | center | 500]]
> 
> 4. **~={red}Write Back Caching=~**:
> * **~={blue}Good For=~**: Write Heavy + Stale Data Ok/Risk of Dataloss in the async phase
> * **~={blue}What Happens on Cache Miss=~**: N/A Cache is up to date. Though if never accessed priorly, the cache server fetches from the DB.
> 
> ![[Pasted image 20250207090603.png | center | 500]]
>

>[!Note]- Stateless vs Stateful Architecture
> <!-- Multiline -->
> This architecture defines how session data is stored. Whether it's stored on servers or a shared NoSQL database.
>  
> **~={blue}Stateful=~**
> ![[Pasted image 20250208082639.png | center | 550]]
> * **~={blue}How is Session Data Stored=~**: 
> 	* ~={green}Servers=~: Stores client data from one request to another
> * **~={red}Example=~**:
> 	* To authenticate User A, it's HTTP request must always be rerouted to Server 1, as the session data is only stored on Server 1. Adding and removing servers become more challenging.
> 
> **~={blue}Stateless=~**
> ![[Pasted image 20250208083227.png | center | 550]]
> * **~={blue}How is Session Data Stored=~**:
> 	* ~={green}Servers=~: Keeps no state information
> 	* ~={green}Shared Database=~: Stores session data
> * ~={red}Example=~:
> 	* HTTP requests can be sent to any web servers which can fetch state data from the shared database. This data is keep out of the web servers, making it more **~={green}scalable (horizontally)=~**.
> 

>[!Info]- (7.1) Server <-> Database
> <!-- Multiline -->
> Refer to (5.2)
> 

>[!Info]- (7.2) Servers <-> Shared NoSQL Database
> <!-- Multiline -->
> * If we have sharded SQL databases, the data is **split across multiple servers** based on a **sharding key**. However, some **~={green}data may be better suited for NoSQL=~** such as (like orders, bookings, logs, cache, and temporary sessions) which don't need strict relational integrity.
> * Thus we can store this data in a shared NoSQL database.
> 

>[!Info]- (7.3) Message Queue
> <!-- Multiline -->
> Is stored in **~={red}memory=~** on a **~={red}dedicated server=~**. It supports asynchronous communication, and serves as a buffer for them.
> * **~={green}Producer Web Servers=~**:  Create messages and publish them to the queue
> * **~={green}Consumer Web Servers=~**: Connect to the queue and consume the messages, They are effectively "subscribed" to the queue.
> 
> This decoupling allows for greater scaling, where: 
> * producers can post a message to the queue wen the consumer is unavailable to process it, and
> * consumers can read messages from the queue when the producer is unavailable
> 
> **~={red}Example=~**:
> ![[Pasted image 20250208085858.png | center | 500]]

>[!Info]- (8) Content Delivery Network (CDN)
> <!-- Multiline -->
> * **~={green}What they are=~**: Are geographically dispersed servers used to deliver static content, where they cache content like **~={red}images, videos, CSS, JS=~** etc. It's effectively a cache for images.
> * **~={green}What they do=~**: User's connect to the closest CDN for image content rather than servers. This is quicker.
> 
>  ![[Pasted image 20250208100610.png | center | 300]]
> * **~={green}How they work=~**:
> ![[Pasted image 20250208101218.png | center | 600]]
> 1. **~={purple}User Gets Image from UR=~**L: Retrieve the image via an HTTP request
> 2. ~={purple}If the CDN Doesn't Have it=~: It retrieves it from a database server
> 3. ~={purple}Original Source of Data Returns image to CDN=~: Once it retrieves it, it is given an Time-To-Live (TTL) which describes how long it should be cached for.
> 4. ~={purple}CDN Caches=~: CDN then caches the image based on the TTL
> 5. If another request for the same image comes in, so long as the image has not expired, they can retrieve from the CDN.
> 
> * **~={green}Considerations=~**:
> 	* **~={red}Cost=~**: Run by third parties. Should only cache frequently cached items to minimise cost.
> 	* **~={red}TTL=~**: Setting an appropriate TTL is hard (refer to (6) Cache block).
> 	* **~={red}Invalidating Files=~**: If we attach a version number to the URL for each file, should the URL change (i.e image.png?v=2 to image.png?v=3), we will now be forced to fetch from the database server instead, if only v2 is in the CDN
> 

>[!Info]- (9) Logging, Metrics, and Automation
> <!-- Multiline -->
> * **~={blue}Logging=~**: Monitoring error logs helps identify issues in the system. Logs can be tracked per server or aggregated for centralised access via a shared database.
> * **~={blue}Metrics=~**: Collecting metrics provides insights into system health and business performance.
>   - **~={green}Host-level=~**: CPU, Memory, Disk I/O.
>   - **~={green}Aggregated=~**: Database/cache performance.
>   - **~={green}Business=~**: Daily active users, retention, revenue.
> * **~={blue}Automation=~**: Automating CI/CD, testing, and deployment enhances productivity by detecting issues early and streamlining workflows.

#flashcards/sysdesign/concepts/buildingblocks