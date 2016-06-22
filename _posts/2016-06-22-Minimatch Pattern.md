Minimatch Pattern
====================

First time I saw the term [minimatch pattern](https://github.com/isaacs/minimatch) was during this wee while trying to set Continuous Deployment on Visual Studio Team Services for a WPF application that must be deployed using ClickOnce.

One of the tasks I tried using on the build definition was a [cURL Upload Files](https://www.visualstudio.com/docs/build/steps/utility/curl-upload-files). It seems to work fine, but the problem is to keep the same file/folder structure after upload contents to the server.

The files copied must be specified using minimatch pattern. AS far I understand this is a minimal matching utility and is the de-facto file matching library used internally by node/npm and many node-based projects.

The way I see, I cannot define a minimatch pattern on cURL task that will upload files to my ftp server while keeping the file/folder structure.

For the moment, I was not able to find any off-the-shelf build task on Visual Studio Team Services that is capable of doing that. I think I will have to do it using PowerShell script or something similar. Any ideas?

I opened [a MSDN forum thread](https://social.msdn.microsoft.com/Forums/en-US/4777d4ba-72e9-4e04-ac18-c9bca48885a4/copy-clickonce-apppublish-folder-to-external-ftp-server?forum=TFService) about this subject.


