Sign the ClickOnce Manifests
====================

We have a previous post blog on medium that we must bring to this post here.

The most updated article that uses [VNext is this one](https://blogs.msdn.microsoft.com/tfssetup/2015/10/15/building-clickonce-apps-using-build-vnext/), but it gets the publishing output to a file share.

There is this ClickOnce intro material on [how to build ClickOnce from command line](https://msdn.microsoft.com/en-us/library/ms165431.aspx)

According to [this reference](https://msdn.microsoft.com/en-us/library/xc3tc5xx.aspx?f=255&MSPPError=-2147217396) you cannot use msbuild to deploy the application if you need advanced features like Trusted Application Deployment (I suspect signing manifests too). So we will have to stick with mage.exe for the deployment part. Note that publish can still be performed by msbuild with the publish target.

