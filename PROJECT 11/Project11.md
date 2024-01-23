
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

**Step 1:** Install Java  - a key requirement for Jenkins to work

- Install the latest Java JRE by running the command `sudo apt install default-jre`




 