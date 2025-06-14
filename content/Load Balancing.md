## Introduction
Load balancers are essential for distributing incoming traffic across a group of servers. Their primary purpose is to enhance a system's availability and reliability by preventing any single server from becoming overwhelmed, which in turn avoids performance issues and downtime.

![](load_balancer.excalidraw.png)

To achieve maximum scalability and redundancy, you can implement load balancing at multiple layers of your system:
- **User to Web Server:** A load balancer receives incoming user requests and distributes them among a fleet of web servers. This is the most common and public-facing use.
- **Web Server to Application/Cache Server:** After the initial request is handled, web servers may need to communicate with internal services. A load balancer can manage this traffic between the web servers and a layer of application servers or caching servers.
- **Application Server to Database:** Application servers often need to read or write data. A load balancer can be placed between the application servers and the database servers to manage these connections efficiently.
---
## Load Balancing algorithms
### Round Robin
Distributes requests to servers in a simple, cyclical order.
- **Pros**:
	- Ensures equal distribution of requests.
    - Simple to implement.
    - Effective when servers have similar capacities.
- **Cons**:
    - **No Load Awareness**: Ignores the current load or capacity of each server.
    - **No Session Affinity**: Can disrupt stateful applications as subsequent requests from the same client may go to different servers.
    - **Predictable Pattern**: The distribution pattern can be predictable, potentially exposing vulnerabilities.
- **Use Cases**:
    - **Homogeneous Environments**: Best for server pools where all nodes have similar performance.
    - **Stateless Applications**: Ideal for applications where each request is independent.
### Least Connections
Directs new requests to the server with the fewest active connections.
- **Pros**:
    - **Load Awareness**: Adapts to the current load, leading to better resource utilization.
    - **Dynamic Distribution**: Handles unpredictable traffic patterns well.
    - **Efficient in Heterogeneous Environments**: Works well even if servers have different capacities.
- **Cons**:
    - **Higher Complexity**: Requires real-time monitoring of active connections.
    - **State Maintenance**: Adds overhead by needing to track connection states.
- **Use Cases**:
    - **Heterogeneous Environments**: Suitable for server pools with varying capacities.
    - **Variable Traffic Patterns**: Effective for applications with unpredictable traffic.
    - **Stateful Applications**: Helps distribute active sessions more evenly.
### Weighted Round Robin (WRR)
Assigns a "weight" to each server based on its capacity and distributes requests proportionally. More powerful servers get more requests.
- **Pros**:
    - **Capacity-Based Distribution**: Utilizes powerful servers more effectively.
    - **Flexibility**: Weights can be adjusted as server capacities change.
- **Cons**:
    - **Complex Weight Assignment**: Determining the correct weights can be challenging.
    - **No Real-time Load Awareness**: Doesn't account for transient server load, only pre-assigned capacity.
- **Use Cases**:
    - **Heterogeneous Server Environments**: Ideal when servers have different processing capabilities.
    - **Scalable Web Applications** and **Database Clusters**.
### Weighted Least Connections
Combines the principles of **Weighted Round Robin** and **Least Connections**. It considers both the server's pre-defined weight and the current number of active connections.
- **Pros**:
    - **Dynamic and Capacity-Aware**: Balances traffic based on both real-time load and server capacity.
    - **Flexibility**: Handles heterogeneous servers and variable loads effectively.
- **Cons**:
    - **High Complexity**: The most complex of the common algorithms to implement.
    - **State Maintenance Overhead**: Must track both active connections and server weights.
- **Use Cases**:
    - **Heterogeneous Server Environments** with high and variable traffic.
    - **High Traffic Web Applications** and **Database Clusters**.
### IP Hash
Uses a hash of the client's IP address to determine which server receives the request. This ensures that a user is consistently sent to the same server.
- **Pros**:
    - **Session Persistence**: Guarantees that requests from the same client land on the same server, which is crucial for stateful applications.
    - **Simplicity**: Does not require the load balancer to maintain connection state.
- **Cons**:
    - **Uneven Distribution**: Can lead to an unbalanced load if client IPs are not evenly distributed (e.g., many users behind a single corporate NAT).
    - **Disruption on Change**: Adding or removing servers changes the hash mapping, disrupting existing sessions.
