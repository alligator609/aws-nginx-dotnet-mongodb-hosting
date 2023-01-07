sudo apt update
sudo apt upgrade
sudo apt install nginx
sudo ufw app list
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status
sudo systemctl status nginx
sudo apt install mysql-server
sudo service mysql status
sudo mysql -V
sudo mysql_secure_installation

sudo apt install php8.1-fpm php8.1 php8.1-common php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-curl php8.1-gd php8.1-imagick php8.1-cli php8.1-imap php8.1-mbstring php8.1-opcache php8.1-soap php8.1-zip php8.1-intl php8.1-bcmath unzip -y

php -v

reference:
https://www.tutsmake.com/how-to-install-lemp-stack-nginx-mysql-php-on-ubuntu-22-04/