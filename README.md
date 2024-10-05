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

   ![image 1](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%201.jpg)

In summary, client-server architecture is a way of organizing how different parts of a computer system communicate and work together. It separates the user interface (client) from data storage and processing (server), making it easier to manage applications, resources, and users. This architecture is widely used in various applications, including web services, email, and file sharing.


## Involving a Database Server

If we extend the Client-Server concept further and add a Database Server to our architecture, we can get this picture:

   ![image 2](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%202.jpg)

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

   ![image 3](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%203.jpg)

5. Connect to both instance via SSH on different terminal windows

   `ssh -i your-key.pem ec2-user@<public-ip-address-of-ec2>`
   

   ![image 4](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%204.jpg)

   This is for `mysql-server` instance

   ![image 5](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%205.jpg)

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

   ![image 6](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%206.jpg)

#### Repeat the whole process of MySQL installation on `mysql-client`


## Step 3 - Allow acces to MySQL-SERVER from MySQL-CLIENT 

1. Configure Security group: By default, MySQL uses TCP port 3306. To allow mysql-client to access mysql-server, we need to open port 3306.

2. Go to AWS Management Console.

3. Navigate to EC2 > Security Groups.

4. Find the security group attached to mysql-server.

5. Edit the Inbound rules:

      Protocol: TCP
      Port Range: 3306
      Source: The private IP address of your `mysql-client` instance.

6. Save the settings.

   ![image 7](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%207.jpg)
      


## Step 4 - Edit MySQL Configuration to Allow Remote Connections

1. On mysql-server, open the MySQL configuration file and allow connections from remote hosts:

   `sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf`
   

2. Change the bind-address from initial ip to allow from anywhere 0.0.0.0

   ![image 8](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%208.png)

3. Restart MySQL:

   `sudo systemctl restart mysql`


## Step 5 - Connect `mysql-client` to `mysql-server` remotely

1. Get `mysql-server` ip by:

   `hostname -I`

2. Go to `mysql-client` terminal window and connectc to `mysql-server` remotely

   `mysql -u root -p -h <private-ip-of-mysql-server>`

   When prompted, enter the root password you set during MySQL installation on the `mysql-server`


3. When prompted, enter the password for `mysql-server`

4. Test the connection by running:

   `SHOW DATABASES;`

   ![image 9](https://github.com/Captnfresh/Client-Server-Architecture/blob/main/Client-Server%20Architecture/image%209.jpg)


Congratulations! Your Client-Server Architecture is created, we have successfully deployed a fully functional MySQL Client-Server Architecture on AWS.


## Important to note:

You might get errors when you run the command `mysql -u root -p -h <private-ip-of-mysql-server>`. Do not panic, I will show you possible ways of fixing that issue.

### 1. Create a user first:

`CREATE USER 'root'@'%' IDENTIFIED BY 'password';`

Where % is the user or IP address you want to use
      password is the pasword created during mysql installation at the root user `mysql-server`


### 2. Grant Privileges to the User:

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;`

Where % is the user or IP address you want to use


### 3. Flush Privileges:

`FLUSH PRIVILEGES;`

This will apply the changes instantly.

### After all of this is done, you can proceed to Step 5 with no errors                                          





























  
