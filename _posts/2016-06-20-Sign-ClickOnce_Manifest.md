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

