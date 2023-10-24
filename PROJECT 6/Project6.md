
# Documentation for Understanding Client-Server Architecture with MYSQL as RDBMS

Client-Server architecture refers to a model where two or more computers are connected together over a network to send and receive requests (information or data) between one another. 

During communications, each machine in a Client-Server architecture has its own role with the machine sending requests known as the "Client" and the machine responding to those requests (serving the requests) is known as the "Server".

## Understanding Client-Server Architecture

Basically, in a client-server architecture, the client sends a GET request for data, while the server accepts and serves the request via a GET response, sending the data back in the form of packets to the user who has requested them.

Here's a diagram representing a Client-Server architecture:

![Alt text](Images/client-server_image.png)

In the diagram above, the client tries to access a website through a web browser by sending a http request to the web server (with Apache, Nginx, IIS, etc. installed) over the internet. The web server responds by serving the website back to the client.

The above architecture can be enhanced with the addition of a database server. That gives us the architecture shown below:

![Alt text](Images/client-server-database_image.png)

### Characteristics of a Client-Server Architecture







