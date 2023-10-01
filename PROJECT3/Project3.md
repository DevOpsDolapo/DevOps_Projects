
# Documentation for the LAMP Stack Implementation Project

There are different technology stacks used in the development of various solutions such as software, applications, websites, etc. These technology stacks are known as Web Stacks and they include the LAMP, LEMP, MERN, and MEAN stacks. As a DevOps Engineer, it is important to have a good knowledge and understanding of how to implement these technology stacks.

What is a Technology Stack?

A technology stack is a set of tools and frameworks used to build or develop software products. They are specifically used together in the creation of functional software or applications. These technology stacks are named using the acronyms of the individual technologies involved in developing a particular product.

Examples of technology stacks are:

- LAMP (Linux, Apache, MySQL, PHP (or Python or Perl))

- LEMP (Linux, NginX, MySQL, PHP (or Python or Perl))

- MERN (MongoDB, ExpressJS, ReactJS, NodeJS)

- MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

## Introduction to LAMP Stack Architecture

The LAMP stack consists of four (4) different software technologies that developers use to build web applications and websites. The LAMP stack is quite popular among developers because all four technologies are open source; they are maintained by a community and freely available for everyone to use. They are free of cost, easy to use, easy to maintain, and flexible.

The LAMP architecture is great for creating, hosting, and maintaining web content.

The acronym LAMP can be broken down as follows:

- L (Linux operating system)

- A (Apache web server)

- M (MySQL database server)

- P (PHP programming language)

## Spinning up and EC2 instance and Creating an Ubuntu Server OS

Amazon Web Services (AWS) is a cloud services provider that facilitates on-demand delivery of IT resources over the internet through a pay-as-you-go pricing template.

For this project, I'll be using free-tier servers on the AWS platform known as EC2 (Elastic Compute Cloud) running on the Ubuntu Server OS.

### Spinning up an EC2 instance and creating an Ubuntu Server OS

To spin up the required EC2 instances for my project, I'll need to log into my AWS account

![Alt text](Images/aws_login.JPG)

To create the EC2 instances, I follow these steps;

1. Clicked on 'Launch a Virtual Machine'

![Alt text](Images/launch_instance.JPG)

2. On the 'Launch an instance' page, fill in the details for my servers and the number of instances I need, then select Ubuntu as my preferred OS.

![Alt text](Images/select_instance_details.JPG)

3. Select a 'free-tier eligible' AMI (Amazon Machine Image) 

![Alt text](Images/free_tier_eligible.JPG)

4. Create a key pair for secure connection to my EC2 instances or select an existing (saved) key pair

![Alt text](Images/key_pair_creation_selection.JPG)

5. Click on 'Launch instance' at the bottom of the page to launch the EC2 instances

![Alt text](Images/launch_instance_click.JPG)

6. The two EC2 instances have been created and they are both running on the Ubuntu Server OS

![Alt text](Images/EC2_instances.JPG)

7. The servers can be renamed to indicate their individual identities

![Alt text](Images/servers_instances.JPG)

### Connecting to the EC2 Instances

To connect to the servers I just created on AWS, I'll be using a software known as Termius

1. Click on 'NEW HOST' on the termius software

![Alt text](Images/termius_newhost.JPG)

2. Copy the Public IPV4 address from the Apache server instance on AWS

![Alt text](Images/apache_ip.JPG)

3. Paste the IP address in the New Host configuration pane in Termius

![Alt text](Images/termius_ip.JPG)







