
# Documentation for the LEMP Stack Implementation Project

The LEMP Stack is one of the most popular Technology Stacks used in the development of software, applications, and websites. A great working knowledge of this Web Stack is an essential addition to a DevOps Engineer's skill set.

The term Technology Stack was defined in the previous project, but here is a quick recap:

What is a Technology Stack?

A technology stack is a set of tools and frameworks used to build or develop software products. They are specifically used together in the creation of functional software or applications. These technology stacks are named using the acronyms of the individual technologies involved in developing a particular product.

Examples of technology stacks are:

- LEMP (Linux, NginX, MySQL, PHP (or Python or Perl))

- LAMP (Linux, Apache, MySQL, PHP (or Python or Perl))

- MERN (MongoDB, ExpressJS, ReactJS, NodeJS)

- MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

## Introduction to LEMP Stack Architecture

The LEMP stack is made up of Linux OS, Nginx Server, MySQL Database, and PHP. Its components are also all open-source just like the LAMP stack and is used to create dynamic web applications.

The acronym LEMP can be broken down as follows:

- L (Linux operating system)
Linux is an open-source operating system. In this case the webserver runs on the Linux operating system.

- E (Nginx web server)
This is pronounced as “Engine X”. It is a very popular web server. While it is open source just like Apache, it is faster in certain situations and comes with better security.

- M (MySQL database server)
A Database Management System (DBMS) that allows us to store and manage data. 

- P (PHP)
PHP is a server-side scripting language that communicates with the database and server. 

### How the LEMP Stack Works

**Nginx**: Works similarly to Apache in the LAMP stack. It processes user requests and responds with suitable output.

**PHP**: Processes requests, communicates with the database, carries out user authentication, etc.

**MySQL**: Handles database requests.

**Linux**: The Operating System that runs at the base of this stack.

## Spinning up an EC2 instance and Creating an Ubuntu Linux Server OS

Amazon Web Services (AWS) is a cloud services provider that facilitates on-demand delivery of IT resources over the internet through a pay-as-you-go pricing template.

For this project, I'll be using free-tier servers on the AWS platform known as EC2 (Elastic Compute Cloud) running on the Ubuntu Server OS.

To spin up the required EC2 instances for my project, I'll need to log into my AWS account

![Alt text](Images/aws_login.JPG)

To create the EC2 instances, follow these steps;

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

## Connecting to the Nginx EC2 Instance

To connect to the Nginx server I just created on AWS, I'll be using a software known as Termius.

To connect to the Nginx server, follow the steps 1 to 6 below:

1. Click on 'NEW HOST' on the termius software

![Alt text](Images/termius_newhost.JPG)

2. Copy the Public IPV4 address from the Nginx server instance on AWS

![Alt text](Images/nginx_ip.jpg)

3. Paste the IP address in the New Host configuration pane in Termius

![Alt text](Images/termius_ip.JPG)

4. Set the Username as 'ubuntu', and import the key file saved earlier from AWS. Note that SSH uses TCP port 22 for connections and this is usually open by default on our EC2 instance.

![Alt text](Images/ubuntu_set_name.JPG)

![Alt text](Images/import_key_file.JPG)

5. Save the imported key file

![Alt text](Images/save_key_file.JPG)
 
 6. Launch the server by double-clicking on the host created in Termius

 ![Alt text](Images/termius_launch_host.JPG)

 Successful connection to the Nginx EC2 instance:

  ![Alt text](Images/nginx_ec2_successful_connection.jpg)

## Installing the Nginx Web Server and Updating the Firewall

To install Nginx on the Ubuntu server, we need to follow these steps:

1. Update the list of packages using the Ubuntu package manager by running the `sudo apt update` package

![Alt text](Images/nginx_update_packages.jpg)

![Alt text](Images/nginx_update_packages2.jpg)

2. Install Nginx by running the command `sudo apt install nginx`

![Alt text](Images/nginx_installation.jpg)

![Alt text](Images/nginx_installation2.jpg)

3. Verify that Nginx is running by using the command `sudo systemctl status nginx`

![Alt text](Images/nginx_status.jpg)

The result shows that the service is enabled and active (running)

### Updating the Firewall rules

