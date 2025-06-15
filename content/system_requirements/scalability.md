## Introduction
Scalability is the ability of a system to handle a growing amount of load gracefully. A scalable system can increase its capacity by adding resources without requiring a fundamental redesign.

![](imgs/scalability.excalidraw.png)

## **How it's Measured** 
By analyzing the system's performance metrics (latency, throughput) as the load (e.g., number of users, requests) increases.
### **Why it's Important**
A business needs its systems to grow as its user base grows. A system that isn't scalable will eventually fail or become unacceptably slow under increased demand.
### **Common Techniques**
- **Horizontal Scaling (Scaling Out):** Adding more machines to the pool of resources. This is the preferred method for modern web applications and relies on stateless architecture.
- **Vertical Scaling (Scaling Up):** Increasing the resources (CPU, RAM, SSD) of an existing machine. This has physical limits and is often more expensive.
- **Asynchronous Processing:** Using message queues to decouple components and process non-urgent tasks in the background, smoothing out load spikes.