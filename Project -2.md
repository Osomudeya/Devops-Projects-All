# WEB STACK IMPLEMENTATION (LEMP STACK)

## STEP 1 – INSTALLING THE NGINX WEB SERVER

**Steps**
1. To start, update server’s package index. Afterwards, use apt install to get the Nginx installation going. To start the update run: Sudo apt update -y

2. Next, install Nginx by running: sudo apt install nginx -y

3. At the prompt, enter Y to confirm that you want to install Nginx. This would complete the installation process.

4. To confirm that nginx was successfully installed and is running as a service in Ubuntu, run: sudo systemctl status nginx
5. Green indicator shows that the server was successfully install and running.
   <img width="673" alt="Screen Shot 2022-12-06 at 12 18 12 AM" src="https://user-images.githubusercontent.com/99102616/205764200-fd57bd6f-a02d-4c31-b430-fbeec95e6431.png">
  
6. The server is running and we can access it locally and from the Internet but to access it from the internet port 80 must be open to allow traffic from the internet in.

7. To test that the server can be accessed locally from the instance run the curl command: curl http://localhost:80 The output shows that the server is accessible from the local host.
8. To test that the server can be accessed from the internet, open a browser and type the following url with the public IP of your Ubuntu instance; http://Public-IP-Address:80
   <img width="1247" alt="Screen Shot 2022-12-06 at 12 37 51 AM" src="https://user-images.githubusercontent.com/99102616/205768491-f7a4a5be-b1f3-4ac7-ba1f-00dbfd459c4e.png">

## STEP 2 — INSTALLING MYSQL

**Steps** 
1. To acquire and install SQL run: sudo apt install mysql-server -y in the terminal.
2. Next, log into MySQL: sudo mysql
3. Run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. run the follwing command: ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
4. Exit SQL shell by typing exit
5. Start interactive scripting to configure the validate password pluggin. Run: sudo mysql_secure_installation and answer Y to all the prompts. At the point you can change the password of your root user and also decide the level of password validation.
6. When done, test to know if login to the console is possible. type: sudo mysql -p
7. This would prompt you to enter root user password. Enter the chosen password and enter.

8. Type exit to exit MySQL console.

## STEP 3 – INSTALLING PHP

**Steps**

1. PHP has to be installed to process code and generate dynamic content for the web server.
2. Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
3. To install the 2 packages at once, run: sudo apt install php-fpm php-mysql -y

## STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

**Steps**
1. Now that we have PHP components installed. Next, I will configure Nginx to use them.
2. Create the root web directory for your_domain as follows:sudo mkdir /var/www/projectLEMP
3. Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user: sudo chown -R $USER:$USER /var/www/projectLEMP
4. Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use vi: sudo vi /etc/nginx/sites-available/projectLEMP
5. This will create a new blank file. Paste in the following bare-bones configuration:
```
   server {
   listen 80;
   server_name projectLEMP www.projectLEMP;
   root /var/www/projectLEMP;

   index index.html index.htm index.php;

   location / {
   try_files $uri $uri/ =404;
   }

   location ~ \.php$ {
   include snippets/fastcgi-php.conf;
   fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
   }

   location ~ /\.ht {
   deny all;
   }
   }
```
6. In the nano editor, enter CTRL+X to exit and Y to confirm.
7. Activate the configuration by linking to the config file from Nginx’s sites-enabled directory, run: sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
8. Test your configuration for syntax errors by typing: sudo nginx -t
9. We need to disable default Nginx host that is currently configured to listen on port 80, for this run: sudo unlink /etc/nginx/sites-enabled/default
10. Next, reload Nginx to apply the changes: sudo systemctl reload nginx
11. The website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that the new server block works as expected: sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
12. Open a browser and try to open the website URL using IP address: http://Public-IP-Address:80

    <img width="1251" alt="Screen Shot 2022-12-06 at 1 18 35 AM" src="https://user-images.githubusercontent.com/99102616/205775035-dbcf90aa-bb56-48d9-ba70-94634852ed4d.png">

## STEP 5 – TESTING PHP WITH NGINX

**Steps**
1. Now that LAMP stack is completely installed and fully operational. We test it to validate that Nginx can correctly hand .php files off to your PHP processor.
2. Open a new file called info.php within your document root in your text editor: sudo vi /var/www/projectLEMP/info.php
3. Type the following lines into the new file.

```
<?php
phpinfo();

```

4. You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php: http://server_domain_or_IP/info.php
5. Remove the created file, as it contains sensitive information about your PHP environment and your Ubuntu server: sudo rm /var/www/your_domain/info.php

## STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)

**Steps**
1. The goal in this step is to create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.
2. Because the native MySQL PHP library mysqlnd currently doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.
3. I will be creating a database named example_database and a user named example_user.
4. Connect to the MySQL console using the root account: sudo mysql
5. To create a new database, run the following command from your MySQL console: CREATE DATABASE example_database;
6. Next, create a new user and grant him full privileges on the database. The following command creates a new user named example_user, using mysql_native_password as default authentication method. Run: CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord1@';
7. Next, give this user permission over the example_database database: GRANT ALL ON example_database. TO 'example_user'@'%';*
8. This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.
9. Exit the console: exit
10. Now test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials: mysql -u example_user -p
11. After logging in to the MySQL console, confirm that you have access to the example_database database: SHOW DATABASES;

```
mysql> SHOW DATABASES;
```

This will give you the following output:

```
Output
+--------------------+
| Database           |
+--------------------+
| example_database   |
| information_schema |
+--------------------+
2 rows in set (0.000 sec)

```


Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:

```
CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );

```

Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:

```
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");

```

To confirm that the data was successfully saved to your table, run:

```
mysql>  SELECT * FROM example_database.todo_list;

```

You’ll see the following output:


```
Output
+---------+--------------------------+
| item_id | content                  |
+---------+--------------------------+
|       1 | My first important item  |
|       2 | My second important item |
|       3 | My third important item  |
|       4 | and this one more thing  |
+---------+--------------------------+
4 rows in set (0.000 sec)

```

After confirming that you have valid data in your test table, you can exit the MySQL console:

```
mysql> exit

```

Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. 
We’ll use vi for that:


```
nano /var/www/projectLEMP/todo_list.php

```

The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. 
If there is a problem with the database connection, it will throw an exception.

Copy this content into your todo_list.php script:


```
<?php
$user = "example_user";
$password = "password";
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


Save and close the file when you are done editing.

You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:


```
http://<Public_domain_or_IP>/todo_list.php

```

You should see a page like this, showing the content you’ve inserted in your test table:


https://drive.google.com/uc?export=view&id=1qrYScyXP91v-s0hgrcBmw5WAv0eAIzEb

That means your PHP environment is ready to connect and interact with your MySQL server.

![todolist](https://user-images.githubusercontent.com/99102616/205860745-52e9dce9-fe0f-45f7-89d3-2a196c33701a.png)

Issues Faced
While everything seemed to on go smoothly during the project, I however encountered an issue when I got to the final step where I had to access the database from the internet through the browser. I got an error saying: Error!: SQLSTATE[HY000] [1045] Access denied for user 'example_user'@'localhost' (using password: YES) After several attempts at troubleshooting the issue, I reliased that I had not changed the password value in the PHP script to the new password that I created when I created the example_user.

