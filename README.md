# Project Documentation: Deployment of LEMP Stack on EC2 Instance

## Overview
This repository contains documentation for setting up a LEMP (Linux, Nginx, MySQL, PHP) stack on an Amazon EC2 instance. The LEMP stack is a robust and popular web development environment that provides all the necessary components to run dynamic websites and web applications efficiently.

### Stack Components
Our setup utilizes the following technologies:

- **Linux:** Ubuntu Server 24.04 LTS
- **Engine-X (Nginx):** High-performance HTTP server
- **MySQL:** Reliable relational database management system
- **PHP:** Versatile server-side scripting language

This combination offers a powerful, scalable, and flexible platform for web development and deployment.

## Setting up the EC2 Instance

1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Create a key pair
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 24.04 LTS as the operating system.
![key pair](images/key%20pair.png)     ![launch instance](images/launch%20instance.png)

2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.
![launch instance1](images/launch%20instance1.png)


3. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address
![inbound rule](images/inbound%20rule.png)

4. **Review and Launch**:
 - Review the configuration and launch the instance.
![review instance](images/review%20instance.png)


5. **Connect to the Instance**:
   - Use Git Bash to connect to the instance via SSH.
![ssh client](images/ssh%20client.png)

8. **Grant Permission for the private key and SSH to the instance**
  - We then launch bash and go to the directory where our `.pem` file is.

```bash
$ cd downloads
```

Next, we run these commands to make sure our server is up and running

```
chmod 400 my-ec2-key.pem
ssh -i "my-ec2-key.pem" ubuntu@18.209.18.61
```
![git bash](images/git%20bash.png)

## Installing the Nginx Web Server

1. **Update Package Repository**:
   ```bash
   sudo apt update
   ```
![git bash1](images/git%20bash1.png)

2.**Install Nginx**
   ```bash
sudo apt install Nginx -y
```
![git bash2](images/git%20bash2.png)

3.**Enable and verify that Nginx is running on as a service on the OS**:
```bash
sudo systemctl status Nginx
```
If it green and running, then it is correctly installed
![git bash3](images/git%20bash3.png)

__4.The server is running and can be accessed locally in the ubuntu shell by running the command below:__

```
curl http://localhost:80
OR
curl http://127.0.0.1:80
```
![git bash4](images/git%20bash4.png)

5.**Test how Nginx can respond to request over the internet**
```
http://<public-ip address>:80
```
![nginx server](images/nginx%20server.png)

__6.__ __Another way to retrieve the public ip address other than check the aws console__

```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```

## Installing Mysql
1. **mysql is popular relational database**:
   ```bash
   sudo apt install mysql-server
   ```
![mysql](images/mysql.png)

2.**log into mysql**:
   ```bash
   sudo mysql
   ```

3.**running a security script to set password for the root user using  mysql_native_password***:
   ```
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
   ```

Exit the MySQL shell
```
exit
```
![mysql2](images/mysql2.png)


