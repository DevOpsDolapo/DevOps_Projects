
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

With our Jenkins/Ansible server (JAN001) already installed and running Ubuntu 20.04, we can go ahead to install Jenkins on the server as follows:

**Step 1: Install Java - a key requirement for Jenkins to work**

- Install the latest Java JRE by running the command `sudo apt install default-jre`

![Alt text](Images/jen-server.png)

![Alt text](Images/jen-server1.png)

- To compile and run some specific Java-based software, we'll need to install Java JDK by running the command `sudo apt install default-jdk`

![Alt text](Images/jen-server2.png)

![Alt text](Images/jen-server3.png)

**Step 2: Install Jenkins**

To install Jenkins on `JAN001`, we'll need to do the following:

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

- Set up the Jenkins installation by visiting Jenkins using the domain name or IP address of `JAN001` along with port `8080`. For our server this will be `10.19.0.119:8080` This takes us to the `Unlock Jenkins` page

![Alt text](Images/jen-server11-0.png)

![Alt text](Images/jen-server11.png)

- Display the password in `JAN001` terminal by running the command `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` 

![Alt text](Images/jen-server12.png)

- Copy the password from the terminal and paste it into the Administrator password field, then click Continue.

![Alt text](Images/jen-server13.png)

- Customize Jenkins by installing the suggested plugins

![Alt text](Images/jen-server14.png)

![Alt text](Images/jen-server15.png)

- Once the installation is complete, create the first administrative user by filling out the form with the necessary details, then click on `save and continue`

![Alt text](Images/jen-server16.png)

- On the `Instance Configuration` page, confirm the preferred URL for your Jenkins instance. Use either the domain name for your server or the server’s IP address, then click on `Save and Finish`

![Alt text](Images/jen-server17.png)

- After confirming the appropriate information, click Save and Finish. You’ll receive a confirmation page confirming that “Jenkins is Ready!”

![Alt text](Images/jen-server18.png)

- Click on `Start using Jenkins` to visit the main Jenkins dashboard:

![Alt text](Images/jen-server19.png)

**Step 3: Install and Configure Ansible on the Jenkins/Ansible Server**

- Create a new repository called `ansible-config-mgt` in GitHub

    - Login into GitHub and go to the repositories page. Click on `New` on the right hand side of the page.

    ![Alt text](Images/github-repo.png)

    - On the `Create a new repository` page, fill in all the details for the new repository and click on `Create repository` at the bottom of the page

    ![Alt text](Images/github-repo1.png)

    ![Alt text](Images/github-repo2.png)

- Install Ansible in `JAN001` by running the commands `sudo apt update` and `sudo apt install ansible`

![Alt text](Images/jen-server20.png)

![Alt text](Images/jen-server21.png)

![Alt text](Images/jen-server22.png)

- Check the version of Ansible running by using the command `ansible --version`

![Alt text](Images/jen-server23.png)

**Step 4: Configure a Jenkins Build Job to Archive the `ansible-config-mgt` Repository**

This job archives the repository content every time a change is made to it.

