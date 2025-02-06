![[sys_design_initial.png]]
>[!Info]- (0) Overall System
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (1, 2) DNS and IP Address
> <!-- Multiline -->
> ![[Pasted image 20250201215542.png | center | 500]]
> 1. **~={green}DNS=~**: Translating domain names to IP addresses.
> 	* **~={red}Example=~**: website.com -> 192.168.1111
> 2. **~={green}IP Address=~**: Unique identifier for each device on a network. 
> 	* **~={red}Types=~**: IPv4 = 32 bit, IPv6 = 128 bit
> 	* ~={red}Public vs Private=~: Public are unique across the entire internet. Private is unique within a local network, and only accessible by devices within the same network.
>
> Once the IP address is obtained, HTTP requests are sent to our server, where the server can respond with the webpage (HTML) or JSON responses.

>[!Info]- (3, 4) Load Balancers
> <!-- Multiline -->
> A load balancer **~={purple}evenly distributes=~** incoming traffic among web servers that are defined in a load-balanced set.
> 
> {Add Image}
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

>[!Info]- (5.2) Database
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
>* **~={red}Limit=~**: Has a hard limit, there is only so much CPU and RAM you can add to a single server
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

>[!Info]- (5.3) Database Vertical vs Horizontal Scaling
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (6) Cache
> <!-- Multiline -->
> {Add Image}
> 

>[!Note]- Stateless vs Stateful Architecture
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (7.1) Server <-> Database
> <!-- Multiline -->
> {Add Image}
> 

>[!Note]- (7.2) Database Replication
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (7.3) Servers <-> Shared Database
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (7.3) Message Queue
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (8) Content Delivery Network (CDN)
> <!-- Multiline -->
> {Add Image}
> 

>[!Info]- (9) Logging, Metrics, and Automation
> <!-- Multiline -->
> {Add Image}
> 

#flashcards/sysdesign/concepts/buildingblocks