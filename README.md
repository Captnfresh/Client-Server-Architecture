# Client-Server Architecture with MySQL


## What is Client-Server Architecture?


### 1. Basic Definition: 

In a client-server architecture, there are two main components: the client and the server. The client requests services or resources, while the server provides those services or resources.

### 2. How It Works:
   
A. Client: This is typically the device or application (like a web browser, mobile app, or desktop software) that the user interacts with. The client sends requests to the server for information or actions.

B. Server: This is a powerful computer or program that listens for requests from clients and responds to them. It stores data, runs applications, and performs tasks on behalf of the client.

### 3. Communication: 
The client and server communicate over a network (like the internet or a local area network). When the client sends a request, the server processes it and sends back a response. This is often done using specific protocols, like HTTP for web services.

### Example of Client-Server Architecture

Web Browsing: When you enter a URL in your web browser (the client), the browser sends a request to a web server (the server) to retrieve the website. The server processes this request, finds the appropriate webpage, and sends it back to your browser to display.

  image 1

In summary, client-server architecture is a way of organizing how different parts of a computer system communicate and work together. It separates the user interface (client) from data storage and processing (server), making it easier to manage applications, resources, and users. This architecture is widely used in various applications, including web services, email, and file sharing.


## Involving a Database Server

If we extend the Client-Server concept further and add a Database Server to our architecture, we can get this picture:

image 2

In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

The setup on the diagram above is a typical generic Web Stack architecture that we have already implemented in previous projects (LAMP, LEMP, MEAN, MERN), this architecture can be implemented with many other technologies - various Web and DB servers, from small Single-page applications SPA to large and complex portals.


## Objective:

Implement a MySQL-based client-server architecture using AWS EC2 Linux instances.


## Prerequisites:

1. AWS account with permission to create EC2 instances.

2. Basic knowledge of Linux commands and MySQL.


## Step 1 - Create and Configure Two Linux-Based EC2 Instances

1. Create two EC2 instances, one acting as the MySQL Server, and the other as the MySQL Client.

2. Go to your AWS Management Console.

3. Launch two new EC2 instances with an Ubuntu AMI.

4. You can name the instances as follow:
         EC2 Instance 1: mysql-server
         EC2 Instance 2: mysql-client

Ensure both instances are in the same VPC (Virtual Private Cloud) and subnet so they can communicate via private IP addresses.

For security purposes, generate different key pair for SSH access.

image 3

5. Connect to both instance via SSH on different terminal windows

   `ssh -i your-key.pem ec2-user@<public-ip-address-of-ec2>`
   

   image 4

   This is for `mysql-server` instance

   image 5

   This is for `mysql-client` instance


## Step 2 - Install MySQL

1. On the `mysql-server` instance, install MySQL Server:

   `sudo apt update`

   `audo apt install mysql-server -y`

2. Start MySQL:

   `sudo systemctl start mysql`

3. Enable MySQL:

   `sudo systemctl enable mysql`

4. Secure your database:

   `sudo mysql_secure_installation`

Follow the prompts to set root password and secure the database installation. 

image 6

#### Repeat the whole process of MySQL installation on `mysql-client`


## Step 3 - Allow acces to MySQL-SERVER from MySQL-CLIENT 

1. Configure Security group
   


   






























  