For the Nginx web server to accept inbound traffic, we need to open up TCP port 80 on the Nginx EC2 instance. Web browsers use port 80 to access web pages on the internet. 

However, because we created two EC2 instances at the same time and we already opened up TCP port 80 on the Apache web server, the Nginx web server shares the same Firewall rule. 

The steps to open up TCP port 80 on the Apache EC2 instance are as follows:

1. Click on the relevant EC2 instance and go to the security tab

![Alt text](Images/port80_inbound.JPG)

2. Click on the security groups link

![Alt text](Images/port80_inbound2.JPG)

3. Click on 'Edit inbound rules'

![Alt text](Images/port80_inbound3.JPG)

4. To add HTTP (port 80) to the list of rules, click on 'Add rule' and select HTTP from the drop down menu. Change the source to 'Anywhere-IPV4'. Then click on 'Save rules'

![Alt text](Images/port80_inbound4.JPG)

TCP Port 80 opened on the nginx server:

![Alt text](Images/pic_showing_port80_nginx.jpg)

- To access the Nginx web server locally, run the commands `curl http://localhost:80` or `curl http://127.0.0.1:80` in the web server instance:

a. `curl http://localhost:80`

![Alt text](Images/curl_nginx1.jpg)

b. `curl http://127.0.0.1:80`

![Alt text](Images/curl_nginx2.jpg)

The above outputs show successful connection to the Nginx server.

- To access the Nginx web server through a web browser, type the command `http://52.55.96.71` in a web browser (where 52.55.96.71 is the public ip address of the Nginx web server) and hit enter.

![Alt text](Images/nginx_webpage.jpg)

The above page shows that the web server is correctly installed and accessible through the local firewall.

## Installing MySQL

To install MySQL, follow the steps below:

1. Run `sudo apt install mysql-server` in the Nginx web server

![Alt text](Images/mysql_install1.jpg)

![Alt text](Images/mysql_install2.JPG)

2. Verify the MySQL installation by logging into console through the command `sudo mysql`

![Alt text](Images/mysql_login.jpg)

3. It's important to run a security script on the MySQL installation to remove insecure default settings and lockdown access to the database management system.

The command to run to set the MySQL password of this installation to 'PassWord.SQL' is `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '********.***'`. For security purposes, the actual password is hidden in asterisks.

![Alt text](Images/mysql_password_set.JPG)

To start the interactive script, run `sudo mysql_secure_installation`

![Alt text](Images/mysql_script1.jpg)

![Alt text](Images/mysql_script2.jpg)

![Alt text](Images/mysql_script3.jpg)

![Alt text](Images/mysql_script4.jpg)

![Alt text](Images/mysql_script5.jpg)

![Alt text](Images/mysql_script6.jpg)

![Alt text](Images/mysql_script7.jpg)

4. I run the command `sudo mysql -p` to test if I can access the MySQL console using the set password.

![Alt text](Images/mysql_enter_password.jpg)

The login is successful after entering my password

![Alt text](Images/mysql_enter_password2.jpg)

## Installing PHP

To install PHP on a Nginx web server, it's essential to install an external program to handle PHP processing and act as bridge between the PHP interpreter and the web server. For this, I'll need to install `php-fpm`, the "PHP fastCGI process manager" and instruct Nginx to pass PHP requests to the process manager for processing. Also, I'll need to install `php-mysql`, a PHP module allows communication between PHP and MySQL-based databases. 

The command to run these three packages at once is `sudo apt install php-fpm php-mysql`

![Alt text](Images/php_install1.jpg)

![Alt text](Images/php_install2.jpg)

### Configuring Nginx to use the PHP Processor

We can create server blocks on a Nginx web server which are similar to virtual hosts on Apache. These server blocks encapsulate configuration details and are able to host more than one domain on a single server.

For this project, I'll be using pjlemp_stack as the root directory for my domain. The steps are as follows:

1. Create the domain root directory

![Alt text](Images/nginx_configuration.jpg)

2. Change ownership to the $USER environment variable

![Alt text](Images/nginx_configuration1.jpg)

3. Create a new configuration file in Nginx's `sites-available` directory. I'll create a `pjlemp_stack` file in this directory and populate it with the bare-bones configuration

