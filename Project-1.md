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