
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

### Provision two EC2 instances running on the Ubuntu OS**

**Step 1: Click on the 'Launch instances' tab on the AWS dashboard**

![Alt text](Images/aws-provision.png)

**Step 2: Fill out the details of the servers, selecting the Ubuntu OS, and attaching (or creating) a key pair, then click on 'Launch instance' at the bottom of the page**

![Alt text](Images/aws-provision1.png)

![Alt text](Images/aws-provision2.png)

Check that the instances are up and running

![Alt text](Images/aws-provision3.png)

### Open Port 8000 to Allow Traffic from Anywhere

**Step 1: Edit the 'inbound rules' under the Security Group tab of the EC2 instances**

![Alt text](Images/aws-provision4.png)

Click on 'Add rule' to add the necessary rule details and click on 'Save rules' 

![Alt text](Images/aws-provision5.png)

![Alt text](Images/aws-provision6.png)

### Connect to the Webservers

**Step 1: We'll connect to the webservers through the Termius software**

![Alt text](Images/termius-apache1.png)

![Alt text](Images/termius-apache2.png)

### Deploy and Configure the Webservers Using a Script

We'll use the shell script below to deploy and configure the Apache webservers

```
#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2
```
**Step 1: The above shell script will be added to a file named `install.sh` on both `Apache-server1` and `Apache-server2` with the some modifications made to the script to reflect our webservers using the command `sudo nano install.sh`**

![Alt text](Images/script-apache.png)

![Alt text](Images/script-apache1.png)

![Alt text](Images/script-apache2.png)

**Step 2: Make the shell script executable on the two webservers by using the command `sudo chmod +x install.sh`**

![Alt text](Images/script-apache11.png)

![Alt text](Images/script-apache22.png)





