---
layout: post
title: Task.ContinueWith Has no Synchronization Context
---

Similar to my [previous post](2016-08-09-MVVM-Light-conflict-ServiceLocation.md), we experienced another issue in result of downgrading our WPF business application from .NET 4.5 to 4.0.

The tricky thing here is the problem happens only when the application runs on Windows XP (which supports up to .NET 4.0). Although the solution is targeted to .NET 4.0 and should behave the same way no matter is the operating system version, there is a difference when you run the application on a system with .NET 4.5. Since [.NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx), the binaries for .NET 4.0 does not exists on the PC with .NET 4.5 installed.

![Conflict](..\images\in-place-upgrade-net45.png)

This means  my application uses different binaries when it runs on Windows XP from newer versions.

Digging on the net, I discovered [.NET 4.0 has a known bug](https://social.msdn.microsoft.com/Forums/vstudio/en-US/629d5524-c8db-466f-bc27-0ced11b441ba/taskcontinuewith-from-wcf-client-call-has-no-synchronizationcontext?forum=wcf) around keeping synchronization context when using `TaskScheduler.FromCurrentSynchronizationContext()`.

![Conflict](..\images\dll-hell.jpg)

So in order to fix it, I adopted [Steven S. solution](http://stackoverflow.com/questions/4659257/how-can-synchronizationcontext-current-of-the-main-thread-become-null-in-a-windo) on this thread. Basically we keep a `TaskScheduler` reference of the main UI thread synchronization context and use to communicate with it from long running async operations, like `Task.ContinueWith` methods.  

This seems to fix the problem and works in both .NET 4.0 and 4.5.


 