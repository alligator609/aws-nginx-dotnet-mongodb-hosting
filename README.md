# Step 1 Install Nginx
sudo apt update
sudo apt install nginx
# Step 2 – Adjusting the Firewall
1. sudo ufw allow 'Nginx HTTP' // for http on port 
1. sudo ufw enable // enable 
3. sudo ufw status // check status

# Step 3 – Checking your Web Server
systemctl status nginx // server status 

# Step 4 – Managing the Nginx Process

1. To stop your web server, type:

 `sudo systemctl stop nginx`  
2. To start the web server when it is stopped, type:

 `sudo systemctl start nginx`
3. To stop and then start the service again, type:

`sudo systemctl restart nginx`
4. If you are only making configuration changes, Nginx can often reload without dropping connections. To do this, type:

`sudo systemctl reload nginx`
5. By default, Nginx is configured to start automatically when the server boots. If this is not what you want, you can disable  this behavior by typing:

`sudo systemctl disable nginx`
6. To re-enable the service to start up at boot, you can type:

`sudo systemctl enable nginx`

# Step 5 – Setting Up Server Blocks (Recommended)
1. Create the directory for your_domain as follows, using the -p flag to create any necessary parent directories:
 `sudo mkdir -p /var/www/your_domain/html`

2. Create the directory for your_domain as follows, using the -p flag to create any necessary parent directories:

   `sudo mkdir -p /var/www/your_domain/html`
3. Next, assign ownership of the directory with the $USER environment variable:

   `sudo chown -R $USER:$USER /var/www/your_domain/html`
4. The permissions of your web roots should be correct if you haven’t modified your umask value, which sets default file permissions. To ensure that your permissions are correct and allow the owner to read, write, and execute the files while granting only read and execute permissions to groups and others, you can input the following command:

   `sudo chmod -R 755 /var/www/your_domain`
5. Next, create a sample index.html page using nano or your favorite editor:

   `nano /var/www/your_domain/html/index.html`



reference:
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04"# aws-nginx-hosting" 
