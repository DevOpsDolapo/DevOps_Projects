
# Implementing WordPress Website With LVM Storage Management

## What is a 3-Tier Architecture?

The 3-tier architecture is a client-server architecture that separates the user interface (presentation layer), application processing (application layer), and data management (data layer) into three distinct tiers or layers. It is commonly used in modern web solutions and enterprise systems because it provides scalability, security, and flexibility.

The diagram below shows a 3-tier architecture setup:

![Alt text](Images/3-tier-architecture.png)

Here is a brief description of each tier in the 3-tier architecture:

**Presentation Tier**: This is the user interface or client layer of an application or web solution. It is responsible for presenting data to the user and receiving input from the user. This layer can be a desktop application, mobile app, or web browser.

**Application Tier**: This is the middle layer of the 3-tier architecture. It is responsible for processing and managing the business logic of the web solution or application. This tier communicates with the presentation tier to receive user input and communicates with the data management tier to retrieve or store data. This tier includes application servers, web servers, or Application Programming Interfaces (APIs).

**Data Management Tier**: This is the third layer of the 3-tier architecture. It is responsible for managing and storing data. This tier includes data warehouses, databases, or data lakes. The data management tier communicates with the application tier to receive or store data.

For DevOps engineers, a deep understanding of the core components of web solutions and the ability to handle common challenges, troubleshoot issues, and effectively manage a website's resources are essential. Thus, the main thrust of this project is to implement a web solution using different technologies. 

My 3-tier architecture setup for this project will be:

1. A PC to serve as a client

2. An EC2 Linux server (the web server running on RedHat Linux OS), where I'll install WordPress. (Due to AWS EC2 charges, I had to switch to an Oracle VirtualBox VM)

3. An EC2 Linux server (the database server running on RedHat Linux OS). Due to AWS EC2 charges, I had to switch to an Oracle VirtualBox VM.

## Configuring a Web Server and Implementing LVM Storage Subsystem on It

To configure a web server, here are the steps to follow:

### Create A Web Server

Launch an EC2 instance to use as the Web Server.  

**Step 1: Login into AWS and then click on `Launch instance` on the EC2 Dashboard**

![Alt text](Images/aws_launch-instance.png)

**Step 2: On the 'Launch an instance' page, fill in the details of the EC2 instance**

![Alt text](Images/aws_launch-instance2.png)

**Step 3: Check that the instance is up and running**

![Alt text](Images/aws_launch-instance3.png)

### Create Storage Volumes

Create 3 storage volumes of 10GiB each in the same Availability Zone (AZ) as the Web Server.

**Step 1: Click on the 'Volumes' link on the EC2 Dashboard**

![Alt text](Images/aws_volume_creation1.png)

**Step 2: On the 'Volumes' page, click on 'Create volume'**

![Alt text](Images/aws_volume_creation2.png)

**Step 3: On the Volume Creation page, enter the Volume settings then click on 'Create volume'**

![Alt text](Images/aws_volume_creation3.png)

![Alt text](Images/aws_volume_creation4.png)

**Step 4: Repeat 'Step 3' to create two more 10GiB volumes**

The three newly created volumes are shown below

![Alt text](Images/aws_volume_creation5.png)

### Attach the Newly Created Storage Volumes to the EC2 Instance

To attach the storage volumes created in the last section, follow these steps

**Step 1: Click on the 'Volume ID' of one of the Volumes to go into the Volume Details**

![Alt text](Images/aws_volume_attach1.png)

**Step 2: Under the 'Actions' tab, click on 'Attach Volume'**

![Alt text](Images/aws_volume_attach2.png)

**Step 3: On the 'Attach volume' page, select the relevant EC2 instance, then click on 'Attach volume' at the bottom of the page**

![Alt text](Images/aws_volume_attach3.png)

![Alt text](Images/aws_volume_attach4.png)

**Step 4: Repeat Steps 1 to 3 above to attach the other Storage Volumes to the EC2 instance, then check under the 'Storage' tab of the EC2 instance to confirm that they have been attached**

![Alt text](Images/aws_volume_attach5.png)

### Implement LVM Storage Configuration on the Web Server

After provisioning the Web Server EC2 instance, the next thing is to configure the LVM Storage Subsystem.

The steps to do this are as follows:

**Step 1: Connect to the Web Server using its public IP address on the Termius software**

![Alt text](Images/termius-connect1.png)

![Alt text](Images/termius-connect2.png)

**Step 2: Use the `lsblk` command to check the block devices attached to the Web Server and the `df -h` command to see all mounts and free space on the Server**

The `lsblk` command shows that `xvdf`, `xvdg`, and `xvdh` are attached.
![Alt text](Images/webserver-storages1.png)

