## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

AWS Account setup and provisioning on Ubuntu server

steps

1. Logged in to my AWS account as IAM user
2. I selected Ubuntu free tier instance
3. I had the required configurations such as ((Enabling public IP, setting up security group, and key pair) and, finally launched the instance.)
   ![git-1](https://user-images.githubusercontent.com/99102616/205598041-8db60c38-cd68-4744-acd8-97fc21ac73d6.png)
   ![git-2](https://user-images.githubusercontent.com/99102616/205598658-252b53e3-9072-476c-8576-9efbd8027b38.png)
   ![git-3](https://user-images.githubusercontent.com/99102616/205598688-4128e032-9eab-4ae7-8841-b0d92955c705.png)
4. Next I SSH into the instance using Mac Terminal 
5. In the Terminal, I typed cd Downloads to navigate to the location of my key-pair. 
6. Inside the Downloads directory, I connected to my instance using the Public DNS. "ssh -i <keypair-name> ubuntu@<pubicDNS>"
   <img width="917" alt="git-4" src="https://user-images.githubusercontent.com/99102616/205599871-ac62cdb2-1e96-4e8d-92a3-d24a0b722bac.png">

### INSTALLING APACHE AND UPDATING THE FIREWALL

Steps
1. Install Apache using Ubuntu’s package manager ‘apt', Run the following commands: To update a list of packages in package manager: sudo apt update -y

2. To run apache2 package installation: sudo apt install apache2 -y

3. Next, I verified that Apache2 is running as a service in the OS. with command: sudo systemctl status apache2

4. The green light indicates Apache2 is running.
   <img width="919" alt="git-5" src="https://user-images.githubusercontent.com/99102616/205601135-23fc6b5a-3121-4d09-b7c0-afe97974d612.png">

5. Open port 80 on the Ubuntu instance inbound security group to allow access from the internet.

6. Access the Apache2 service locally in our Ubuntu shell by running: curl http://localhost:80 or curl http://127.0.0.1:80 This command would output the Apache2 payload indicating that it is accessible locally in the Ubuntu shell.

7. Next, test that Apache HTTP server can respond to requests from the Internet. Open a browser and type the public IP of the Ubuntu instance: http://52.87.235.179:80 This outputs the Apache2 default page.
   <img width="1223" alt="apache" src="https://user-images.githubusercontent.com/99102616/205745984-38ec7730-64e8-4e5a-8080-9c375f3c02db.png">

## INSTALLING MYSQL

**Steps**
In this step, I installed a Database Management System (DBMS)  for storing and managing data on the site in a relational database.

1. Run ‘apt’ to acquire and install this software, run: sudo apt install mysql-server

2. Confirm installation by typing Y when prompted.

3. Once installation is complete, log in to the MySQL console by running: sudo mysql
4. Next, run a security script that comes pre-installed with MySQL, to remove insecure default settings and lock down access to your database system. run: ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'; then exit MySQL shell by typing exit and enter.

5. Run interactive script by typing: sudo mysql_secure_installation and following the instructions.

6. Next, test that login to MySQL console works. Run: sudo mysql -p
   <img width="757" alt="Screen Shot 2022-12-05 at 10 47 00 PM" src="https://user-images.githubusercontent.com/99102616/205749328-9888e02a-8060-4651-98d8-1798dc500950.png">
7. Type exit and enter to exit console.

## STEP 3 — INSTALLING PHP

**Steps**
1. To install these 3 packages at once, run: sudo apt install php libapache2-mod-php php-mysql -y

2. After installation is done, run the following command to confirm your PHP version: php -v

<img width="561" alt="Screen Shot 2022-12-05 at 11 02 29 PM" src="https://user-images.githubusercontent.com/99102616/205751956-2b6053fd-5415-4f86-8ba0-dbcc20911df2.png">

3. At this point, your LAMP stack is completely installed and fully operational.

## STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE

**Steps**
1. Setting up a domain called project lamp. Create the directory for project lamp using ‘mkdir’. Run: sudo mkdir /var/www/projectlamp
2. Assign ownership of the directory with your current system user, run: sudo chown -R $USER:$USER /var/www/projectlamp
3. Next, create and open a new configuration file in Apache’s sites-available directory. Type: sudo vi /etc/apache2/sites-available/projectlamp.conf
4. This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text: <VirtualHost *:80> ServerName projectlamp ServerAlias www.projectlamp ServerAdmin webmaster@localhost DocumentRoot /var/www/projectlamp ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined
   <img width="567" alt="Screen Shot 2022-12-05 at 11 15 10 PM" src="https://user-images.githubusercontent.com/99102616/205754490-52f8341b-945a-488b-8ffa-46a927e0b935.png">
5. To save and close the file. Hit the esc button on the keyboard, Type :, Type wq. w for write and q for quit and Hit ENTER to save the file. 
6. use the ls command to show the new file in the sites-available directory: sudo ls /etc/apache2/sites-available
7. Next, use a2ensite command to enable the new virtual host: sudo a2ensite projectlamp

8. Disable the default website that comes installed with Apache. type: sudo a2dissite 000-default

9. Ensure your configuration file doesn’t contain syntax errors, run: sudo apache2ctl configtest

10. Finally, reload Apache so these changes take effect: sudo systemctl reload apache2

11. The website is active, but the web root /var/www/projectlamp is still empty. 
12. Create an index.html file in that location so that we can test that the virtual host works as expected:sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html

13. Reload the public IP to see changes to the apache2 default page.

<img width="1258" alt="Screen Shot 2022-12-05 at 11 34 37 PM" src="https://user-images.githubusercontent.com/99102616/205757513-2a5141c9-e0cb-4301-8358-d82faed6daee.png">

## STEP 5 — ENABLE PHP ON THE WEBSITE

**Steps**

1. With the default Directory Index settings on Apache, a file named index.html will always take precedence over an index.php file. To make index.php file tak precedence need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive.

2. Run: sudo vim /etc/apache2/mods-enabled/dir.conf then: #Change this: #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm #To this: DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm

3. Save and close file.

4. Next, reload Apache so the changes take effect, type: sudo systemctl reload apache2

5. Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server. Create a new file named index.php inside the custom web root folder, run: vim /var/www/projectlamp/index.php

6. This will open a blank file. Add the PHP code: <?php phpinfo();

7. Save and close the file, then refresh the page to see changes.
   <img width="1256" alt="Screen Shot 2022-12-05 at 11 44 00 PM" src="https://user-images.githubusercontent.com/99102616/205758974-cd3ba78a-66a3-4784-b36d-036e83a9ede9.png">

