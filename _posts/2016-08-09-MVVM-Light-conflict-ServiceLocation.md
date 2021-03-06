---
layout: post
title: MVVM Light conflict with ServiceLocation
lang: en
ref: mvvmlight
---

After downgrading my WPF business application from .NET 4.5 to 4.0 we experienced a conflict Similar to what was reported on [this thread](http://stackoverflow.com/questions/14791089/mvvm-light-assembly-conflict-with-microsoft-practices-servicelocation). When I tried to install the application (using ClickOnce) the following error was shown:

>Unable to install or run the application. The application requires that assembly Microsoft.Practices.ServiceLocation Version 1.0.0.0 be installed into the Global Assembly Cache (GAC) first.

![Conflict](..\images\conflict-between-hardware-and-software.jpg)

Although the explanation on the thread is clear about the origin of the conflict (MVVM light public key token) this should be fixed in the versions we are referencing and it should not be bothering us anymore.

In our case, the conflict started after downgrade [Microsoft Practices Logging](https://www.nuget.org/packages/EnterpriseLibrary.Logging/) package from 6.0 to 5.0. We use this library for application logging. 

As we could not find a way to fix this conflict and were already thinking about changing our logging  mechanism (something like [log4Net](https://csharp.today/log4net-tutorial-great-library-for-logging/)) we decided to remove Microsoft Practices packages and the conflict was gone.