![Alt text](Images/webserver-storages2.png)

**Step 3: Create a single partition on each of the 3 disks using the `gdisk` utility, using the command `sudo gdisk /dev/xvdf` for the first disk and the relevant names for the other two disks**

![Alt text](Images/webserver-storages3.png)

![Alt text](Images/webserver-storages4.png)

![Alt text](Images/webserver-storages5.png)

**Step 4: Use the `lsblk` command to view the newly-configured partitions on the disks**

![Alt text](Images/webserver-storages6.png)

**Step 5: Install the `lvm2` package by running the `sudo yum install lvm2` command, then run `sudo lvmdiskscan` command to check for available partitions**

![Alt text](Images/webserver-storages7.png)

![Alt text](Images/webserver-storages8.png)

![Alt text](Images/webserver-storages9.png)

**Step 6: Mark each of the three partitions as Physical Volumes (PVs) to be used by LVM by running the command `sudo pvcreate /dev/partition`**

![Alt text](Images/webserver-storages10.png)

**Step 7: Confirm that the PVs have been created by running the command `sudo pvs`**

![Alt text](Images/webserver-storages11.png)

**Step 8: Add all 3 Physical Volumes (PVs) to a Volume Group (VG) named `webdata-vg` using the command `sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`**

![Alt text](Images/webserver-storages12.png)

**Step 9: To check if the VG has been successfully created run the command `sudo vgs`**

![Alt text](Images/webserver-storages13.png)

**Step 10: Create 2 logical volumes `apps-lv` and `logs-lv` using the lvcreate command. `apps-lv` will be used to store data for the website and `logs-lv` will be used to store data for logs**

![Alt text](Images/webserver-storages14.png)

**Step 11: Verify that the Logical Volumes (LVs) has been created successfully by running `sudo lvs`**

![Alt text](Images/webserver-storages15.png)

**Step 12: Verify the complete setup by running the commands `sudo vgdisplay -v #view complete setup - VG, PV, and LV` and `sudo lsblk`**

![Alt text](Images/webserver-storages16.png)
![Alt text](Images/webserver-storages17.png)

![Alt text](Images/webserver-storages18.png)

**Step 13: Format the LVs to the `ext4` filesystem by running the commands `sudo mkfs -t ext4 /dev/webdata-vg/apps-lv` and `sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`**

![Alt text](Images/webserver-storages19.png)

![Alt text](Images/webserver-storages20.png)

**Step 14: Create /var/www/html directory to store website files by running the command `sudo mkdir -p /var/www/html` and /home/recovery/logs directory to store backups of log data by running the command `sudo mkdir -p /home/recovery/logs`**

![Alt text](Images/webserver-storages21.png)

**Step 15: Mount `/var/www/html` on the `apps-lv` logical volume by running the command `sudo mount /dev/webdata-vg/apps-lv /var/www/html/`**

![Alt text](Images/webserver-storages22.png)

**Step 16: Back up all the files in `/var/log` into the `/home/recovery/logs` directory. This step is necessary before mounting the filesystem because all the existing data on `/var/log` will be deleted in the next step**

![Alt text](Images/webserver-storages23.png)

**Step 17: Mount `/var/log` on the `logs-lv` logical volume by running the command `sudo mount /dev/webdata-vg/logs-lv /var/log`**

![Alt text](Images/webserver-storages24.png)

**Step 18: Restore the backed-up log files back into the `/var/log` directory by running the command `sudo rsync -av /home/recovery/logs/. /var/log`**

![Alt text](Images/webserver-storages25.png)

**Step 19: Update the `/etc/fstab` file to enable auto-mount every time the Web Server is restarted. The UUID of each device will be used to update `/etc/fstab`**

Run `sudo blkid` command to get the UUIDs

![Alt text](Images/webserver-storages26.png)

Then run `sudo vi /etc/fstab`

![Alt text](Images/webserver-storages27.png)

**Step 20: Test the configuration and reload the daemon by running the commands `sudo mount -a` and `sudo systemctl daemon-reload`**

![Alt text](Images/webserver-storages28.png)

**Step 21: Verify the setup by running `df -h`**

![Alt text](Images/webserver-storages29.png)

## Configuring a Database Server and Implementing LVM Storage Subsystem on It

**Please note: Due to the restrictions on AWS Free Tier EC2 instances and the resultant accruing charges, I'll be switching to Oracle VirtualBox (VMs) running on RHEL7 for the remainder of this project. The outcome of running steps 1 to 21 above on the Web Server VM is shown below**

![Alt text](Images/webserver-storages_vm.png)

**To setup the Database server, the relevant steps from 1 to 21 above will be replicated on another VM**

