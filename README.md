# Step 1 Install Nginx
sudo apt update
sudo apt install nginx
# Step 2 – Adjusting the Firewall
1. sudo ufw app list
2. sudo ufw allow OpenSSH
3. sudo ufw allow 'Nginx Full'
4. sudo ufw allow 'Nginx HTTP' // for http on port 
5. sudo ufw enable // enable 
6. sudo ufw status // check status

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

2. Next, assign ownership of the directory with the $USER environment variable:

   `sudo chown -R $USER:$USER /var/www/your_domain/html`
3. The permissions of your web roots should be correct if you haven’t modified your umask value, which sets default file permissions. To ensure that your permissions are correct and allow the owner to read, write, and execute the files while granting only read and execute permissions to groups and others, you can input the following command:

   `sudo chmod -R 755 /var/www/your_domain`
4. Next, create a sample index.html page using nano or your favorite editor:

   `nano /var/www/your_domain/html/index.html`


Save and close the file by pressing Ctrl+X to exit,

In order for Nginx to serve this content, it’s necessary to create a server block with the correct directives. Instead of modifying the default configuration file directly, let’s make a new one at /etc/nginx/sites-available/your_domain:

`sudo nano /etc/nginx/sites-available/your_domain`
Paste in the following configuration block, which is similar to the default, but updated for our new directory and domain name:
 /etc/nginx/sites-available/your_domain

 ~~~
server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
} 
~~~

Notice that we’ve updated the root configuration to our new directory, and the server_name to our domain name.

Next, let’s enable the file by creating a link from it to the sites-enabled directory, which Nginx reads from during startup:

`sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/`


To avoid a possible hash bucket memory problem 

`sudo nano /etc/nginx/nginx.conf`
Find the server_names_hash_bucket_size directive and remove the # symbol.

test to make sure that there are no syntax errors in any of your Nginx files

`sudo nginx -t`
If there aren’t any problems, restart Nginx to enable your changes:


`sudo systemctl restart nginx`


Now, verify the Nginx server using the curl command.

``` curl -4 http://localhost ```

To verify from outside make sure in security rules u add Inbound http and https 

 
reference:
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04 


# To see from public 