Install Let’s Encrypt SSL
HTTPS is a protocol for secure communication between a server (instance) and a client (web browser). Due to the introduction of Let’s Encrypt, which provides free SSL certificates, HTTPS are adopted by everyone and also provides trust to our audiences.

```sudo apt install python3-certbot-nginx```
Now we have installed Certbot by Let’s Encrypt for Ubuntu 22.04, run this command to receive our certificates.

```sudo certbot --nginx --agree-tos --redirect -m youremail@email.com -d domainname.com -d www.domainname.com```
Certificates provided by Let’s Encrypt are valid for 90 days only, so we need to renew them often. So, let’s test the renewal feature using the following command.

```sudo certbot renew --dry-run```
This command will test the certificate expiry and configures the auto-renewable feature.