**Step 1: Use the `lsblk` command to check the block devices attached to the Database Server and the `df -h` command to see all mounts and free space on the Server**

The `lsblk` command shows that `sdb`, `sdc`, and `sdd` are attached.

![Alt text](Images/dbserver_storages1.png)

![Alt text](Images/dbserver_storages2.png)

**Step 2: Create a single partition on each of the 3 disks using the `gdisk` utility, using the command `sudo gdisk /dev/sdb` for the first disk and the relevant names for the other two disks**

![Alt text](Images/dbserver_storages3.png)

![Alt text](Images/dbserver_storages4.png)

![Alt text](Images/dbserver_storages5.png)

**Step 3: Use the `lsblk` command to view the newly-configured partitions on the disks**

![Alt text](Images/dbserver_storages6.png)

**Step 4: Run `sudo lvmdiskscan` command to check for available partitions**

![Alt text](Images/dbserver_storages7.png)

**Step 5: Mark each of the three partitions as Physical Volumes (PVs) to be used by LVM by running the command `sudo pvcreate /dev/partition`**

![Alt text](Images/dbserver_storages8.png)

**Step 6: Confirm that the PVs have been created by running the command `sudo pvs`**

![Alt text](Images/dbserver_storages9.png)

**Step 7: Add all 3 Physical Volumes (PVs) to a Volume Group (VG) named `dbdata-vg` using the command `sudo vgcreate dbdata-vg /dev/sdb1 /dev/sdc1 /dev/sdd1`**

![Alt text](Images/dbserver_storages10.png)

**Step 8: To check if the VG has been successfully created run the command `sudo vgs`**

![Alt text](Images/dbserver_storages11.png)

**Step 9: Create 1 logical volume `db-lv` using the command `sudo lvcreate -n db-lv -L 20G dbdata-vg`**

![Alt text](Images/dbserver_storages12.png)

**Step 10: Verify that the Logical Volume (LV) has been created successfully by running `sudo lvs`**

![Alt text](Images/dbserver_storages13.png)

**Step 11: Verify the complete setup by running the commands `sudo vgdisplay -v #view complete setup - VG, PV, and LV` and `sudo lsblk`**

![Alt text](Images/dbserver_storages14.png)

![Alt text](Images/dbserver_storages15.png)

![Alt text](Images/dbserver_storages16.png)

![Alt text](Images/dbserver_storages18.png)

**Step 12: Format the LV to the `ext4` filesystem by running the commands `sudo mkfs -t ext4 /dev/dbdata-vg/db-lv`**

![Alt text](Images/dbserver_storages19.png)

**Step 13: Create /db directory to store database files by running the command `sudo mkdir /db`**

![Alt text](Images/dbserver_storages21.png)

**Step 14: Mount `/db` on the `db-lv` logical volume by running the command `sudo mount /dev/dbdata-vg/db-lv /db/`**

![Alt text](Images/dbserver_storages22.png)

**Step 15: Update the `/etc/fstab` file to enable auto-mount every time the Database Server is restarted. The UUID of the device will be used to update `/etc/fstab`**

Run `sudo blkid` command to get the UUIDs

![Alt text](Images/dbserver_storages28.png)

Then run `sudo vi /etc/fstab`

![Alt text](Images/dbserver_storages29.png)

**Step 16: Test the configuration and reload the daemon by running the commands `sudo mount -a` and `sudo systemctl daemon-reload`**

![Alt text](Images/dbserver_storages30.png)

**Step 17: Verify the setup by running `df -h`**

![Alt text](Images/dbserver_storages31.png)

## Installing WordPress on the Web Server

To install WordPress on the Web Server, we need to follow the steps below:

**Step 1: Update the repository by running the command `sudo yum update -y`**

![Alt text](Images/wp-images1.png)

**Step 2: Install wget, Apache, and its dependencies**

I'll need to run the command `sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json` to install the above packages.

![Alt text](Images/wp-images2.png)

![Alt text](Images/wp-images3.png)

![Alt text](Images/wp-images4.png)

As shown in the information at the start of the installation process, the wget and httpd packages have already been installed.

**Step 3: Confirm that Apache is running on the Web Server by executing the command `sudo systemctl status httpd`**

![Alt text](Images/wp-images9.png)

**Step 4: Install PHP and Its Dependencies**

To install PHP and its dependencies, we'll need to run the following commands 
```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```
![Alt text](Images/wp-images10.png)

![Alt text](Images/wp-images11.png)

![Alt text](Images/wp-images12.png)

![Alt text](Images/wp-images13.png)

![Alt text](Images/wp-images14.png)

**Step 5: Restart Apache on the Web Server**

![Alt text](Images/wp-images17.png)

**Step 6: Download the WordPress package and copy it to the `/var/www/html` directory on the Web Server**

