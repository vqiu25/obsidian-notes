# Alex Xu

## Understanding the Problem and Establish Design Scope

### Functional Requirements

1. **~={purple}What securities are being traded=~**:
	1. **~={green}Stocks=~**: We only need to support stocks
	2. **~={green}Options=~**: You pay a one-time fee to be able (optional) to buy or sell a stock at a certain price in the future
		1. Strike Price
		2. Expiry Date
	3. **~={green}Futures=~**: You pay a one-time fee to be able (not optional) to buy or sell a stock at a certain price in the future
		1. Strike Price
		2. Delivery Date
2. **~={purple}What order operations are supported=~**: (Only limit orders)
	1. Placing a new order
	2. Cancelling an order
3. **~={purple}Scale of the exchange, how many symbols and how many orders=~**:
	1. At least 100 symbols
	2. 1 billion orders per day

### Non-functional Requirements

1. **~={green}Availability=~**: High availability 99.99%. Downtime is not good.
	* $(1-0.9999)*365*24*60 = 52$ minutes of downtime
2. **~={red}Fault Tolerance=~**: If the orderbook goes down, there should be backups that can take its place
3. **~={blue}Latency=~**: At least the 99th percentile. Which is to say:
	1. 99% of requests are faster than $x$ ms
	2. The remaining 1% is slower than $y$ ms
4. **~={purple}Security=~**: Perform KYC checks 

### Back of the envelope estimation

* 100 tickers
* 1 billion orders per day
* The exchange is open for 6.5 hours per day
* **~={purple}QPS (Queries per second)=~**: 1 billion / (6.5 * 60 * 60) = 43,000
* Peak QPS is 5x the amount

## High Level Design

### System Diagram

![[Drawing 2025-04-12 12.31.41.excalidraw | center | 700]]

### Data Model


# Youtube Video

## Requirements

1. Create an exchange to match buyers and sellers 
2. Provide an ordered list of operations that everybody can agree on (eventually)!
3. Provide an ordered list of all activities pertaining to each client (Placed Orders, Cancelled Orders, Trades) back to each client

## The Limit Order Book (Matching Engine)

Exchanges keep track of bids and asks:

* **~={green}Bids (Buy Order)=~**: Find the minimum price someone is willing to sell at
	* ✅: If the buy price >= min sell price
	* ❌: If the buy price < min sell price
* **~={red}Asks (Sell Order)=~**: Find the maximum price someone is willing to buy at
	* ✅: If the sell price <= max buy price
	* ❌: If the sell (ask) price > max buy (bid) price

A trade will go through if there exists both a bid and ask where the `bid price >= ask price`. 

# Matching Engine Performance

1. The matching engine needs to be **~={green}fast=~**!
	* Needs to handle all incoming orders and cancellations
	* We want to limit the number of I/O's (network calls and calls to disk). Ideally everything locally **~={blue}runs in memory=~**.
2. The matching engine needs to be **~={green}deterministic=~**.
	* If the order book currently has state $S$, applying an order $m$ to it will always produce the same state $S'$.

# Matching Engine Networking

The matching engine has many interested parties in what it is publishing. Everyone should receive market data at the same time:

1. **~={blue}Clients (Robinhood)=~** that are sending in orders
2. **~={blue}Banks/clearing=~** companies
3. Other **~={blue}market data vendors=~**

To make it fair:

* ❌ **~={purple}TCP=~**: Requires a bunch of one to one connections from the exchange (multiple threads for each connection), but it's not guaranteed that everyone would receive the same message at the same time.
* ✅ **~={purple}UDP=~**: We can therefore use ~={green}**UDP multicas**t=~ instead. This allows us to send the same packet to multiple users at the same time.
	* This means we lose congestion control, flow control, reliable broadcast, and ordered broadcast
	* To solve the non-ordered-ness, we use re-transmitters. The idea here is that we use UDP multicast and broadcast to all the clients and $R$ number of transmitters. Should a client not receive an order/message, it will ping the closest re-transmitter, check if it has it, and get it to resend it.
		* Let $p =$ probability of a re-transmitter caching the desired order
		* Probability a re-transmitter does not have the missing the order $P=(1 - p)$
		* Probability $R$ number of re-transmitter does not have the missing the order $P=(1 - p)^R$
		* Probability at least 1 re-transmitter (not all failures = at least 1 success) has the missing order $P=1-(1-p)^R$

![[Drawing 2025-04-08 23.07.37.excalidraw | center]]
# Matching Engine Fault Tolerance

The matching engine runs completely in memory, and could go down. Hence we need a **~={blue}backup=~**!

