# Assignment 3 Part 1

## Creating a New User

1. Log in to your server as the root user. Replace `your_server_ip` with your ip address server.
   ```
   ssh -i ~/.ssh/do-key root@your_server_ip
   ```
2. Use the following command to update the package lists:
   ```
   sudo apt update
   ```
3. Install the `sudo` package:
   ```
   sudo apt install sudo
   ```
4. Create a new user with the name of your choice. Replace `newuser` with your desired username:
   ```
   sudo adduser newuser
   ```
5. Grant administrative privileges to the new user:
   ```
   sudo usermod -aG sudo newuser
   ```
6. Change the default shell of the new user to `bash`:
   ```
   sudo chsh -s /bin/bash newuser
   ```
7. Test the new user's SSH access by logging in as the new user:
   ```
   ssh newuser@your_server_ip
   ```

## Preventing Root User from SSH Access

1. Open the SSH configuration file:
   ```
   sudo nano /etc/ssh/sshd_config
   ```
2. Find the line that reads `PermitRootLogin yes` and change it to `PermitRootLogin no`.
3. Save and close the file.
4. Restart the SSH service:
   ```
   sudo systemctl restart sshd
   ```

## Installing Nginx

1. Use the following command to update the package lists:
   ```
   sudo apt update
   ```
2. Install Nginx by running the following command:
   ```
   sudo apt install nginx
   ```

## Configuring Nginx to Serve a Sample Website

1. Create a new directory for your website:
   ```
   sudo mkdir -p /var/www/nginx.com/html
   ```
2. Create a sample `index.html` file:
   ```
   sudo vim /var/www/nginx.com/html/index.html
   ```
   Add the following content to the file:
   ```
   <html>
       <head>
           <title>Home</title>
       </head>
       <body>
           <h1>Welcome to Ghiervis' website</h1>
       </body>
   </html>
   ```
3. Create a new server block configuration file:
   ```
   sudo vim /etc/nginx/sites-available/nginx.com
   ```
   Add the following content to the file:
   ```
   server {
       listen 80;
       listen [::]:80;

       root /var/www/nginx.com/html;
       index index.html;

       server_name nginx.com www.nginx.com;

       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```
4. Create a symbolic link to enable the server block:
   ```
   sudo ln -s /etc/nginx/sites-available/nginx.com /etc/nginx/sites-enabled/
   ```
5. Test the Nginx configuration:
   ```
   sudo nginx -t
   ```
6. Restart Nginx to apply the changes:
   ```
   sudo systemctl restart nginx
   ```