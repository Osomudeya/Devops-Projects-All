## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

AWS Account setup and provisioning on Ubuntu server

steps

1. Logged in to my AWS account as IAM user
2. I selected Ubuntu free tier instance
3. I had the required configurations such as ((Enabling public IP, setting up security group, and key pair) and, finally launched the instance.)
   ![](/Users/mac/Desktop/git repo build/git-1)
   ![](/Users/mac/Desktop/git repo build/git-2)
   ![](/Users/mac/Desktop/git repo build/git-3)

4. Next I SSH into the instance using Mac Terminal 
5. In the Terminal, I typed cd Downloads to navigate to the location of my key-pair. 
6. Inside the Downloads directory, I connected to my instance using the Public DNS. "ssh -i <keypair-name> ubuntu@<pubicDNS>"

### INSTALLING APACHE AND UPDATING THE FIREWALL

Steps
1. Install Apache using Ubuntu’s package manager ‘apt', Run the following commands: To update a list of packages in package manager: sudo apt update -y

2. To run apache2 package installation: sudo apt install apache2 -y

3. Next, I verified that Apache2 is running as a service in the OS. with command: sudo systemctl status apache2

4. The green light indicates Apache2 is running.

