# Documentation for the Linux Practice Projects

## This project aims to help the student gain valuable experience working with Linux commands in the Linux terminal

### Task 1 - sudo 

The **sudo** command lets you perform tasks requiring administrative or root permissions.

Command: **sudo apt upgrade**

Being the first time running the **sudo apt upgrade** command on the Linux machine, it upgraded the operating system packages and libraries.

*Below is a screenshot showing the command and the start of the upgrade process:*
![Task1 - showing the sudo apt upgrade command and its execution](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/89ba5672-4fd0-4a8f-9f9b-c90722eebd3e)

### Task 2 - pwd (present working directory)

The **pwd** command shows the full path of a user's current working directory.

*The picture below shows the outcome of running the command when a user is in the Desktop directory:*
![Task 2 - showing pwd command in desktop directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/dfef6c4e-fda4-411c-ab91-d749d61c4039)

*The picture below shows the outcome of running **pwd** in the Documents directory:*

![Task 2 - showing pwd command in documents directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/b74bc6f0-6e7a-4de8-be2d-f972562c6990)

### Task 3 - cd (change directory)

The **cd** command allows you to navigate through linux directories (folders)

The pictures below show the results of running the cd command to switch between different directories

*The outcome of changing directory to the commandslinux directory:*

![Task 3 - changing directory to the commandslinux directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/3e2fde76-ef48-46d5-9f3e-96c865a37ed9)

*Executing the **cd** command to move back to previous directory:*

![Task 3 - command to move back to previous directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/aedfa78c-b5a1-42e6-b008-20cab2285243)

*Executing the **cd** command to move one directory up:*

![Task 3 - command to move one directory up](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/875f7c25-c893-4e77-8c4b-67e8c467842b)

### Task 4 - ls (list)

The ls command is used to list files and directories within the system

*The **ls** command listing files and directories in the Documents directory:*

![Task 4 - ls command lists files and directories in the Documents directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/eeedb622-e288-482c-bc71-237dbfc676ac)

*The **ls** command lists files and directories in the home directory of vboxuser:*
![Task 4 - ls command lists files and directories in the home directory of vboxuser](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/96e88db3-642b-4fae-9326-fd1c4854fce7)

The **ls** command can also be used with flags to specify certain options and outcomes from our results.

*The **ls -a** (using the -a flag) command allows us to see visible and hidden files/folders in any directory. In Linux, hiddens files and folders are denoted by a .(dot) preceeding the file or folder name:*
![Task 4 - ls -a command shows visible and hidden files](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/73b1029a-ecce-4179-9753-d3452d9441d4)

*The **ls -lh** (using the -lh flag) command shows files and directories listed in a human-readable format along with their sizes:*
![Task 4 - ls -lh command shows files and directories listed in a human-readable format with their sizes](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/6f395cec-faa3-4ef0-9e7a-ae6a63a1a4bf)

### Task 5 - cat (concatenate)

The **cat** command lists, merges, and writes out the content of a file

*The **cat** command is used to list the content of three different files (file, file1, and file2):*
![Task5 - the cat command lists the content of three different files (file, file1, and file2)](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/da05d974-62aa-4ead-a2ee-3ece68b44b58)

*The **tac** command displays the content of file2 in reverse order:*

![Task5 - the cat command displays the content of file2 in reverse order](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/52a84e45-cd13-4667-9918-0ad7ef7bff4c)

*Below, the **cat** command is used to merge the contents of file, file1, and file2 and store the output in file3:*
![Task5 - the cat command is used to merge the contents of file, file1, and file2](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/26ce1d4c-52b1-4056-b778-2817000b2dca)

### Task 6 - cp (copy)

The **cp** command is used to copy files and directories along with their contents

*The **cp** command used to copy a file to the test_directory folder on the Desktop:*
![Task6 - the cp command used to copy a file to a folder in the Desktop directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/7f727bbc-b68f-45e0-96e3-ec5f616d2422)

*The **cp** command used to copy files to the Desktop:*

![Task6 - the cp command used to copy files to the Desktop directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/8fb0adb3-941f-4087-ae7e-73430e7a14ed)

*The **cp** command is also used to copy the contents of one file to another file as shown below:*
![Task6 - the cp command used to copy the contents of one file to another file](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/283b0b01-ed0a-4702-b208-a851488e22df)

*With the -R (recursive) flag, the **cp** command is used to copy an entire directory or the contents of a directory to another directory. In this case, we're copying the contents of Desktop directory to the test_folder directory in Documents:*
![Task6 - using cp with the -R flag](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/d87833a3-252a-4cfd-b574-b55822ee9abc)

*Confirming the contents of the Desktop directory:*

![Task6 - confirming contents of the Desktop directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/73143cb6-11f6-4f75-9e53-21ee0acaefe4)

### Task 7 - mv (move)

The **mv** command allows us to move and rename files and directories

*The **mv** command used to move files and directory from Desktop to the first_folder directory in Documents:*
![Task7 - mv command moves files and directory on Desktop to the first_folder directory in Documents](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/7a21ede5-30b1-4919-b744-adb151d2ee39)

*The **mv** command used to rename test_directory to renaming_directory:*
![Task7 - mv command to rename test_directory to renaming_directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/98bab244-7801-4dbd-b34e-2a910512a489)

*The **mv** command used to rename file3 to file_renamed:*
![Task7 - mv command to rename file3 to file_renamed](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/d18357c6-b91d-48aa-a2f3-4688b5329d14)

### Task 8 - mkdir (make directory)

The **mkdir** command is used to create one or more folders (directories). The command is also used to set permissions for the created directories at the same time. It is important that the relevant user has the necessary permissions to create entries in the parent directory or they would get a permission denied error.

***mkdir** command creates a directory august in the Desktop directory:*

![Task8 - mkdir command creates a directory august in the Desktop directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/4be890cf-fc62-4772-89cc-d800241f9b8d)

The **mkdir** command also works with flags. Some of the flags used with this command are as follows;

***mkdir -p** or -parents (allows a user to create a directory September2023 between two existing directories):*
![Task8 - mkdir -p command creates a directory september2023 in the Documents directory](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/80807bfa-6f9d-43f0-a860-a6085a97dfef)

***mkdir -m** (allows the user to set permissions on a directory at the time of creation). For instance, to create a directory Aug2023 with full read, write, execute permissions:*

![Task8 - mkdir -m command creates a directory Aug2023 with full permissions (777)](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/394343eb-1479-462c-8b85-b1a866c17332)

***mkdir -v** (the verbose flags prints a message when a directory is created):*

![Task8 - mkdir -v command creates a directory and prints a message](https://github.com/DevOpsDolapo/LinuxPracticeProjects/assets/115728279/80f9dcd0-796b-4d77-8d83-a02739654962)























