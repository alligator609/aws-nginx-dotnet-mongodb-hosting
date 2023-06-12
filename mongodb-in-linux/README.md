# Install MongoDB Community Edition onn Ubuntu
 ## Import the public key used by the package management system
 ``` 
 sudo apt-get install gnupg
 
 ```
 ```
  curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \ sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \ --dearmor
 
 ```
## Create a list file for MongoDB

```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

## Reload local package database.

``` 
sudo apt-get update
```

## Install the MongoDB packages

``` 
sudo apt-get install -y mongodb-org 
```

## Start MongoDB

```
sudo systemctl start mongod

```
## enable MongoDB
```
sudo systemctl enable mongod
```
## stop MongoDB
```
sudo systemctl stop mongod
```
## restart MongoDB
```
sudo systemctl restart mongod
```
## Begin using MongoDB.

```
mongosh

```
## show dbs

``` 
shob dbs 

```
## Create new database
```  use DATABASE_NAME ```
## Know your current selected database
``` db ```
## Drop database

``` db.dropDatabase(test)   ```

## Create collection
``` db.createCollection("name") ``` 
## To check collections list
```
show collections
```
## Drop collection

``` db.createCollection(name) ```

## Using mongodump to Back Up a MongoDB Database

### Make directory  
```
 sudo mkdir /var/backups/mongobackups
```

```
sudo mongodump --db newdb --out /var/backups/mongobackups/$(date +'%m-%d-%y')
```

## Using mongorestore to Restore and Migrate a MongoDB Database

```
sudo mongorestore --db newdb --drop /var/backups/mongobackups/10-29-20/newdb/

```

