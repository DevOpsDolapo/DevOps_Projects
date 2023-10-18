
# Documentation for the Shell Scripting Hands-On Project

A shell script is a text file that contains a succession of UNIX-based operating system commands. It is known as a shell script because it combines a succession of commands that would normally have been typed into a keyboard one at a time, into a single-file script.

The Shell of the UNIX-based operating system functions as its Command Line Interface (CLI) and interpreter for the commands that are used to communicate with the system.

For a DevOps Engineer, shell scripting is particularly important because it helps automate repititive tasks. Shell scripts are created in a text file with a `.sh` extension and are executed directly on the command line or called from other scripts.

## Introduction to Shell Scripting and User Input

Shell scripting uses the same syntax in a script as it would on the command line. Let's look at some elements of a shell scripting syntax.

### Shell Scripting Syntax Elements

1. **Variables**: Variables store different types of data such as strings, numbers, and arrays. Values are usually assigned to variables using the `=` operator. The value in a variable can be accessed by using the variable name preceded by the `$` sign.

To define a variable `name` and assign a value `David` to the variable:

![Alt text](Images/assign_variable.png)

To retrieve value from the variable run the command `echo $name`:

![Alt text](Images/retrieve_variable_value.png)

2. **Control Flow**: Control flow statements like `while loops`, `for loops`, `case`, and `if-else` statements are used to control the flow of execution in a script. With these statements, we can make iterations over lists, make decisions, and execute commands based on decisions.







