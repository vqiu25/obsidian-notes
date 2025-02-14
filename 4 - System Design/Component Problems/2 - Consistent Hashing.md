> [!quote]+ Overview
>````col 
>```col-md 
> **~={blue}Description=~**:
> * Consistent hashing is a **~={red}distributed hashing technique=~** that dynamically distributes requests across a set of servers. Unlike traditional hashing, which can cause major disruptions when nodes are added or removed, consistent hashing minimises key reassignment by only affecting a small portion of the data.
>``` 
>```col-md 
>**~={green}Benefits=~**:
>* **~={green}Scalability=~**: Easily add or remove nodes without significant rebalancing. 
>* **~={green}Fault Tolerance=~**: If a node fails, only a subset of keys is remapped to neighbouring nodes. 
>* **~={green}Efficient Load Distribution=~**: Helps distribute requests evenly, preventing hotspots.
>``` 
>```` 
# Rehashing Problem

>[!Note]- Rehashing Problem
> <!-- Multiline -->
>**~={purple}How is Data Distributed Across Servers?=~**
>If we have $n=4$ servers, then $serverIndex = hash(key) \% n$. We use this equation fetch the cached data from the serverIndex that is returned.
>
> ![[Drawing 2025-02-12 17.27.54.excalidraw | center | 550]]
>**~={purple}What happens when Servers are Added or Removed?=~**
>However, if a server is added or removed, let say removed, $n=3$. Then we need to rehash every key with $\%3$. This will redistribute the data for every key, making them swap servers, rather than only impacting the data that was stored on the removed server.

# What is Consistent Hashing

>[!Note]- Consisting Hashing
> <!-- Multiline -->
>**~={purple}What is Consistent Hashing?=~**
>A kind of hashing where on average, if a hash table is resized, only $k/n$ keys need to be remapped on average, compared to every key being remapped.
>
>**~={purple}Hash Space, Hash Ring, Hashing Servers and Keys=~**
>* **~={green}Hash Mapping=~**: We map every server and key to a hash ring
>* **~={green}Which Keys are Stored on Which Servers=~**: Look at the keys, and go clockwise. Whichever server it reaches first, is the server that key is stored in.
>
> ![[Drawing 2025-02-13 08.02.38.excalidraw | center | 400]]
> 
>````col
>```col-md
>**~={green}Adding Server=~**
>* Before Key 0 was stored on Server 0
>* Adding Server 4, Key 0 is remapped to Server 4
>
> ![[Drawing 2025-02-13 08.18.34.excalidraw | center | 210]]
>
>```
>```col-md
>**~={red}Removing Server=~**
>* Before Key 2 was stored on Server 2
>* Removing Server 2, Key 2 is remapped to Server 3
>
> ![[Drawing 2025-02-13 08.23.58.excalidraw | center | 350]]
>
>```
>````
>
>**~={purple}Solving the Partition Issue Using Virtual Nodes=~**
>The partition issue is when adding or removing nodes causing the space between servers to be not uniform. This means:
>1. The gaps between servers are not uniform
>2. Keys are not uniformly distributed to servers
>
>To solve this, we use something called **~={green}virtual nodes=~**. For every server we have, we will "duplicate it x" number of times. So we will place the server nodes $x$ times on the ring. I.e. if we were mapping the server with IP Address 192.168.14, the virtual nodes could look like:
>* 192.168.14#1 -> Hash Function -> Placed on Ring
>* 192.168.14#2 -> Hash Function -> Placed on Ring
>* ...
>
>By simply **~={green}having more nodes=~** on the ring, the distribution of keys become more **~={blue}balanced=~**.
