
## Implementing a DevOps Tooling Website Solution

As a DevOps Engineer, it's important to know how to implement a tooling website solution that can allow easy and seamless access to DevOps tools within a corporate infrastructure.

For this project, our DevOps tooling website will be running on a 3-tier architecture, which is essentially a client-server architecture that separates the user interface (presentation layer), application processing (application layer), and data management (data layer) into three distinct tiers or layers. 

This DevOps tooling website will consist of the following tools:

1. [Jenkins](https://jenkins.io/) - this is an automation server used to build CI/CD pipelines. It is free and open source.

2. [Kubernetes](https://kubernetes.io/) - used for container orchestration. It is useful for automating computer application deployment, scaling, and management. It is also an open source software.

3. [Jfrog Artifactory](https://jfrog.com/artifactory/) - this is a Universal Repository Manager with support for all major packaging formats, build tools, and CI servers.

4. [Rancher](https://www.rancher.com/) - an open source software platform that allows organizations run and manage [Docker](https://www.docker.com/) and Kubernetes during production.

5. [Grafana](https://grafana.com/) - this is a multi-platform analytics and interactive visualisation web application. It is open source.

6. [Prometheus](https://prometheus.io/) - this is an open-source monitoring system that features a dimensional data model, flexible query language, efficient time-series database, and modern alert systems.

7. [Kibana](https://www.elastic.co/kibana) - this software allows users to visualize Elastic Search data and navigate the Elastic Stack.

The tooling components in this project will be running on the following infrastructure:

1. **Infrastructure**: Oracle VM VirtualBox

2. **Web Servers**: Red Hat Enterprise Linux 8

3. **Database Server**: Ubuntu 20.04 + MySQL

4. **Storage (NFS) Server**: Red Hat Enterprise Linux 8 + NFS Server

5. **Programming Language**: PHP

6. **Code Repository**: [GitHub](https://github.com/darey-io/tooling)

The diagram below shows a pictorial representation of the 3-tier architecture setup for the DevOps Tooling Website project:

![Alt text](Images/3-tier-architecture_for_tooling_website.png)

The diagram above shows three (3) Web Servers sharing a common Database while at the same time having access to a Network File System (NFS) Server as a shared file storage. Even though the NFS is situated on a completely separate hardware, it acts as a local file system through which the Web Servers can access the same files.

Our setup for this project includes three (3) Web Servers running on RHEL8, One (1) Storage Server running on RHEL8 with NFS Server installed, and one (1) Ubuntu Server (ubuntu 20.04) with MySQL installed, which is our Database Server. The diagram below shows the required systems:

![Alt text](Images/vms-tooling.png)

### Implementing a Website using NFS for the Backend File Storage

To prepare our NFS Server for the tooling solution, we'd need to do the following:

**Step 1: Create three (3) Virtual Hard Disks of 10GiB each named xvdf.vdi, xvdg.vdi, and xvdh.vdi in the Hard Disk Selector panel of Oracle VM VirtualBox Manager**

![Alt text](Images/nfs-server1.png)

![Alt text](Images/nfs-server2.png)

![Alt text](Images/nfs-server3.png)

**Step 2: Add the Virtual Hard Disks that were created in the last step to the NFS Server Virtual Machine**

![Alt text](Images/nfs-server4.png)

**Step 3: Launch the NFS Server machine**

![Alt text](Images/nfs-server.png)

**Step 4: Configure LVM on the NFS Server**

- Run the `lsblk` command to check the block devices attached to the NFS Server (NFS001)**

![Alt text](Images/nfs-server5.png)

- Use the `df -h` command to see all mounts and free space on the Server

![Alt text](Images/nfs-server6.png)

- Create a single partition on each of the three (3) disks using the `gdisk` utility

![Alt text](Images/nfs-server7.png)

![Alt text](Images/nfs-server8.png)

![Alt text](Images/nfs-server9.png)

- Use the `lsblk` command to view the newly-configured partitions on the disks

![Alt text](Images/nfs-server10.png)

- Run `sudo lvmdiskscan` command to check for available partitions

![Alt text](Images/nfs-server11.png)

- Mark each of the three partitions as Physical Volumes (PVs) to be used by LVM by running the command `sudo pvcreate /dev/partition`

![Alt text](Images/nfs-server12.png)

- Confirm that the PVs have been created by running the command `sudo pvs`

![Alt text](Images/nfs-server13.png)

- Add all 3 Physical Volumes (PVs) to a Volume Group (VG) named `webdata-vg` by running the command `sudo vgcreate webdata-vg /dev/sdb1 /dev/sdc1 /dev/sdd1`

![Alt text](Images/nfs-server14.png)

- Check if the VG has been successfully created, by running the command `sudo vgs`

![Alt text](Images/nfs-server15.png)

- Create three (3) logical volumes `lv-apps`, `lv-logs`, and `lv-opt` by running the `sudo lvcreate` command

![Alt text](Images/nfs-server16.png)

- Verify that the Logical Volume (LV) has been created successfully by running `sudo lvs`

![Alt text](Images/nfs-server17.png)

- Verify the complete setup by running the commands `sudo vgdisplay -v` and `sudo lsblk`

![Alt text](Images/nfs-server18.png)

![Alt text](Images/nfs-server19.png)

![Alt text](Images/nfs-server20.png)

![Alt text](Images/nfs-server21.png)

![Alt text](Images/nfs-server22.png)

- Format the Logical Volumes to the `xfs` filesystem by running the command `sudo mkfs -t xfs <path to logical volume>`

![Alt text](Images/nfs-server23.png)

- Create mount points `/mnt/apps`, `/mnt/logs`, and `/mnt/opt` on `/mnt` using the `mkdir` command as follows:

```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opt
```
![Alt text](Images/nfs-server24.png)

- Mount the created mount points as follows:

```
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
sudo mount /dev/webdata-vg/lv-opt /mnt/opt'
```
![Alt text](Images/nfs-server25.png)

- Add the changes to `fstab` to ensure that they are persistent and will stay after reboot by running the command `sudo vi /etc/fstab`**

  - Run the `blkid` command to get the `UUID` and filesystem type of the Logical Volumes

  ![Alt text](Images/nfs-server41.png)

  - Populate `fstab` with the necessary details

  ![Alt text](Images/nfs-server42.png)

**Step 5: Install NFS Server, configure it to start on reboot, and ensure it is up and running**

```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![Alt text](Images/nfs-server26.png)

![Alt text](Images/nfs-server27.png)

![Alt text](Images/nfs-server28.png)

**Step 6: Set permissions that will allow the Web Servers to read, write, and execute files on the NFS Server by running these sets of commands**

```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service
```
![Alt text](Images/nfs-server29.png)

![Alt text](Images/nfs-server30.png)

![Alt text](Images/nfs-server31.png)

**Step 7: Configure access to NFS for clients within the same subnet by running the following commands**

*For this project, the subnet cidr for the Web Servers and the NFS Server is `10.19.0.0/24`*

```
sudo vi /etc/exports

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
```
![Alt text](Images/nfs-server32.png)

![Alt text](Images/nfs-server33.png)

- Save the changes to the file, then run the following command `sudo exportfs -arv`

![Alt text](Images/nfs-server34.png)

- Check which ports are being used by NFS by running the command `rpcinfo -p | grep nfs`

![Alt text](Images/nfs-server35.png)

- Open the above ports by adding firewall rules to allow the NFS Server to be accessible from the clients. It's important to open ports `TCP 111`, `UDP 111`, and `UDP 2049`, in addition to `TCP 2049`. The steps to do this are as follows:

    - Create a new zone to accommodate this configuration by running the command `sudo firewall-cmd --new-zone=special --permanent`, and reload the firewall configuration by running `sudo firewall-cmd --reload`

    ![Alt text](Images/nfs-server36.png)

    - Add the source ip/cidr to the firewall rule by running the command `sudo firewall-cmd --zone=special --permanent --add-source=10.19.0.0/24`

    ![Alt text](Images/nfs-server37.png)

    - Add the various ports to the firewall rule by running the `sudo firewall-cmd --zone=special --permanent --add-port=111/tcp` and other commands accordingly.

    ![Alt text](Images/nfs-server38.png)

    - Reload the firewall confirguration by running `sudo firewall-cmd --reload`

    ![Alt text](Images/nfs-server39.png)

    - Check if our new zone is now active with the rules in place by running the command `sudo firewall-cmd --zone=special --list-all`

    ![Alt text](Images/nfs-server40.png)


### Configuring a Backend Database as Part of the 3-Tier Architecture

To install and configure a MySQL DBMS on our Database Server to work with the remote Web Servers, we need to do the following:

**Step 1: Install MySQL Server on the Database Server**

- First thing to do is to update the Ubuntu DB Server by running the `sudo apt update` command

![Alt text](Images/db-server.png)

- Then we can run the `sudo apt install mysql-server -y` command

![Alt text](Images/db-server1.png)

![Alt text](Images/db-server2.png)

*Step 2: Create a database and name it `tooling`

![Alt text](Images/db-server3.png)

*Step 3: Create a database user and name it `webaccess` and grant it permission to the `tooling` database to do anything only from the webservers `subnet cidr`.

***Note: Since this version of RHEL8 supplies MySQL8 and PHP7.2. The command to create the use is slightly different. This is necessary to take care of any compatibility issues.***

![Alt text](Images/db-server4.png)

![Alt text](Images/db-server4-1.png)

![Alt text](Images/db-server4-2.png)

- Also, change the default authentication plugin by adding `default-authentication-plugin=mysql_native_password` to the SQL configuration file at `/etc/mysql/mysql.conf.d/mysqld.cnf`

![Alt text](Images/db-server4-3.png)

 - Confirm that the database has been created

 ![Alt text](Images/db-server5.png)

 - Open up Port 3306 on the database

 ![Alt text](Images/db-server6.png)

 - Change the `bind-address` and `mysqlx-bind-address` in the `/etc/mysql/mysql.conf.d/mysqld.cnf` to from `127.0.0.1` to `0.0.0.0`

 ![Alt text](Images/db-server7.png)

 ![Alt text](Images/db-server8.png)

 - Restart the `mysql` service by running the command `sudo systemctl restart mysql`

 ![Alt text](Images/db-server9.png)

 ### Preparing the Web Servers

 We need to ensure that the Web Servers can share the same content from our shared storage solutions namely the NFS Server and the MySQL database. For storing the shared files to be used by the Web Servers, we'll use the NFS Server and mount the logical volume created earlier `lv-apps` to `/var/www` where Apache stores files that it serves to users.

 In this setup, our Web Servers are `stateless` - we can remove them or add new ones at anytime without affecting the integrity of the data on the NFS or in the database.

 Our steps are as follows:

 **Step 1: Configure the NFS Client on the Web Servers**

 - Install the NFS client on the Web Servers by running the command `sudo yum install nfs-utils nfs4-acl-tools -y`

 ![Alt text](Images/wb-server.png)

 **Step 2: Create the `/var/www` directory and mount the `/mnt/apps` directory from the NFS Server on it**

  ![Alt text](Images/wb-server1.png)

  **Step 3: Verify that the NFS was mounted by running the `df -h` command

  ![Alt text](Images/wb-server2.png)

  **For WebServer002:**

  ![Alt text](Images/wb-server3.png)

  **For WebServer003:**

  ![Alt text](Images/wb-server4.png)

  **Step 3: Add the changes to `fstab` to ensure that they are persistent and will stay after reboot by running the command `sudo vi /etc/fstab` and adding the following line `<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0`**

  ![Alt text](Images/wb-server5.png)

  ![Alt text](Images/wb-server6.png)

  **For WebServer002:**

  ![Alt text](Images/wb-server7.png)
  
  **For WebServer003:**

  ![Alt text](Images/wb-server8.png)

  **Step 4: Install Apache and PHP**

  Run the block of code below on the Web Servers to install Apache and PHP on them

  ```
  sudo yum install httpd -y

sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo yum module reset php

sudo yum install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

sudo systemctl status php-fpm

sudo setsebool -P httpd_execmem 1
  ```
![Alt text](Images/wb-server9.png)

![Alt text](Images/wb-server10.png)

![Alt text](Images/wb-server11.png)

![Alt text](Images/wb-server12.png)

![Alt text](Images/wb-server13.png)

![Alt text](Images/wb-server14.png)

![Alt text](Images/wb-server15.png)

![Alt text](Images/wb-server16.png)

**For WebServer002:**

![Alt text](Images/wb-server17.png)

![Alt text](Images/wb-server18.png)

![Alt text](Images/wb-server19.png)

![Alt text](Images/wb-server20.png)

![Alt text](Images/wb-server21.png)

![Alt text](Images/wb-server22.png)

![Alt text](Images/wb-server23.png)

![Alt text](Images/wb-server24.png)

**For WebServer003:**

![Alt text](Images/wb-server25.png)

![Alt text](Images/wb-server26.png)

![Alt text](Images/wb-server27.png)

![Alt text](Images/wb-server28.png)

![Alt text](Images/wb-server29.png)

![Alt text](Images/wb-server30.png)

![Alt text](Images/wb-server31.png)

![Alt text](Images/wb-server32.png)

**Step 5: Confirm that the Apache files and directories are available in the Web Servers in the `/var/www` directory, and in the NFS Server in `/mnt/apps`**

![Alt text](Images/wb-server33.png)

**On WebServer002:**

![Alt text](Images/wb-server34.png)

**On WebServer003:**

![Alt text](Images/wb-server35.png)

**On NFS Server:**

![Alt text](Images/wb-server36.png)

**Step 6: Mount the `/mnt/logs` directory from the NFS Server on the `/var/log/httpd` directory of the Web Servers**

![Alt text](Images/wb-server37.png)

**On WebServer002:**

![Alt text](Images/wb-server38.png)

**On WebServer003:**

![Alt text](Images/wb-server39.png)

- Add the change to `fstab` to ensure that it is persistent and will stay after reboot

![Alt text](Images/wb-server40.png)

![Alt text](Images/wb-server41.png)

![Alt text](Images/wb-server42.png)

**Step 7: Fork the tooling source code from the `Darey.io Github account` to my Github account**

Here are the steps I followed to fork the `tooling` repository to my Github account from `Darey.io Github account`:

- Navigate to the tooling source code on Github

![Alt text](Images/wb-server43.png)

- Click on `fork` at the top of the page and that takes you to the `Create a new fork` page. Click on `Create fork` at the bottom of the page

![Alt text](Images/wb-server44.png)

![Alt text](Images/wb-server45.png)

- A copy of the `tooling` repository is added to my Github account

![Alt text](Images/wb-server46.png)

**Step 8: Download the `tooling` repository from my Github account to WebServer001**

- Install the `git` package in the Web Server by running the command `sudo yum install git`

![Alt text](Images/wb-server48.png)

![Alt text](Images/wb-server49.png)

- Type `git init` to initialize an empty git repository

![Alt text](Images/wb-server50.png)

- Click on `Code` at the top of the Github account page and copy the repository link

![Alt text](Images/wb-server47.png)

- Type `git clone` in the Web Server terminal and paste the copied link

![Alt text](Images/wb-server51.png)

**Step 9: Deploy the tooling website's code to WebServer001 by copying the contents of the `html` file from the repository to `/var/www/html` on the Web Server. The same content will exist on WebServer002 and WebServer003**

![Alt text](Images/wb-server52.png)

![Alt text](Images/wb-server52-1.png)

![Alt text](Images/wb-server52-2.png)

**Step 10: Open `Port 80` and the `http` service on the WebServers by running the command `sudo firewall-cmd --zone=public --permanent --add-port=80/tcp` and `sudo firewall-cmd --zone=public --permanent --add-service=http`. Reload the firewall by running the command `sudo firewall-cmd --reload`**

![Alt text](Images/wb-server53.png)

![Alt text](Images/wb-server53-1.png)

![Alt text](Images/wb-server54.png)

**For WebServer002:**

![Alt text](Images/wb-server59.png)

**For WebServer003:**

![Alt text](Images/wb-server60.png)

**Step 11: Disable SELinux by running the command `sudo setenforce 0`. Make the change permanent by editing the selinux config file using the command `sudo vi /etc/sysconfig/selinux`. Set `SELINUX=disabled`**

![Alt text](Images/wb-server55.png)

![Alt text](Images/wb-server56.png)

![Alt text](Images/wb-server57.png)

**For WebServer002:**

![Alt text](Images/wb-server61.png)

![Alt text](Images/wb-server62.png)

**For WebServer003:**

![Alt text](Images/wb-server63.png)

![Alt text](Images/wb-server64.png)

**Step 12: Edit the `httpd.conf` file under the `ServerName` section with the Web Server IP Address to allow the httpd service recognise it**

  ![Alt text](Images/wb-server58-1.png)

  ![Alt text](Images/wb-server58-2.png)

  - Start the `httpd` service and enable it to run automatically at boot time**

![Alt text](Images/wb-server58.png)

**For WebServer002:**

![Alt text](Images/wb-server65.png)

**For WebServer003:**

![Alt text](Images/wb-server66.png)

**Step 13: Update the website's configuration to connect to the database by editing the `functions.php` file in `/var/www/html`. Edit the `connect to database` section and fill out the necessary details with the `database user`, `database password`, `database name` and `private IP address for the database server`**

![Alt text](Images/wb-server67.png)

![Alt text](Images/wb-server68.png)

**Step 14: Install MySQL client on the Web Servers**

![Alt text](Images/wb-server69.png)

![Alt text](Images/wb-server70.png)

**For WebServer002:**

![Alt text](Images/wb-server71.png)

![Alt text](Images/wb-server72.png)

**For WebServer003:**

![Alt text](Images/wb-server73.png)

![Alt text](Images/wb-server74.png)

**Step 14: Apply the `tooling-db.sql` script to the database by running the command `mysql -h <database-private-ip> -u <db-username> -p <database> < tooling-db.sql`. Ensure you run this command in the `tooling` folder downloaded earlier**

![Alt text](Images/wb-server75.png)

**Step 15: Open the tooling website from the browser using WebServer001 IP Address and login with the necessary credentials `user: admin` and `password: admin`**

![Alt text](Images/tw-pic.png)

![Alt text](Images/tw-pic1.png)

![Alt text](Images/tw-pic2.png)

**For WebServer002**

Open the tooling website from the browser using WebServer002 IP Address and login with the necessary credentials `user: admin` and `password: admin`**

![Alt text](Images/tw-pic3.png)

![Alt text](Images/tw-pic4.png)

![Alt text](Images/tw-pic5.png)

**For WebServer003**

Open the tooling website from the browser using WebServer003 IP Address and login with the necessary credentials `user: admin` and `password: admin`**

![Alt text](Images/tw-pic6.png)

![Alt text](Images/tw-pic7.png)

![Alt text](Images/tw-pic8.png)
































    



