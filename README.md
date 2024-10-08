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

4.**Start the interactive script by running:**
```
sudo mysql_secure_installation
```
![mysql3](images/mysql3.png)

__5.__ __After changing root user password, log in to MySQL console.__

A command prompt for password was noticed after running the command below.
```
sudo mysql -p
```
Exit MySQL shell
```
exit
```
![mysql4](images/mysql4.png)

## Installing PHP
**To Install php, run**:

```bash
sudo apt install php-fpm php-mysql
```
![php](images/php.png)

## Configuring Nginx to use PHP processor

**Create a root web directory for your_domain:**

```bash
sudo mkdir /var/www/projectLEMP
```

**Next, we assign ownership of the directory with the $USER environment variable, which will reference our current system user:**

```bash
sudo chown -R $USER:USER /var/www/projectLEMP
```
![php1](images/php1.png)

Then, we open a new configuration file in nginx's `sites-available` using nano:

```bash
sudo nano /etc/nginx/sites-available/projectLEMP
```

This will create a new blank file. Paste in the following bare-bones configuration:

```bash
#/etc/nginx/sites-available/projectLEMP

server {
   listen 80;
   server_name projectLEMP www.projectLEMP;
   root /var/www/projectLEMP;

   index index.html index.htm index.php;

   location / {
       try_files $uri $uri/ =404;
   }

   location ~ \.php$ {
       include snippets/fastcgi-php.conf:
       fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
   }

   location ~ /\.ht {
       deny all;
   }

}
```
![php3](images/php3.png)  ![php2](images/php2.png)

### Here’s what each directives and location blocks does:

- __listen__ - Defines what port nginx listens on. In this case it will listen on port 80, the default port for HTTP.

- __root__ - Defines the document root where the files served by this website are stored.

- __index__ - Defines in which order Nginx will prioritize the index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page for PHP applications. You can adjust these settings to better suit your application needs.

- __server_name__ - Defines which domain name and/or IP addresses the server block should respond for. Point this directive to your domain name or public IP address.

- __location /__ - The first location block includes the try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate result, it will return a 404 error.

- __location ~ \.php$__ - This location handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

- __location ~ /\.ht__ - The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root, they will not be served to visitors.

then we save and close the file.  
Next, we activate our configuration by linking the config file from nginx's `sites-enabled` directory.

```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```

This will tell nginx to use the configuration next time it is reloaded.  
We test our configuration for syntax eroors by:

```bash
sudo nginx -t
```

It should return:

```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is okay
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
![php3](images/php3.png)

If any errors are reported, go back to your configuration file to review its contents before continuing.

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

```bash
sudo unlink /etc/nginx/sites-enabled/default
```


When ready, we reload nginx and apply changes made.

```bash
sudo systemctl reload nginx
```


Our website is now active! We then create an index.html file in the location so that we can test that our new server block works as expected:

```bash
sudo echo ‘Hello LEMP from hostname’ $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) ‘with public IP’ $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
![php4](images/php4.png)

Now go to your browser and try to open your website URL using IP address:
```
http://<Public-IP-Address>:80
```
![Hello lemp](images/Hello%20lemp.png)

You can also access your website in your browser by public DNS name, not only by IP - try it out, the result must be the same (port is optional):
```
http://<Public-DNS-Name>:80
```
![hello lemp1](images/hello%20lemp1.png)

## Testing PHP with Nginx

**1. Create a test PHP file in the document root. Open a new file called info.php within the document root.**
```bash
nano /var/www/projectLEMP/info.php
```

paste the code below:
```bash
<?php
phpinfo();
```

**2. We can now access this page in our web browser by visiting the domain name followed by `/info.php:`**
```bash
http://34.238.232.0/info.php
```

After checking the details about PHP, it is best to remove the file as it contain sensitive information:
```bash
sudo rm /var/www/projectLEMP/info.php
```
![php5](images/php5.png)   ![php6](images/php6.png)

# Retrieving Data from MySQL Database with PHP
1.**connect to MYSQL database** 
```bash
sudo mysql
```

2.**create a new Database**
```bash
mysql> CREATE DATABASE `example_database`;
```

3.**create a user and grant the user full priviledge with password**
```bash
mysql>  CREATE USER 'my_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.2';

mysql> GRANT ALL ON example_database.* TO 'my_user'@'%';
```

4.**Now exit the MySQL shell with:**
```bash
mysql> exit
```

5. **check if the user has proper permission by logging**
```bash
$ mysql -u my_user -p
```

6.**After logging in to the MySQL console, confirm that you have access to the example_database database:**
```bash
mysql> SHOW DATABASES;
```

7.**Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:**
```bash
CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
```
![database3](images/database3.png)

8.**Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:**
```bash
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My second important item");
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My third important item");
```

9.**To confirm that the data was successfully saved to your table, run:**
```bash
mysql>  SELECT * FROM example_database.todo_list;
```
![database4](images/database4.png)

10.**After confirming that you have valid data in your test table, you can exit the MySQL console:**
```bash
mysql> exit
```

11.**Now we create a PHP script that will connect to MySQL and query for our content.**
```bash
nano /var/www/projectLEMP/todo_list.php
```

12.**Copy this content into your todo_list.php script:**
```bash
<?php
$user = "example_user";
$password = "PassWord.1";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```
**save and close**
![database6](images/database6.png)

**You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:**
```bash
http://34.238.232.0/todo_list.php
```
![todo_list](images/todo%20list.png)

## conclusion
we have successfully set up a LEMP stack on an Amazon EC2 instance using GIT Bash for remote server access. You can now deploy and host your web applications on this environment.

