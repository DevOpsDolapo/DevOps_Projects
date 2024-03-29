
## Implementing Ansible Refactoring and Static Assignments

Code Refactoring means making changes to the source code without changing expected behaviour of the software. The main thrust of refactoring is to improve code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic. 

In this project, we'll move things around a little bit in the code, but the overall state of the infrastructure remains the same.

Our architecture will be running on the same infrastructure components as Project 11, but we'll be configuring two (2) more RHEL 8 servers as our UAT Webservers:

1. **Infrastructure**: Oracle VM VirtualBox

2. **Jenkins/Ansible Server**: Ubuntu 20.04 + Jenkins + Ansible

3. **Web Servers**: Red Hat Enterprise Linux 8

4. **Database Server**: Ubuntu 20.04 + MySQL

5. **Storage (NFS) Server**: Red Hat Enterprise Linux 8 + NFS Server

6. **Load Balancer**: Ubuntu 20.04 + Nginx

7. **GitHub**: For creating our repository

8. **Visual Studio Code**: For preparing our development environment

9. **UAT Web Servers**: Red Hat Enterprise Linux 8

### Improving Jenkins Pipeline To make it Better
### Improving Jenkins Pipeline To make it Better

In our current Jenkins setup, every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, each subsequent change consumes space on the Jenkins server. We'll improve this setup by using the `Copy Artifact` plugin.

**Step 1: Create a new directory called `ansible-config-artifact` in the Jenkins/Ansible server (JAN001) where we will store all artifacts after every build**

- Run the command `sudo mkdir /home/vboxuser/ansible-config-artifact` to create the directory

![Alt text](Images/refac1.png)

- Change permissions to this directory, so Jenkins can save files there by running the command `sudo chmod -R 0777 /home/vboxuser/ansible config-artifact`

![Alt text](Images/refac2.png)

- On the Jenkins web console, go to -> Manage Jenkins -> Plugins -> Available Plugins, then search for `Copy Artifact` and install this plugin without restarting Jenkins

![Alt text](Images/refac3.png)

![Alt text](Images/refac4.png)

![Alt text](Images/refac5.png)

![Alt text](Images/refac6.png)

![Alt text](Images/refac7.png)

- Create a new Freestyle project and name it `save_artifacts`

![Alt text](Images/refac8.png)

- Apply the appropriate settings, then save

![Alt text](Images/refac9.png)

![Alt text](Images/refac10.png)

**Step 2: The main idea of `save_artifacts` project is to save artifacts into the `/home/vboxuser/ansible-config-artifact` directory. To achieve this, we need to create a Build step and choose `Copy artifacts from other project`, specify `ansible` as the source project and `/home/vboxuser/ansible-config-artifact` as the target directory**

![Alt text](Images/refac11.png)

![Alt text](Images/refac12.png)

- Click on `Apply` then `Save`

![Alt text](Images/refac13.png)

**Step 3: Run a test by editing the `README` file for `ansible-config-mgt` repository. Both Jenkins jobs should complete one after the other**

- The initial state of both Jenkins jobs is shown below

![Alt text](Images/refac14.png)

![Alt text](Images/refac15.png)

![Alt text](Images/refac16.png)

![Alt text](Images/refac17.png)

![Alt text](Images/refac18.png)

![Alt text](Images/refac19.png)

![Alt text](Images/refac20.png)

### Refactor Ansible Code by Importing other Playbooks

**Step 1: Pull down the code from the `main branch` into a new branch named `refactor`**

- Run the command `git checkout -b refactor` to create the new branch

![Alt text](Images/refac21.png)

- Within the `playbooks` folder, create a new file named `site.yml`, which is going to be an entry point into the entire infrastructure configuration. In other words, `site.yml` will become a parent to all other playbooks that will be developed, including `common.yml`

![Alt text](Images/refac22.png)

- Create a new folder in root of the repository named `static-assignments` . The `static-assignments` folder is where all other children playbooks will be stored.

![Alt text](Images/refac23.png)

- Move `common.yml` file into the newly created `static-assignments` folder

![Alt text](Images/refac24.png)

- Inside the `site.yml` file, import `common.yml` playbook

![Alt text](Images/refac25.png)

- The current folder structure:

![Alt text](Images/refac26.png)

- Since we need to apply some tasks to the `dev` servers and `wireshark` is already installed, we can create another playbook under `static-assignments` and name it `common-del.yml` . 

![Alt text](Images/refac27.png)

- In the `common-del.yml` playbook, we'll configure deletion of the wireshark utility, by adding the block of code below

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: vboxuser
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    yum:
      name: wireshark
      state: removed

- name: update LB server
  hosts: lb
  remote_user: vboxuser
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    apt:
      name: wireshark-qt
      state: absent
      autoremove: yes
      purge: yes
      autoclean: yes