Run these series of commands:
```
mkdir wordpress
cd   wordpress
sudo wget http://wordpress.org/latest.tar.gz
sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php
cp -R wordpress/. /var/www/html/ #the contents of the wordpress folder should be copied, not the wordpress folder itself
```
![Alt text](Images/wp-images18.png)

![Alt text](Images/wp-images19.png)

![Alt text](Images/wp-images20.png)

![Alt text](Images/wp-images21.png)

![Alt text](Images/wp-images21-1.png)

**Step 7: Configure SELinux Policies**

Run the following commands to setup SELinux Policies:
```
 sudo chown -R apache:apache /var/www/html/wordpress
 sudo chcon -h system_u:object_r:httpd_sys_content_t /var/www/html/wordpress -R
 sudo setsebool -P httpd_can_network_connect=1
```
![Alt text](Images/wp-images22.png)

## Installing MySQL on the Web Server and the Database Server

**Step 1: To install MySQL on both the Web Server and the Database Server, run the following commands**

```
sudo yum update
sudo yum install mysql-server
```

![Alt text](Images/wp-images23.png)

![Alt text](Images/wp-images24.png)

![Alt text](Images/wp-images25.png)

![Alt text](Images/wp-images26.png)

![Alt text](Images/wp-images26-1.png)

![Alt text](Images/wp-images26-2.png)

**Step 2: Verify that the MySQL service is up and running on both the Web Server and the Database Server**

![Alt text](Images/wp-images27.png)

![Alt text](Images/wp-images27-1.png)

## Configuring the Connection Between the Database and WordPress

**Step 1: It's important to run a security script on the MySQL server installation to remove insecure default settings and lockdown access to the database management system. 

![Alt text](Images/db_creation1.png)

![Alt text](Images/db_creation2.png)

![Alt text](Images/db_creation3.png)

**Step 2: Then run these commands sequentially**
```
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'mydbuser'@'%' IDENTIFIED WITH mysql_native_password BY 'mypassword';
GRANT ALL PRIVILEGES ON *.* TO 'mydbuser'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```
![Alt text](Images/db_creation4.png)

**Step 3: Configure WordPress to Connect to Remote Database**

**Hint:** It's important to open up MySQL port 3306 on the Database Server. To do this, we run the following commands on the Database Server

```
sudo firewall-cmd --zone=public --permanent --add-port=3306/tcp
sudo firewall-cmd --zone=public --permanent --add-service=mysql

```
![Alt text](Images/db_firewall1.png)

**Step 4: Reload the firewall rules to apply changes by running the command `sudo firewall-cmd --reload`**

![Alt text](Images/db_firewall2.png)

**Step 5: Verify the firewall rules by running the command `firewall-cmd --list-all`**

![Alt text](Images/db_firewall3.png)

**Step 6: Set the Bind Address on the Database Server by editing the database configuration file (`my.cnf`) in `/etc`, running the command `sudo vi /etc/my.cnf`**

![Alt text](Images/db_firewall4-0.png)

The below entries are added to the file:

![Alt text](Images/db_firewall4.png)

**Step 7: Restart the `mysqld service` for the changes to take effect by running the command `sudo systemctl restart mysqld`

![Alt text](Images/db_firewall5.png)

**Step 7: Edit the `wp-config.php` file on the Web Server and populate it with the relevant details which are the `DB_NAME, DB_USER, DB_PASSWORD, and DB_HOST` settings**

![Alt text](Images/db_client1.png)

![Alt text](Images/db_client2.png)

**Step 8: Restart the `httpd` service by running the command `sudo systemctl restart httpd`**

![Alt text](Images/db_client3.png)

**Step 9: Rename the Apache welcome page**

![Alt text](Images/db_client4.png)

**Step 10: Connect to the Database Server from the Web Server by running the command `sudo mysql -u 'user' -p -h <DB-Server-Private-IP-address>`, where `user` is the database user created earlier and `<DB-Server-Private-IP-address>` is the Private IP address of the database server.**

![Alt text](Images/db_client5.png)

From the above, it's clear that the Web Server can access the database hosted on the Database Server.

**Step 11: Let's run a query from the Web Server database client to the Database Server**

![Alt text](Images/db_client6.png)

**Step 11: Enable inbound rules for TCP port 80 on the Web Server**

![Alt text](Images/db_client7.png)

**Step 12: Let's try and access WordPress through the broswer on the Web Server**

![Alt text](Images/wp-page1.png)

![Alt text](Images/wp-page2.png)

![Alt text](Images/wp-page3.png)

![Alt text](Images/wp-page4.png)

![Alt text](Images/wp-page5.png)

![Alt text](Images/wp-page6.png)




























 