**~={purple}How to Detect a Failure=~**:

![[Drawing 2025-04-09 10.09.39.excalidraw | center]]

**~={red}Option 1=~** ❌: **~={purple}Recreate State by Sending Orders to Primary and Backup=~**

![[Drawing 2025-04-09 10.56.45.excalidraw | center]]

**~={green}Option 2=~** ✅: State Machine Replication

When the **~={green}primary=~** orderbook receives an order, before even processing it/updating it's orderbook, it will send these orders to the **~={red}backup=~** orderbooks

![[Drawing 2025-04-09 11.12.33.excalidraw | center]]

Each order has to be processed atomically. Should the primary order book finish processing `Order 1`, and crashes, the backup orderbook will contain `Order 2`, thus it can continue if there are still orders in the queue.

If each order is not a single packet, we need sequence numbers and checksum for the multiple packets representing a single order. This makes **~={green}each order atomic=~**.

# Matching Engine Partitioning

Since we're relying on a single primary matching engine for message ordering, the only way to improve our throughput is to partition it:

* ❌ **~={purple}Partition within the same symbol=~**:
	* Either partitions aren't aware of other partitions, making them inconsistent
	* Or they are consistent, but to do so will require network calls which will be slow
* ✅ **~={purple}Partition based on stock symbols=~**:
	* We will have a matching engine/order book for every stock.
![[Drawing 2025-04-09 16.32.30.excalidraw | center | 300]]
A concern to consider is that if we wanted to cancel 2 orders at the same time atomically under this partitioning, this is not supported. To do so, we would need to perform 2 phase locking, where if one message doesn't come through, none of them do. However this worsens latency, so we don't allow this.

**~={red}Example=~**:

> [!QUOTE] 2PL
> 
**~={purple}Phase 1 (Growing phase)=~**:
Acquire all the locks you need before doing anything.
>
**~={purple}Phase 2 (Shrinking phase)=~**:
Once you start releasing any lock, you can’t acquire new ones.

**~={purple}Without 2PL, here’s what might go wrong=~**:
1. You successfully cancel the AAPL order.
2. Before you cancel the GOOGL order, a network glitch happens or someone else modifies it.
3. Now you’ve canceled only one of the two — not atomic, not safe.

**~={purple}Using 2PL=~**:
1. You’d acquire locks on both AAPL and GOOGL order books first.
2. Only after both locks are acquired, you proceed to cancel both orders.
3. If one lock fails (e.g., timeout or network failure), you release both and rollback — nothing is canceled.

# Returning Ordered Messages For Clients

If we are using multiple threads, we need to ensure that each order/message is performed in the order that clients placed their order in.

**~={red}Example=~**:

We have **2 order books**, each potentially running on their **own thread or system**:

**🟪 ~={purple}Orderbook 1 (e.g., for AAPL)=~:**
* `unordered_map<pair<userId, stockId>, Order>` → Tracks the last active order per user/stock.
* `map<double, deque<Order>>` → Price level → queue of orders (FIFO at each price).

**🟪 ~={purple}Orderbook 2 (e.g., for TSLA)=~:**
* Same structure as above.

Each Order also contains a **sequence number** (like a timestamp) that reflects the **client’s global message order**.

---

**🧠 ~={purple}Why Do We Need Per-Client Sequence Numbers?**=~

A single client (e.g., user 42) may place orders across multiple stocks:

1. **~={purple}Order A=~**: cancel AAPL
2. **~={purple}Order B=~**: place order on TSLA

Since AAPL and TSLA are handled by different order books (and threads), they may process messages **out of order**.

> ✅ To preserve **global order per client**, we assign a **monotonic sequence number** to every message.

---

**🏗️ ~={purple}StockExchange Class**=~

```cpp
class StockExchange {
    map<StockId, OrderBook> stockOrderBooks;       // stock → order book
    map<UserId, mutex> clientLocks;                // one mutex per client
    map<UserId, long long> clientSequenceNumbers;  // seq num per client
};
```

---

**🔄 ~={purple}Order Handling Logic**=~

When an order message comes in (e.g., cancel, place order):

1. **Acquire the lock for the client**:

```cpp
lock_guard<mutex> guard(clientLocks[userId]);
```

2. **Assign the next sequence number**:

```cpp
long long seqNo = clientSequenceNumbers[userId]++;
```

3. **Attach the sequence number to the Order object**

4. **Dispatch the order to the appropriate order book**:

```cpp
stockOrderBooks[stockId].handleOrder(Order(..., seqNo));
```

# System Diagram

![[Drawing 2025-04-09 23.25.27.excalidraw | center | 500]]