# all database
``` SELECT schema_name FROM information_schema.schemata; ```

# show all database 
```SHOW DATABASES; ```
# create a database
``` create database your_database_name; ```
# check all users
mysql> USE mysql;
mysql> SELECT User, Host, plugin FROM mysql.user;
mysql>  
# create new user to access database
sudo mysql -u root # I had to use "sudo" since it was a new installation

``` mysql> USE mysql; ``` <br>
``` mysql> CREATE USER 'newuser'@'localhost' IDENTIFIED BY '12345678'; ``` <br>
``` mysql> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_SYSTEM_USER'@'localhost'; ``` <br>
``` mysql> UPDATE user SET plugin='auth_socket' WHERE User='YOUR_SYSTEM_USER'; ```  <br>
``` mysql> FLUSH PRIVILEGES; ``` <br>
``` mysql> exit; ``` <br>

``` sudo service mysql restart ```
# Import A MySQL Database
The file must be in .sql format. It can not be compressed in a .zip or .tar.gz file.

1. Start by uploading the .sql file onto the ubuntu server.
2. If you haven't already done so, create the MySQL database via the control panel. Click here for further instructions.
3. Using SSH, navigate to the directory where your .sql file is.
Next, run this command:

``` mysql -p -u root database_name < sms.sql ```

# select database
 ``` use db_name;  ```
# show all table 
``` SHOW TABLES ```