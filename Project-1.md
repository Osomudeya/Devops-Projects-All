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

### INSTALLING APACHE AND UPDATING THE FIREWALL

Steps
1. Install Apache using Ubuntu’s package manager ‘apt', Run the following commands: To update a list of packages in package manager: sudo apt update -y

2. To run apache2 package installation: sudo apt install apache2 -y

3. Next, I verified that Apache2 is running as a service in the OS. with command: sudo systemctl status apache2

4. The green light indicates Apache2 is running.