```
![Alt text](Images/refac28.png)

- Update `site.yml` with - `import_playbook: ../static-assignments/common-del.yml` instead of `common.yml`

![Alt text](Images/refac29.png)

- Save and push all changes

![Alt text](Images/refac30.png)

- Run `site.yml`against the `dev` servers by running the code

```
cd /home/ubuntu/ansible-config-mgt/

ansible-playbook -i inventory/dev.yml playbooks/site.yml
```
![Alt text](Images/refac32.png)

- Make sure that wireshark is deleted on all the servers by running wireshark --version

![Alt text](Images/refac33.png)

![Alt text](Images/refac34.png)

![Alt text](Images/refac35.png)

![Alt text](Images/refac36.png)

![Alt text](Images/refac37.png)

### Configure UAT Webservers Using Roles

We'll configure two (2) new Web Servers as UAT using a dedicated role to make our configuration reusable. Since I'm using Oracle VM VirtualBox for this project. I've created two new VirtualBox VMs as our UAT WebServers `(WAT001 and WAT002)`

**Step 1: Create a `roles` directory**

- Create the directory in the `ansible-config-mgt` repository with the structure below

```
└── webserver
    ├── README.md
    ├── defaults
    │   └── main.yml
    ├── handlers
    │   └── main.yml
    ├── meta
    │   └── main.yml
    ├── tasks
    │   └── main.yml
    └── templates
```
![Alt text](Images/refac38.png)

![Alt text](Images/refac39.png)

- Update the `ansible-config-mgt/inventory/uat.yml` file with the IP addresses of the UAT Webservers

![Alt text](Images/refac40.png)

- Uncomment the roles_path in the `/etc/ansible/ansible.cfg` file, and provide a full path to your roles directory using `roles_path = /home/vboxuser/ansible-config-mgt/roles`, so Ansible knows where to find configured roles

![Alt text](Images/refac41.png)

**Step 2: Write configuration tasks for the webserver role**

- Go into the `tasks` directory under `roles` in `ansible-config-mgt` and edit the `main.yml` file with configuration tasks to do the following:

  - Install and configure Apache (httpd service)
  - Clone the Tooling website from GitHub https://github.com/DevOpsDolapo/tooling.git
  - Ensure the tooling website code is deployed to /var/www/html on each of the two (2) UAT Webservers (WAT001 and WAT002)
  - Make sure httpd service is started

- The block of code to achieve the above tasks is

```
---
- name: install apache
  become: true
  ansible.builtin.yum:
    name: "httpd"
    state: present

- name: install git
  become: true
  ansible.builtin.yum:
    name: "git"
    state: present

- name: clone a repo
  become: true
  ansible.builtin.git:
    repo: https://github.com/DevOpsDolapo/tooling.git
    dest: /var/www/html
    force: yes

- name: copy html content to one level up
  become: true
  command: cp -r /var/www/html/html/ /var/www/

- name: Start service httpd, if not started
  become: true
  ansible.builtin.service:
    name: httpd
    state: started

- name: recursively remove /var/www/html/html/ directory
  become: true
  ansible.builtin.file:
    path: /var/www/html/html
    state: absent
```
![Alt text](Images/refac42.png)

### Reference the Webserver Role

**Step 1: In the `static-assignments` folder, create a new assignment for `uat-webservers` called `uat-webservers.yml`. This is where we will reference the role**

- Run the code block below to reference the role

```
---
- hosts: uat-webservers
  roles:
     - webserver
```
![Alt text](Images/refac43.png)

- Refer the `uat-webservers.yml` role inside `site.yml`

![Alt text](Images/refac44.png)

### Commit and Test

**Step 1: Commit the changes, create a Pull Request and merge them to the main branch, ensure the webhook triggers two consequent Jenkins jobs, they ran successfully and copied all the files to the Jenkins-Ansible server into /home/vboxuser/ansible-config-mgt/ directory**

![Alt text](Images/refac45.png)

![Alt text](Images/refac46.png)

![Alt text](Images/refac47.png)

- Create a Pull Request for `Refactor` on GitHub

![Alt text](Images/refac48.png)

![Alt text](Images/refac49.png)

- Check on Jenkins to see if the jobs were triggered

![Alt text](Images/refac50.png)

![Alt text](Images/refac51.png)

- Checkout from the current branch into the main branch and pull down all the changes into the main branch

![Alt text](Images/refac52.png)

**Step 2: Now run the playbook against the uat inventory**

- Run the command `ansible-playbook -i inventory/uat.yml playbooks/site.yml` on the remote SSH connection to the Jenkins-Ansible server on VS Code

![Alt text](Images/refac53.png)

![Alt text](Images/refac54.png)

- Both the UAT Webservers are now configured and we can reach them from the browser using `http://<Web-UAT-Server-Public-IP-or-Public-DNS-Name>/index.php`

![Alt text](Images/refac55.png)

![Alt text](Images/refac56.png)

- The final Ansible architecture looks something like this

![Alt text](Images/ansible-refactoring.drawio.png)












    

    