- **Use Cases**:
    - **Stateful Applications**: Perfect for services requiring session persistence, like e-commerce shopping carts.
    - **Geographically Distributed Clients**.
### Least Response Time
A dynamic algorithm that sends requests to the server with the lowest average response time.
- **Pros**:
    - **Optimized Performance**: Routes traffic to the fastest responding server, improving client experience.
    - **Dynamic Load Balancing**: Adjusts in real-time to server performance.
- **Cons**:
    - **Complexity**: Requires continuous monitoring of server response times.
    - **Overhead**: Monitoring can add computational overhead.
- **Use Cases**:
    - **Real-Time Applications**: Ideal for services where low latency is critical, such as gaming, streaming, or financial trading.
    - **Web Services & APIs**.
### Least Bandwidth
Distributes requests to the server currently consuming the least amount of bandwidth.
- **Pros**:
    - **Dynamic Network Load Balancing**: Balances traffic efficiently based on data throughput.
    - **Prevents Overloading**: Avoids overwhelming any single server with excessive data traffic.
- **Cons**:
    - **Complexity**: Requires continuous monitoring of bandwidth usage.
    - **Overhead**: Monitoring can add to system overhead.
- **Use Cases**:
    - **High Bandwidth Applications**: Ideal for video streaming, file downloads, and large data transfers.
    - **Content Delivery Networks (CDNs)**.
---
## Types of Load Balancers
### Hardware Load Balancing
These are physical, dedicated devices (appliances) that use specialized hardware like ASICs to distribute traffic.
- **Pros**:
    - **High performance** and throughput.
    - Often includes built-in security and management features.
    - Can handle very large traffic volumes.
- **Cons**:
	- Can be **expensive**.
    - Requires specialized knowledge for configuration.
    - **Limited scalability**; requires purchasing more hardware to add capacity.
- **Example**: A large e-commerce company uses a hardware load balancer to ensure fast response times for its many customers.
### Software Load Balancing
These are applications that run on general-purpose servers or virtual machines to distribute traffic using software algorithms.
- **Pros**:
    - More **affordable** than hardware solutions.
    - **Easily scalable** by adding more server resources.
    - **Flexible** and can be deployed in various environments, including the cloud.
- **Cons**:
    - May have **lower performance** under heavy loads compared to hardware.
    - Consumes resources on the host system.
    - Requires ongoing software maintenance.
- **Example**: A startup uses a software load balancer on a cloud VM to handle its growing user base.
### DNS Load Balancing
This method uses the Domain Name System (DNS) to distribute traffic by resolving a single domain name to multiple IP addresses.
- **Pros**:
    - Relatively **simple to implement**.
    - Provides basic load balancing and failover.
    - Good for directing traffic across **geographically distributed servers**.
- **Cons**:
    - **Slow to update** due to DNS propagation and caching.
    - **No awareness of server health** or current load.
    - Not suitable for applications needing session persistence.
- **Example**: A Content Delivery Network (CDN) uses DNS to send users to the closest edge server based on their location.
### Global Server Load Balancing (GSLB)
An advanced form of load balancing that distributes traffic across multiple, geographically dispersed data centers. It often combines DNS with health checks.

- **Pros**:
    - 🌍 Provides **failover and load balancing across data centers**.
    - Improves performance by directing users to the nearest or best-performing data center.
    - Supports advanced features like health checks and custom routing.
- **Cons**:
    - **Complex** to set up and manage.
    - Can be costly, requiring specialized hardware or software.
    - Subject to the same DNS limitations (e.g., slow updates).
- **Example**: A multinational corporation uses GSLB to ensure high availability for its global web applications.

---

### 6. Hybrid Load Balancing

A strategy that combines multiple load balancing techniques (e.g., hardware, software, and cloud) to optimize performance and reliability.

- **Pros**:
    - Extremely **flexible** and can be tailored to specific needs.
    - Leverages the strengths of different techniques for the best overall result.
    - Allows an organization's strategy to evolve over time.
- **Cons**:
    - Can be **highly complex** to set up and manage.
    - Requires a high level of expertise.
    - Potentially higher costs due to combining multiple services.
- **Example**: A large streaming platform uses hardware balancers in its data centers, cloud balancers for content delivery, and DNS for global traffic management.