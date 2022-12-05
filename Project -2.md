# WEB STACK IMPLEMENTATION (LEMP STACK)

## STEP 1 – INSTALLING THE NGINX WEB SERVER

Steps
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