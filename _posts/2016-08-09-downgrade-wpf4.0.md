---
published: false
---
## Downgrade WPF Application to .NET 4.0

Recently we had to downgrade our business application originally targetting .NET 4.5 to .net 4.0. Here are the main challenges we faced during the process and how we overcome/worked around them.

1 - MVVM Light assembly conflict with Microsoft.Practices.ServiceLocation
Similar to what was reported on [this thread](http://stackoverflow.com/questions/14791089/mvvm-light-assembly-conflict-with-microsoft-practices-servicelocation), when I tried to install the application (using ClickOnce) the following error was shown:

> Unable to install or run the application. The application requires that assembly Microsoft.Practices.ServiceLocation Version 1.0.0.0 be installed into the Global Assembly Cache (GAC) first.


Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
