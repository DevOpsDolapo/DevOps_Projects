
## Implementing Ansible Configuration Management: Ansible-Automate Project

This project aims to give us a better appreciation of DevOps tools by using Ansible Configuration Management to automate a lot of our routine tasks. We'll be automating our processes from Project 7 to 10 and writing code using `YAML`.

For this project, our architecture will include:

- Jump Server (Bastion Host), which is essentially an intermediary server that provides access to an internal network. It shields the servers within the internal network against direct access from the internet or through direct `SSH` connections. This will also be our Ansible Client where we'll be running Playbooks.   

- Two (2) Web Servers

- One (1) Database Server

- One (1) NFS Server

- One (1) Load Balancer Server 

Our architecture will be running on the following infrastructure components:

1. **Infrastructure**: Oracle VM VirtualBox

2. **Jenkins/Ansible Server**: Ubuntu 20.04 + Jenkins + Ansible

3. **Web Servers**: Red Hat Enterprise Linux 8

4. **Database Server**: Ubuntu 20.04 + MySQL

5. **Storage (NFS) Server**: Red Hat Enterprise Linux 8 + NFS Server

6. **Load Balancer**: Ubuntu 20.04 + Nginx

7. **GitHub**: For creating our repository

8. **Visual Studio Code**: For preparing our development environment

### Installing and Configuring a Jenkins/Ansible Server

With our Jenkins/Ansible server already installed and running Ubuntu 20.04, we can go ahead to install Jenkins on our server as follows:

**Step 1: Install Java  - a key requirement for Jenkins to work**

- Install the latest Java JRE by running the command `sudo apt install default-jre`

![Alt text](Images/jen-server.png)

![Alt text](Images/jen-server1.png)

- To compile and run some specific Java-based software, we'll need to install Java JDK by running the command `sudo apt install default-jdk`

![Alt text](Images/jen-server2.png)

![Alt text](Images/jen-server3.png)

**Step 2: Install Jenkins**

To install Jenkins on the Jenkins/Ansible Server, we'll need to do the following:

- Add the Jenkins repository key to the server by running the command `curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null`

![Alt text](Images/jen-server4.png)

- Append the Debian package repository address to the Server's `sources.list` by running the command `echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null`

![Alt text](Images/jen-server5.png)

- Update the server by running the `sudo apt update` command

![Alt text](Images/jen-server6.png)

- Install Jenkins by running the `sudo apt install jenkins` command

![Alt text](Images/jen-server7.png)

- Start Jenkins by running the command `sudo systemctl start jenkins` and check if Jenkins is running by using the command `sudo systemctl status jenkins`

![Alt text](Images/jen-server8.png)

- Open up Jenkin's default port `8080` on the firewall by running the command `sudo ufw allow 8080`

![Alt text](Images/jen-server9.png)

- Check firewall status by running the command `sudo ufw status`

![Alt text](Images/jen-server10.png)

- Set up the Jenkins installation by visiting Jenkins using the domain name or IP address of the Jenkins/Ansible Server along with port `8080`. For our server this will be `10.19.0.119:8080` This takes us to the `Unlock Jenkins` page

![Alt text](Images/jen-server11-0.png)

![Alt text](Images/jen-server11.png)

- Display the password in the Jenkins/Ansible Server's terminal by running the command `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` 

![Alt text](Images/jen-server12.png)

- Copy the password from the terminal and paste it into the Administrator password field, then click Continue.

![Alt text](Images/jen-server13.png)

- Customize Jenkins by installing the suggested plugins

![Alt text](Images/jen-server14.png)

![Alt text](Images/jen-server15.png)

- Once the installation is complete, create the first administrative user by filling out the form with the necessary details



- On the `Instance Configuration` page, confirm the preferred URL for your Jenkins instance. Use either the domain name for your server or the server’s IP address:



- After confirming the appropriate information, click Save and Finish. You’ll receive a confirmation page confirming that “Jenkins is Ready!”:





 