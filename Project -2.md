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
2. Open a new file called info.php within your document root in your text editor: sudo nano /var/www/projectLEMP/info.php
3. Type the following lines into the new file.

```
<?php
phpinfo();
```

4. You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php: http://server_domain_or_IP/info.php
5. Remove the created file, as it contains sensitive information about your PHP environment and your Ubuntu server: sudo rm /var/www/your_domain/info.php