![Alt text](Images/nginx_configuration2.jpg)

![Alt text](Images/nginx_configuration3.jpg)

4. Activate the configuration by creating a link between the config file and Nginx's `sites-enabled` directory. This tells Nginx to use the new config file when it's reloaded.

![Alt text](Images/nginx_configuration4.jpg)

5. Test the configuration by running the command `sudo nginx -t`

![Alt text](Images/nginx_configuration5.jpg)

6. Disable the default Nginx host that is configured to listen through TCP port 80 by running the command `sudo unlink /etc/nginx/sites-enabled/default`

![Alt text](Images/nginx_configuration6.jpg)

7. Reload Nginx to apply the changes made by running the command `sudo systemctl reload nginx`

![Alt text](Images/nginx_configuration7.jpg)

8. Create an `index.html` file in the root directory of the domain created earlier `/var/www/pjlemp_stack` and populate with some information to test that the server block is working.

![Alt text](Images/nginx_configuration8.jpg)

9. Reload the Nginx service by running the command `sudo systemctl reload nginx`

10. Test that the configuration is working by opening the website through the public ip address, using the command `http://<Public-IP-Address>:80`. In this case, the public ip address is the public ip of the web server (52.55.96.71)

![Alt text](Images/nginx_configuration_php.jpg)

The index.html file can be left in place as a temporary landing page for the application until an index.php file is set up to replace it. Once we have a working index.php file, we'd need to remove or rename the index.html file in the domain root directory as it takes precedence over the index.php file by default.

## Testing PHP with Nginx

The LEMP stack is now completely set up. We can test it to verify that Nginx can correctly serve `.php` files off to the PHP processor. To do this follow the steps below:

1. Create a test php file in the domain's root directory

![Alt text](Images/testing_php1.jpg)

2. Paste a line of code in the new file

![Alt text](Images/testing_php2.jpg)

3. Access the page through a web browser by running the command `http://server_domain_or_IP/info.php`

![Alt text](Images/testing_php3.jpg)

Once it is confirmed that the PHP server works fine, it's best to remove the `info.php` file from the root directory because it contains sensitive information.

![Alt text](Images/testing_php4.jpg)

## Retrieving Data from MySQL database with PHP

To retrieve data, we would need to create a test database (DB) with a 'To Do List' and then configure access to it such that the Nginx website can query data from the database and display the data.

The steps to do this are as follows:

1. Connect to the MySQL database using `sudo mysql` command

![Alt text](Images/mysql_connect.jpg)

2. Create a new database by running the command `CREATE DATABASE 'example_database';`

![Alt text](Images/mysql_create_database.jpg)

3. Create a new user with full access to the database just created using the command `CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Password';` where 'example_user' is the name of the new user and 'Password' is the user's password.

![Alt text](Images/mysql_user_creation.JPG)

4. Grant the newly created user permission to the database with full privileges using the command `GRANT ALL ON example_database.* TO 'example_user'@'%';`

![Alt text](Images/mysql_user_permission.JPG)

To test the newly-created user and the password assigned to it, we can run the command `mysql -u dbuser -p`

![Alt text](Images/mysql_user_login.JPG)

To confirm that the user has access to the database run the command `show databases;`

![Alt text](Images/mysql_show_databases.jpg)

5. Create a test table named todo.list by running the command `CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
`
![Alt text](Images/mysql_create_list.jpg)

6. To insert some rows of content into the created table, run the command `INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`. It might be necessary to repeat the command a multiple times to add different values.

![Alt text](Images/mysql_populate_table.jpg)

7. Confirm that the data was successfully added to the table by running the command `SELECT * FROM example_database.todo_list;`

![Alt text](Images/mysql_confirm_data.JPG)

8. Create a new PHP file in the custom root folder of the domain created earlier and populate it with a PHP script that will connect to MySQL and query data from the table created. 

![Alt text](Images/mysql__php_script1.jpg)

![Alt text](Images/mysql__php_script2.jpg)

9. We can then access the infomation on the web browser using the command `http://<Public_domain_or_IP>/todo_list.php`

![Alt text](Images/mysql_php_viewdata.jpg)

The above image shows that our PHP enviroment can connect and interact with the MySQL server.



