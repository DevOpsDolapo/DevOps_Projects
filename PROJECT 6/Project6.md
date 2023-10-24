
# Documentation for Understanding Client-Server Architecture with MYSQL as RDBMS

Client-Server architecture refers to a model where two or more computers are connected together over a network to send and receive requests (information or data) between one another. 

During communications, each machine in a Client-Server architecture has its own role with the machine sending requests known as the "Client" and the machine responding to those requests (serving the requests) is known as the "Server".

## Understanding Client-Server Architecture

Basically, in a client-server architecture, the client sends a GET request for data, while the server accepts and serves the request via a GET response, sending the data back in the form of packets to the user who has requested them.

Here's a diagram representing a Client-Server architecture:

![Alt text](Images/client-server_image.png)

In the diagram above, the client tries to access a website through a web browser by sending a http request to the web server (with Apache, Nginx, or IIS, etc. installed) over the internet. The web server responds by serving the website back to the client.

The above architecture can be enhanced with the addition of a database server. That gives us the architecture shown below:

![Alt text](Images/client-server-database_image.png)

In the diagram above, the web server acts as a "client" that reads and writes data to/from a Database server (MySQL, MongoDB, or SQL server, etc.) through a local network before serving the client that has requested the data through an internet connection.

In simple terms, if we go on a browser and type in `www.propitixhomes.com`, the browser is considered as the "client" that sends a request to the remote server which in turn responds with the requested website. A demonstration of the Client-server communication is shown below by using the `curl -Iv www.propitixhomes.com` command in a linux terminal.

The `curl` package is not installed, so that needs to be installed first.

![Alt text](Images/curl-install.png)

The result of running the `curl` command to the propitixhomes web page is shown below:

![Alt text](Images/curl-command.png)

In the above example, the linux terminal is the client while the www.propitixhomes.com is the server. As seen from the request result, the ip address of the computer functioning as the server is `75.2.115.196` on port `80`. The server is also running on the `nginx` web server software.

### Characteristics of a Client-Server Architecture







