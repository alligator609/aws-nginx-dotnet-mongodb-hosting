``` sudo apt upgrade ```

``` sudo apt update ```

``` sudo apt install nginx ```

``` sudo ufw app list ```

``` sudo ufw allow OpenSSH ```

``` sudo ufw allow 'Nginx Full' ```

``` sudo ufw enable ```

``` sudo ufw status ```

``` sudo systemctl status nginx ```

``` sudo apt install mysql-server ```

``` sudo service mysql status ```

``` sudo mysql -V ```

``` sudo mysql_secure_installation ```

~~~ 
sudo apt install php8.1-fpm php8.1 php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-intl php8.1-bcmath unzip -y 
~~~
# check if php is installed correctly  
php -v
# Configure PHP
``` sudo nano /etc/php/8.1/fpm/php.ini ```
# Hit F6 for search inside the editor and update the following values for better performance.
``` 
upload_max_filesize = 32M 
post_max_size = 48M 
memory_limit = 256M 
max_execution_time = 600 
max_input_vars = 3000 
max_input_time = 1000 
```
# restart 
``` sudo service php8.1-fpm restart ```

#  configure nginx for your website, and to run php use below conf file 
~~~ server {

    listen 80; listen 443; 
    server_name todo.techbucket.xyz;
    root /var/www/todo.techbucket.xyz/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.php index.html index.htm index.nginx-debian.html;

    charset utf-8;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }
}
~~~

# change storage permission
``` chown -R www-data:www-data storage ```

# be sure to check composer install guiline , so u can use laravel  and composer command
# Autoloader Optimization

~~~
 composer install --optimize-autoloader --no-dev 
~~~

# Optimizing Configuration Loading
~~~
php artisan config:cache
~~~

# Optimizing Route Loading
~~~
php artisan route:cache
~~~

# Optimizing View Loading

~~~
php artisan view:cache
~~~
reference:
https://www.tutsmake.com/how-to-install-lemp-stack-nginx-mysql-php-on-ubuntu-22-04/ 