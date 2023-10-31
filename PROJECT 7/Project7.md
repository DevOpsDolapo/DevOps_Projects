
# Implementing Load Balancers with Nginx

Load balancing is an important technique used in computing to maximize resource utilization and guarantee that no single resource in a network is overwhelmed by traffic. In this project, we'll be looking at how we can achieve load balancing with Nginx, a software that can act as a load balancer, webserver, and reverse proxy.

## Introduction to Load Balancing and Nginx

Load balancing allows systems or server admins to distribute workloads across various computing resources such as servers, virtual machines, or containers to achieve improved performance, scalability, and availability. 

For instance, if we have a set of webservers serving a website or application, a load balancer needs to be deployed to share the traffic equally across the webservers. The load balancer sits in front of the webservers and receives all the traffic first before distributing the traffic across the set of webservers. This ensures that none of the webservers gets overworked and consequently improves system performance. 

The diagram below shows a load balancer setup:

![Alt text](Images/load-balancer.png)

- **Load Balancing Algorithms**

There are techniques used to distribute incoming network traffic or workload amongst multiple servers. These techniques are known as load balancer algorithms. Some of them are:

**1. Round Robin:** The round robin algorithm shares requests successively to each server in the pool. It is easy to deploy and ensures an equal distributiom of traffic. It's best when all servers have identical capabilities and resources.

**2. Weighted Round Robin:** This is similar to the round robin technique, but servers are assigned requests based on their capabilities. Servers with more capacities receive more requests. This is best when servers have different performance levels or abilities.  

**3. Least Connections:** This algorithm sends new requests to the server with the least number of active connections. It works best when servers have different workloads or capacities because it sends traffic to the server that's least busy.

**4. Weighted Least Connections:** This is similar to the least connections algorithm. However, servers are assigned requests based on their abilities. Servers with higher capacities receive more requests. This balances traffic based on server capabilities.

**5. IP Hash:** This algorithm employs a hash function based on a client's IP address to constantly map the client to a particular server. This ensures that the same client always connects to the same server, which can be useful for maintaining session data or active connections.




