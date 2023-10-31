
# Implementing Load Balancers with Nginx

Load balancing is an important technique used in computing to maximize resource utilization and guarantee that no single resource in a network is overwhelmed by traffic. In this project, we'll be looking at how we can achieve load balancing with Nginx, a software that can act as a load balancer, webserver, and reverse proxy.

## Introduction to Load Balancing and Nginx

Load balancing allows systems or server admins to distribute workloads across various computing resources such as servers, virtual machines, or containers to achieve improved performance, scalability, and availability. 

For instance, if we have a set of webservers serving a website or application, a load balancer needs to be deployed to share the traffic equally across the webservers. The load balancer sits in front of the webservers and receives all the traffic first before distributing the traffic across the set of webservers. This ensures that none of the webservers gets overworked and consequently improves system performance. 

The diagram below shows a load balancer setup:

![Alt text](Images/load-balancer.png)

### Load Balancing Algorithms

There are techniques used to distribute incoming network traffic or workload amongst multiple servers. These techniques are known as load balancer algorithms. Some of them are:

**1. Round Robin:** The round robin algorithm shares requests successively to each server in the pool. It is easy to deploy and ensures an equal distributiom of traffic. It's best when all servers have identical capabilities and resources.

**2. Weighted Round Robin:** This is similar to the round robin technique, but servers are assigned requests based on their capabilities. Servers with more capacities receive more requests. This is best when servers have different performance levels or abilities.  

**3. Least Connections:** This algorithm sends new requests to the server with the least number of active connections. It works best when servers have different workloads or capacities because it sends traffic to the server that's least busy.

**4. Weighted Least Connections:** This is similar to the least connections algorithm. However, servers are assigned requests based on their abilities. Servers with higher capacities receive more requests. This balances traffic based on server capabilities.

**5. IP Hash:** This algorithm employs a hash function based on a client's IP address to constantly map the client to a particular server. This ensures that the same client always connects to the same server, which can be useful for maintaining session data or active connections.

## Setting Up a Basic Load Balancer

To set up a basic load balancer, we'll need to follow the steps below:

**1. Provision EC2 Instances**

We need to create three (3) EC2 instances with the Ubuntu operating system. The first two EC2 instances will host the Apache webserver, while the third EC2 instance will host the Nginx load balancer.

To provision the Apache EC2 instances, I'll follow these steps:

**Step 1**

To create EC2 instances on AWS, login into AWS and then click on `Launch instance` on the EC2 Dashboard

![Alt text](Images/aws_launch-instance.png)

**Step 2**

On the 'Launch an instance' page, fill in the details of the EC2 instance. It's possible to create two instances at once by specifying the number of instances as '2'

![Alt text](Images/aws_launch-instance2.png)

**Step 3**

Check that the instances are up and running

![Alt text](Images/aws_launch-instance3.png)

**Step 4**

Rename the instances to match the specific names we want to give them

![Alt text](Images/aws_launch-instance4.png)

To provision the Nginx EC2 instances, I'll follow these steps:

**Step 1**

To create the Nginx EC2 instance on AWS, click on `Launch instance` on the EC2 Dashboard

![Alt text](Images/aws_launch-instance.png)

**Step 2**

On the 'Launch an instance' page, fill in the details of the EC2 instance

![Alt text](Images/aws_launch-instance5.png)

**Step 3**

Check that the instances are up and running

![Alt text](Images/aws_launch-instance6.png)

**2. Edit the Security Groups to Open `Port 8000` on the Apache EC2 Instances and `Port 80` on the Nginx EC2 Instance**

To edit inbound rules on the Apache EC2 instances to open `Port 8080`:

**Step 1**

Navigate to the 'Security' tab on the Apache EC2 instances

![Alt text](Images/aws_ec2_8000.png)

**Step 2**

Click on the Security group name (launch-wizard-5) to open the security rules page 

![Alt text](Images/aws_ec2_8000_1.png)

![Alt text](Images/aws_ec2_8000_2.png)

**Step 3**

Click on 'Edit inbound rules' to add the desired rule. Add the desired rule and click on 'Save rules'

![Alt text](Images/aws_ec2_8000_3.png)

![Alt text](Images/aws_ec2_8000_4.png)

![Alt text](Images/aws_ec2_8000_5.png)

To edit inbound rules on the Nginx EC2 instance to open `Port 80`, follow the above Steps 1 to 3.

![Alt text](Images/aws_ec2_8000_6.png)