- Create a new Freestyle project `ansible` in Jenkins and connect it to the `ansible-config-mgt` repository

    - Login into Jenkins and click on `Create a job` in the dashboard

    ![Alt text](Images/jen-server24.png)

    - Fill in the name of the job - `ansible`, select on `Freestyle project` and click on `OK`

    ![Alt text](Images/jen-server25.png)

    - Copy the URL of the `ansible-config-mgt` repository from GitHub, then select `Git` under `Source Code Management` in Jenkins and add the URL

    ![Alt text](Images/jen-server27.png)

    ![Alt text](Images/jen-server26.png)

    - Under the `Branches to build` section, change the Branch specifier to `main`

    ![Alt text](Images/jen-server26-1.png)

    - Scroll down to `Build Triggers` and click on `GitHub hook trigger for GITScm polling`

    ![Alt text](Images/jen-server28.png)

    - Configure a webhook in GitHub and set the webhook to trigger the `ansible` build. Go to `Settings`, then `Webhooks` and `Add webhook`

    ![Alt text](Images/jen-server29.png)

    ![Alt text](Images/jen-server30.png)

    ![Alt text](Images/jen-server31.png)

    - Under `Payload URL` option, add the `<jenkins_url:8080/github-webhook`. Set `Content type` to `application/json`, and click on `Add webhook` at the bottom of the page.

    *Note: Due to the fact that my setup was on VirtualBox and behind my router firewall, numerous attempts to configure a Webhook from GitHub to Jenkins failed. I had to install a software called `ngrok` on the Jenkins/Ansible Server, which generated a Public IP that GitHub could reach. Also, I had to employ a new set of IPs to work with the current configuration. My Jenkins/Ansible Server is now running on `192.168.0.2:8080`*

    ![Alt text](Images/jen-server32.png)

    ![Alt text](Images/jen-server32-1.png)

    - Configure a post-build job to archive all artifacts by clicking on `Archive the artifacts` under the `Post-build actions` section. Enter `**` in the field then `Apply` and `Save`

    ![Alt text](Images/jen-server33.png)

    - Click on `Build Now` to confirm if the setup is fine and check `Build History` for the result of our first build. 

    ![Alt text](Images/jen-server34.png)

    ![Alt text](Images/jen-server35.png)

    - Test the setup by making some changes in the `README.md` file in the `main` branch on GitHub and ensure that the build starts automatically in Jenkins and the file is saved in the `/var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/` folder on `JAN001`

    ![Alt text](Images/jen-server36.png)

    ![Alt text](Images/jen-server35-1.png)

    ![Alt text](Images/jen-server37.png)

    ![Alt text](Images/jen-server38.png)

### Preparing a Development Environment using Visual Studio Code

Visual Studio Code is an integrated development environment (IDE) or Source Code Editor for writing and debugging code. To prepare our development environment using VS Code, we need to first install VS Code, then follow these steps:

**Step 1: Install `Remote Development` extension in VS Code**

- In VS Code, go to `Extensions` and search for `Remote Development`, then install it

![Alt text](Images/jen-server40.png)

- Clone the `ansible-config-mgt` repository from GitHub to JAN001 by running the command `git clone <ansible-config-mgt repo link>`

![Alt text](Images/jen-server39.png)

### Carry Out Ansible Development

To begin Ansible development, go through the following steps:

**Step 1: Clone the `ansible-config-mgt` repository from GitHub to VS Code**

- Click on `Clone Repository` in VS Code. Then click on the `Clone from GitHub` drop down that shows up just under the search box at the top of VS Code

![Alt text](Images/jen-server41.png)

- Choose the repository you want to clone from the list

![Alt text](Images/jen-server42.png)

- Choose the repository location from the pop-up box

![Alt text](Images/jen-server43.png)

- The repository is cloned, and the system asks if you want to open it. Click Open

![Alt text](Images/jen-server44.png)

![Alt text](Images/jen-server45.png)

**Step 2: Create a new branch that would be used to develop new features**

- Go to the terminal on VS Code, and run the `git branch` command to confirm the current branch.

![Alt text](Images/jen-server46.png)

- Run `git status` to check the status of the current branch

![Alt text](Images/jen-server47.png)

- Run `git checkout -b <new branch name>` to create a new branch

![Alt text](Images/jen-server48.png)

- Create a directory `playbooks` in the new branch by running the `mkdir playbooks` command. The directory will be used to store the playbook files.

![Alt text](Images/jen-server49.png)

- Create a directory `inventory` to organise the hosts, by running the `mkdir inventory` command

![Alt text](Images/jen-server51.png)

![Alt text](Images/jen-server50.png)

![Alt text](Images/jen-server52.png)

- Create a playbook named `common.yml` inside the `playbooks` folder

![Alt text](Images/jen-server53.png)

- Create an inventory file for each environment (Development, Staging, Testing, and Production) named `dev`, `staging`, `uat`, and `prod`

![Alt text](Images/jen-server54.png)











    


    























 