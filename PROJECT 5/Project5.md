
# Documentation for the Shell Scripting Hands-On Project

A shell script is a text file that contains a succession of UNIX-based operating system commands. It is known as a shell script because it combines a succession of commands that would normally have been typed into a keyboard one at a time, into a single-file script.

The Shell of the UNIX-based operating system functions as its Command Line Interface (CLI) and interpreter for the commands that are used to communicate with the system.

For a DevOps Engineer, shell scripting is particularly important because it helps automate repititive tasks. Shell scripts are created in a text file with a `.sh` extension and are executed directly on the command line or called from other scripts.

## Introduction to Shell Scripting and User Input

Shell scripting uses the same syntax in a script as it would on the command line. Let's look at some elements of a shell scripting syntax.

### Shell Scripting Syntax Elements

1. **Variables**: Variables store different types of data such as strings, numbers, and arrays. Values are usually assigned to variables using the `=` operator. The value in a variable can be accessed by using the variable name preceded by the `$` sign.

To define a variable `name` and assign a value `David` to the variable using the command `name=David`:

![Alt text](Images/assign_variable.png)

To retrieve the value from the variable run the command `echo $name`:

![Alt text](Images/retrieve_variable_value.png)

2. **Control Flow**: Control flow statements like `while loops`, `for loops`, `case`, and `if-else` statements are used to control the flow of execution in a script. With these statements, we can make iterations over lists, make decisions, and execute commands based on conditions.

a. **if-else**

Example: Using if-else to execute a script based on conditions, the script is as follows:
```
#!/bin/bash

######################################################################
#Author: Dolapo
#Date: 18-10-2023
#Purpose: Script to check age and whether users are eligible to drive
######################################################################

read -p "Please input your age: " input_age

if [ $input_age -lt 18 ];

then 

echo "You are too young to drive"

elif [ $input_age -ge 18 ] && [ $input_age -le 80 ];

then

echo "You are eligible to drive"

else

echo "You are too old to drive"

fi
```
![Alt text](Images/if-else_script.png)

Running the if-else script in the CLI:

![Alt text](Images/if-else_results.png)

b. **for loop**

Example: Iterating through a list using the `for loop`:

![Alt text](Images/for-loop_script.png)

Running the for-loop script in the CLI:

![Alt text](Images/for-loop_result.png)

c. **command substitution**

This allows us to capture the output of a command and use it as a value in our script. 

![Alt text](Images/command_substitution_script.png)

![Alt text](Images/command_substitution_result.png)

d. **input and output**

Bash can handle input and output. The read command is used to accept user input, while the echo command is used to print output to console.

Script to *accept user input*

![Alt text](Images/accept-input_script.png)

![Alt text](Images/accept-input_result.png)

Script to *output text to terminal*

![Alt text](Images/echo_script.png)

![Alt text](Images/echo_result.png)

Script to *output the result of a command into a file*

![Alt text](Images/output_tofile_script.png)

![Alt text](Images/output_tofile_result.png)

Script to *pass the content of a file as input to a command*

The content of the input file is shown below:

![Alt text](Images/file_input.png)

Here's the result from running the command

![Alt text](Images/file_input_result.png)

Script to *pass the result of a command as input to another command*

![Alt text](Images/pass_command_script.png)

Running the command:

![Alt text](Images/pass_command.png)

e. **Functions**

With Functions, we can modularize our codes and make them more reusable. Functions can also be used to group unrelated commands together. To define a function, use the `function` keyword or declare the function name followed by parentheses.

Here's a function script

![Alt text](Images/function_script.png)

Result of running the script

![Alt text](Images/function_result.png)

**1. Script to prompt for name and say hello**

![Alt text](Images/user-input_script.png)

Running the script in the terminal:

![Alt text](Images/user-input_result.png)

## Directory Manipulation and Navigation

To show directory manipulation and navigation in the Linux file system:

- write a script to display the current directory
- create a new directory called "my_directory"
- change to the new directory and create two files inside the directory
- list the files in the directory, move back one level up, and remove "my_directory" and its contents
- list the files in the current directory

The script is detailed below:

![Alt text](Images/directory_manipulation-script.png)
![Alt text](Images/directory_manipulation-script2.png)

Running the script in the terminal:

![Alt text](Images/directory_manipulation-result.png)

## File Operations and Sorting

To implement File Operations and Sorting:

- write a script to create 3 files
- display the files in their current order
- sort them alphabetically and save the sorted files as sorted_files.txt
- display the sorted files and remove the original files
- rename the sorted files to sorted_files_sorted_alphabetically.txt
- display the contents of the final sorted file

Here's the script showing file operations and sorting:

![Alt text](Images/file_operations-script1.png)
![Alt text](Images/file_operations-script2.png)

Running the script gives the following results:

![Alt text](Images/file_operations-results.png)

## Working with Numbers and Calculations

To work with numbers and calculations:

- write a script to perform arithmetic operations (addition, subtraction, multiplication, modulus, and division)
- display the results
- perform complex calculations with the results displayed

The script showing the calculations:

![Alt text](Images/calculations-scripts1.png)
![Alt text](Images/calculations-scripts2.png)

The results from running the script:

![Alt text](Images/calculations-results.png)

## File Backup and Timestamping

As a DevOps Engineer, having backups of important data is very important. 

For this script, we'll:

- define source directory and backup directory paths
- create a timestamp using current date and time
- create a backup directory with the timestamp appended to its name
- the script should then copy everything from the source directory to the backup directory using the `cp -r` command for a recursive copy.
- the script should dispay a message indicating that the backup has been completed successfully
- the script should show the path of the backup directory with the timestamp

The backup script is shown below:

![Alt text](Images/backup-script.png)

The result from running the script

![Alt text](Images/backup-result1.png)

Checking the contents of the backup directory

![Alt text](Images/backup-result2.png)

Checking the backup directory shows backups done with different timestamps

![Alt text](Images/backup-result3.png)




















