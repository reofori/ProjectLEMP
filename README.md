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

And make sure our instance is up and running.
![review instance](images/review%20instance.png)

