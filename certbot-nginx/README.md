## Create SSL using cert bot . You can also follow certbot website for more details :

## Update server
```
sudo apt-get update && apt dist-upgrade -y
```
## Install snapd
```
sudo apt-get install -y snapd

```
## Remove certbot-auto and any Certbot OS packages
```
sudo apt-get remove certbot, sudo dnf remove certbot, or sudo yum remove certbot.
```

## Install Certbot
```
sudo snap install --classic certbot

```
## Prepare the Certbot command
### Execute the following instruction on the command line on the machine to ensure that the certbot command can be run.
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot

```
## Choose how you'd like to run Certbot
### Either get and install your certificates...
### Run this command to get a certificate and have Certbot edit your nginx configuration automatically to serve it, turning on HTTPS access in a single step.
```
sudo certbot --nginx

```
## Or, just get a certificate
### If you're feeling more conservative and would like to make the changes to your nginx configuration by hand, run this command.
```
sudo certbot certonly --nginx

```
## Test automatic renewal
```
sudo certbot renew --dry-run
```
