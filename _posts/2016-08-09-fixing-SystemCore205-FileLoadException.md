---
layout: post
title: Fixing System.Core 2.0.5.0 FileLoadException
---

Similar to my [previous post]({% post_url 2016-08-09-MVVM-Light-conflict-ServiceLocation %}), we experienced another issue in result of downgrading our WPF business application from .NET 4.5 to 4.0.

The problem happens only when the application runs on Windows XP (which supports up to .NET 4.0). Although the solution is targeted to .NET 4.0 and should behave the same way no matter what is the operating system version, there is a difference when you run the application on a system with .NET 4.5.

![Conflict](..\images\in-place-upgrade-net45.png)

As you can see on the previous image, [.NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx), so the binaries for .NET 4.0 are replaced when 4.5 is installed.

This means our application uses different binaries when it runs on Windows XP from newer operating systems.

![Conflict](..\images\dll-hell.jpg)

Scott Hanselman has a [great article](http://www.hanselman.com/blog/FixingSystemCore205FileLoadExceptionPortableLibrariesAndWindowsXPSupport.aspx) that enlightened me everything I had to know to realize what the problem was.

To summarize, .NET 4.0 does not support Portable Class Libraries(PCLs) on its original release. The support was added on .NET 4 vanilla by the [KB2468871](http://www.microsoft.com/en-us/download/details.aspx?id=3556) provided by Windows Update.

The concern here is if we include this KB2468871 as a requirement, users will give up on downloading such a huge update.

Besides, if their license is pirated or non genuine, maybe they will not even be able to get the update via Windows Update. We did not test a redistributable package of this update to see if it works in cases like that. It seems the download is available at the moment, so is a question to be answered yet.

If we need to solve this problem, a follow up question would be how to make sure we do not use a PCL while building our solution. We noticed some NuGet package contain multiple targets like standard net40, net540 etc and at the same time portable-net40+sl4+win8+wp8+wpa81.

Looking at the .csproj of our Visual Studio projects, it seems the standard target (net40) is selected over the portable version (whenever is possible).

In our case, the problem is in the [Common Service Locator](https://www.nuget.org/packages/CommonServiceLocator) package that is a dependency of [MVVM Light](http://www.mvvmlight.net/) v5. This package seems not to follow the multiple targeting convention. 

In the other hand, we must use latest MVVM LIght, since we depend on INavigationService and IDialogService interfaces and implementation recently added.

For the time being, we are going to add KB2468871 as a prerequisite while we found a solution. At this moment, I sent a message to the Common Service Locator authors to know if they plan to include common target versions on future package updates.

Do you have an answer for that? Please spread the word!
 