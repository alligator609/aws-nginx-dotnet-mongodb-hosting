#  Install .NET on Ubuntu 22.04; .NET 6 and .NET 7 are supported.

Use the dotnet  ```--list-sdks and dotnet --list-runtimes ``` commands to see which versions are installed.

Note : Install the SDK if you want to develop & run .NET apps , install the Runtime if you want only run .NET apps.

 ## Install the SDK
``` sudo apt-get update && \ sudo apt-get install -y dotnet-sdk-7.0```

 ##  Install the runtime

 ```sudo apt-get update && \ sudo apt-get install -y aspnetcore-runtime-7.0 ```

 ## then follow nginx installation 

