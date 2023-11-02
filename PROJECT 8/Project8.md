
# Automating Load Balancer Configuration with Shell Scripting

Load balancing automation involves using scripts or software tools to create, update, monitor, and delete load balancers along with their associated rules and policies. 

Automating the load balancer configuration can lead to a reduction in human intervention, help cut out configuration mistakes, and aid adjustment to changing traffic patterns and server conditions.

Some of the benefits of load balancing automation are:

- Improves server administration

- Saves time and resources

- Enhances reliability and security

- Improves scalability and flexibility

- Optimizes performance and availability

## Automating the Deployment and Configuration Apache Webservers

In the previous project (Project 7), we implemented two Apache backend servers with an Nginx load balancer distributing traffic between both servers. The whole process in Project 7 was done manually. However, for this project we'll be automating the process from start to finish by using a shell script to run our commands.

To automate the deployment and configuration of Apache webserver EC2 instances, we'll need to follow the steps below:

**Step 1**

Provision two EC2 instances running on the Ubuntu OS

Click on the 'Launch instances' tab on the AWS dashboard

![Alt text](Images/aws-provision.png)

Fill out the details of the servers, selecting the Ubuntu OS, and attaching (or creating) a key pair, then click on 'Launch instance' at the bottom of the page

![Alt text](Images/aws-provision1.png)

![Alt text](Images/aws-provision2.png)

Check that the instances are up and running

![Alt text](Images/aws-provision3.png)

