---
published: false
---
Continuous Deployment of ClickOnce Binaries from a WPF Application
====================

The application is written in WPF and it is configured to be deployed using ClickOnce

The deployment files must be copied to an an external FTP server where will be avaiblable for customer download.

The application assemblies must be signed with a pfx file

The build takes place on Visual Studio Team Services using a hosted agent.



We have a previous post blog on medium that we must bring to this post here.

The most updated article that uses [VNext is this one](https://blogs.msdn.microsoft.com/tfssetup/2015/10/15/building-clickonce-apps-using-build-vnext/), but it gets the publishing output to a file share of local build folder. In our case we must upload files to an external ftp server. 


There is this ClickOnce intro material on [how to build ClickOnce from command line](https://msdn.microsoft.com/en-us/library/ms165431.aspx)

According to [this reference](https://msdn.microsoft.com/en-us/library/xc3tc5xx.aspx?f=255&MSPPError=-2147217396) you cannot use msbuild to deploy the application if you need advanced features like Trusted Application Deployment (I suspect signing manifests too). So we will have to stick with mage.exe for the deployment part. Note that publish can still be performed by msbuild with the publish target.

It seems cURL [is not capable of managing](http://stackoverflow.com/questions/35149497/tfs-2015-build-vnext-curl-ftp-upload-buggy-or-difficult-to-use) files and folder with spaces. And we do have such scenario because the clickonce deployment files and folders like "Application Files". 
I also found an article entitled [ClickOnce From Azure Blob Storage](http://jake.ginnivan.net/clickonce-from-azure-blob-storage/) that gives good examples of msbuild targets to do assembly generation, signing and publishing. There is a link to the entire script we can use as a reference.

## 07/20/2016

Regarding automatic ClickOnce installer, how can we automatically increment the version number after each build on our build task on Visual Studio Team Service?

http://publicvoidlife.blogspot.de/2014/05/continuous-integration-with-tfs-and.html?showComment=1441022327672

The approach on the referenced article relies on using the changeset as the verion change increment. We must study how to include this on msbuild on team services.

## 01/26/2017
After the expiration of my code signing certificate, Visual Studio was not longer able to run the following command:

Severity	Code	Description	Project	File	Line	Suppression State
Error		The command ""C:\Program Files (x86)\Microsoft SDKs\Windows\v7.1A\bin\signtool.exe" sign /f "Otimize.pfx" /p #Tucano,79 /v "C:\Users\Igor\Source\Workspaces\OtimizeTrueShapeNesting\ClientApp\feature\PartListDataGrid\SuperNestingClient\obj\Debug\OtimizeNestingClient.exe"" exited with code 1.	SuperNestingClient	C:\Users\Igor\Source\Workspaces\OtimizeTrueShapeNesting\ClientApp\feature\PartListDataGrid\SuperNestingClient\SuperNestingClient.csproj	737	

I think we can use exactly this command on the build automation task to properly sign my assemblies