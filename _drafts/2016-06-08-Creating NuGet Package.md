Nuget Package Creation
====================

We had to create a NuGet package to pack a third party licensing library set we use for software rights managements, so it can be reused among other projects.

NuGet is a package manager for the Microsoft development plataform including .NET. A package is a bundle that includes everything necessary to install a library or tool. The packages are available in feeds that Visual Studio accesses to present list of available packages. The official feed is the [NugGet Gallery](http://nuget.org/). Visual Studio contains the user interface dialog to find, install, remove and update packages on your project.

The idea behind this is to use the main benefits of packing components:

* Omit binaries from source control repository, minimizing its size and time to clone it. This keeps the repository cleaner.
* Avoid team members to directly reference binareis from source control. They now have to reference a package.
* Better dependency management. Project owners do not have to include dependent assemblies in their setup. Just add the dependencies on teh package especification and they will be automatically downloaded and installed.

So this specific package presented some technicall challenges:

* Differentt binnaries for x86 and x64 platforms
* Managed and unmanaged libraries
* Eventual configuration file transformation

## A Word About NuGet Client Version

NuGet project is evolving constantly and at the time of this text, Visual Studio 2013 support up to NuGet 2.8.7
while Visual Studio 2015 supports 3.4.2. So pay attention when create a package to use feature supported by the Visual Studio version you want to target.

## Different Platform Binnaries
When the package is ready and someone decides to include this package in his project, it must install the correct libraries, depending on the platform (x86 or x64) the project is being compiled.

For this specific Software Rights Management library, there is no AnyCPU compatible library nor the x86 version run on WoW. So the package must somehow include a check that one of the two platforms are select prior to compilation.

To acomplish this, we based on this article: [Creating a Windows Store App NuGet Package for ARM, x86 and x64 article](http://dontcodetired.com/blog/post/Creating-a-Windows-Store-App-NuGet-Package-for-ARM-x86-and-x64).

We had to make three things to make it work:
1- Inlude the "build" folder on the package with the structure presented in the following image.

![build folder](..\images\2016-06-08-Creating Nuget Package\BuildFolder.png "Build Folder")

 Then we must include the specific platform libraries on each created folder. like the following image:

 ![build folder](..\images\2016-06-08-Creating Nuget Package\x64BuildFolder.png "x64 Build Folder")

2 - Import MSBuild props file into project
When the package is ready, visual studio must add a reference for the correct library on the project "References" node. As both libraries (x86 and x64) cannot co exist in the project, we must change the .csproj file to include conditionaly references, based on the chosen platform version.

Create a new file named {packageid}.props and include the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <ItemGroup Condition="'$(Platform)' == 'x86'">
    <Reference Include="hasp_net_windows">
      <HintPath>$(MSBuildThisFileDirectory)$(Platform)\hasp_net_windows.dll</HintPath>
    </Reference>
  </ItemGroup>
  <ItemGroup Condition="'$(Platform)' == 'x64'">
    <Reference Include="hasp_net_windows_x64">
      <HintPath>$(MSBuildThisFileDirectory)$(Platform)\hasp_net_windows_x64.dll</HintPath>
    </Reference>
  </ItemGroup>
</Project>
```

> We tried to rename the x64 library to the same name of the x86 so this XML could be simplified, but for some reason (maybe tamper proofing) if we rename the library name a FileNotFoundException is thrown when we try touse the library.

3- Import a MSBuild target into project
As this library do not work with AnyCPU platform compilation, we must alert during the target project compilation if AnyCPU is being used. In order to do that, add a {packageid}.targets file and use the following code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="PlatformCheck" BeforeTargets="ResolveAssemblyReferences"
            Condition=" ( ('$(Platform)' != 'x86') AND ('$(Platform)' != 'x64') )">
            <Error  Text="$(MSBuildThisFileName) does not work correctly on '$(Platform)' 
                     platform. You need to specify platform (x86 or x64)." />
    </Target>
</Project> 
```

Altought the convention states that libraries should be placed on the "lib" folder of the package, we cannot adopt this approach, since we cannot have both libraries added at the project references node at the same time. But, if your .dlls have the same name, one thing you can try is to include the "references" node on the nuspec file so you can explicity specify what libraries should go on the project "references" node, no matter what you have on the "lib" folder:
 
 ![References Node](..\images\2016-06-08-Creating Nuget Package\referencesNode.png "References Node")

Maybe this works for you. [Please check this forum thread about this issue](http://stackoverflow.com/questions/10198428/nuget-where-to-place-dlls-for-unmanaged-libraries). It was originally written to deal un managed / unmanaged scenarios, but it worth a try.


## Native Libraries

Altought the libraries we include in the project are managed, they depend on some unamanged (native) ones that must included on the target project output folder. In order to make it work, we decided to perform two things:

1 - Add native libraries on its matching build folder, inside the corresponding platforms (x86 and x64). However, when the final package is added by the client this files are not copied to anywhere. They just stay on the "packages" folder of the solution. So the next step show how to copy them to the output directory

2 - Modify the previously added targets file to include this:

```xml
<Target Name="CopyMyPackage" AfterTargets="AfterBuild">
    <ItemGroup>
      <HaspRuntimeLibraries Include="$(MSBuildThisFileDirectory)$(Platform)\*.*"/>
    </ItemGroup>
    <Copy SourceFiles="@(HaspRuntimeLibraries)" DestinationFolder="$(OutputPath)">
    </Copy>
  </Target> 
  ```
  This will end up copying the managed library already added there on the first charpter of this article, but this is not a problem, since these libraries will end up being there anyway. One alternative could be adding the native libraries to a different folder, but we will moving form the NuGet stantadard folders like build, content, tools and lib. In adition, if you include a .dll file in the nuspec files node that is no located on the "lib" folder, nuget displays a warning during the packing.

## Configuration File Transformation
One of the libraries requires that a specific App.Config startup node attribute be set. In order to do that, we can [include XML transformation file (XDT) to the package](https://docs.nuget.org/create/configuration-file-and-source-code-transformations) to set this attribute during the package installation, if necessary.

Include a app.config.install.xdt file inside the "content" folder of the package containing the following code:
```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <startup useLegacyV2RuntimeActivationPolicy="true" xdt:Transform="SetAttributes(useLegacyV2RuntimeActivationPolicy)">
  </startup>
</configuration>
```

If you want to remove this attribute when the package is uninstalled, use the remove atribute. Add an app.config.uninstall.xdt file into the same folder and define it as the following:

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <startup useLegacyV2RuntimeActivationPolicy="true" xdt:Transform="RemoveAttributes(useLegacyV2RuntimeActivationPolicy)">
  </startup>
</configuration>
```

## NuGet Package Explorer
We started creating packages using the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) GUI, but we experienced some issues while updating nuspecs and also when hosting the resulting nupkg to our server. So we ended up using the noted and command line tools.

## Testing What you Done
Before publishing your created package into a public gallery, like [NugGet Gallery](http://nuget.org/) you can locally "publish" your package so it is available for other projects to consumeit.
All you need to do is go to tools->NuGet Package Manager->Package Manager Settings and create a new package source, like the following image:

 ![Package Source](..\images\2016-06-08-Creating Nuget Package\PackageSource.png "Package Source")

## References
1 - [Creating a single NuGet package containing x86, x64 and ARM binaries](https://wpfspark.wordpress.com/2016/01/21/wpfspark-uwp-creating-a-single-nuget-package-containing-x86-x64-and-arm-binaries/)

2- [Creating a Windows Store App NuGet Package for ARM, x86 and x64](http://dontcodetired.com/blog/post/Creating-a-Windows-Store-App-NuGet-Package-for-ARM-x86-and-x64)

3- [Packaging a Windows Store apps component with nuget](https://blogs.msdn.microsoft.com/mim/2013/09/02/packaging-a-windows-store-apps-component-with-nuget-part-2/)

4 - [Using NuGet 2.5 to deliver unmanaged dlls](http://alski.net/post/2013/05/23/Using-NuGet-25-to-deliver-unmanaged-dlls.aspx)