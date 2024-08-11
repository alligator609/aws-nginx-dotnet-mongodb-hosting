# Making ASP.NET Core application always running on IIS

Set application pool under which the application runs to:


1. Right-click on the same application pool and select “Advanced Settings”. Update the following values:
2. Set start mode to “Always Running”.
3. Setting the start mode to “Always Running” tells IIS to start a worker process for your application right away, without waiting for the initial request.
4. Idle Time out Action set to suspended

<img src="assets/always-running-iis.png">
Set Idle Time-Out (minutes) to 0.


ref (https://docs.hangfire.io/en/latest/deployment-to-production/making-aspnet-app-always-running.html)