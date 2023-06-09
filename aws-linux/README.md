# Install Nginx
amazon-linux-extras install nginx1.12

# Back up existing config
mv /etc/nginx /etc/nginx-backup

# Download the configuration from S3
aws s3 cp s3://{my_bucket}/nginxconfig.io-example.com.zip /tmp

# Install new configuration
unzip /tmp/nginxconfig.io-example.com.zip -d /etc/nginx


// install aws nginx 


$ sudo amazon-linux-extras list | grep nginx
 38  nginx1=latest            disabled      [ =stable ]

$ sudo amazon-linux-extras enable nginx1
 38  nginx1=latest            enabled      [ =stable ]
        
Now you can install:
$ sudo yum clean metadata
$ sudo yum -y install nginx

$ sudo service nginx start
$ sudo systemctl enable nginx
$ sudo systemctl status nginx // status 
$ sudo firewall-cmd --state  // firewall status
$ sudo yum install firewalld
$ sudo systemctl start firewalld 
$ sudo systemctl enable firewalld
$ sudo firewall-cmd --add-service=http --permanent 
$ sudo firewall-cmd --add-service=https --permanent
$ sudo firewall-cmd --reload
$ cd var/log/nginx // log
$ netstat -tunlp
$ iptables -L


$ nginx -v
nginx version: nginx/1.16.1


Directory 
/usr/share/nginx/html


