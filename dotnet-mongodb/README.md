#  Install .NET on Ubuntu 22.04; .NET 6 and .NET 7 are supported.

Use the ``` dotnet --list-sdks and dotnet --list-runtimes ``` commands to see which versions are installed.

Note : Install the SDK if you want to develop & run .NET apps , install the Runtime if you want only run .NET apps.

 ## Install the SDK
``` sudo apt-get update && \ sudo apt-get install -y dotnet-sdk-7.0```
``` sudo apt  install dotnet-host-7.0 ```
``` sudo apt  install dotnet-host-6.0 ```


 ##  Install the runtime

 ```sudo apt-get update && \ sudo apt-get install -y aspnetcore-runtime-7.0 ```

 ## Deactivate HTTPS Redirection Middleware in the Development environment  in program.cs 
 ```  
 if (!app.Environment.IsDevelopment()){
    app.UseHttpsRedirection();
    }
```

## Remove https://localhost:5001 (if present) from the applicationUrl property in the Properties/launchSettings.json file.

## From the command line, run the app: 
``` dotnet <app_assembly>.dll  ```

## Configure a reverse proxy server in program.cs 
 Forwarded Headers Middleware should run before other middleware.
 ``` 
 app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
}); 
```


 ## then follow nginx installation 

``` 
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://127.0.0.1:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

## Configure the ASP.NET Core Web App To Run as a Service
1. Ubuntu contains a service manager called systemd
2. which acts as an initialization system and controls what programs and services run when the system boots up. We shall use systemd to manage our Kestrel server
3. To create the service, we first need to create the configurations in a systemd unit file.
4. The unit file contains information regarding the unit, which is a service in this case. For services, it should have the .service extension and contain some information about the service. These files are required to be in the /etc/systemd/system directory
5. Let’s use “nano” to create the unit file and name it kestrel-app.service:

``` sudo nano /etc/systemd/system/kestrel-app.service ```
``` 
[Unit]
Description=ASP.NET Core Web App running on Ubuntu

[Service]
WorkingDirectory=/var/www/app
ExecStart=/usr/bin/dotnet /var/www/app/DeployingToLinuxWithNginx.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-web-app
# This user should exist on the server and have ownership of the deployment directory
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target

```

```sudo systemctl enable kestrel-app.service ```
``` sudo systemctl start kestrel-app.service ```
``` sudo systemctl stop kestrel-app.service ```
## if u change file name then ReStart deamon
``` sudo systemctl daemon-reload ```
## to check for error logs 
sudo journalctl -fu kestrel-app.service
## to give permission for exact folder 
chmod -R 777 logs
##  check status 
``` sudo systemctl status kestrel-app.service ``` 
## Install mogodb 
check mogodb in linux 


<a href="https://code-maze.com/deploy-aspnetcore-linux-nginx/"> Ref </a>