Please visit below KB to get CU 7:
https://support.microsoft.com/en-us/kb/3092423
To apply this fix, follow these steps:
	1. Upgrade the servers to the .NET Framework 4.5.2 or onward.
	2. Install the cumulative update package.
	3. Please make sure to put following setting in the DistributedCacheService.exe.config file:

 \<appSettings>
     \<add key="backgroundGC" value="true"/>
\</appSettings>

	1. Restart the AppFabric Caching service for the update to take effect.
Note By default, the DistributedCacheService.exe.config file is located under the following directory: %ProgramFiles%\AppFabric 1.1 for Windows Server
Important for Client Application:

It is important that your application or development environment use the same assemblies as the cache servers. During any upgrade of the distributed cache system, make sure that all cache clients using that system have the same versions of the assemblies. Check this by comparing the product version of the cache client's Microsoft.ApplicationServer.Caching.Client.dll file with the product version of the cache server's Microsoft.ApplicationServer.Caching.Server.dll file located in the installation folder.

        • Microsoft.ApplicationServer.Caching.Core.dll
        • Microsoft.ApplicationServer.Caching.Client.dll
        • Microsoft.WindowsFabric.Common.dll
        • Microsoft.WindowsFabric.Data.Common.dll

For More Information, please visit the link:https://msdn.microsoft.com/en-us/library/ff637695%28v=azure.10%29.aspx
