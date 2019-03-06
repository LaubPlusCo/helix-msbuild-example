
# Helix MS Build Example  

This solution shows how you can use msbuild and the new dotnetcore project format in a Sitecore Helix solution.  

No Gulp, Grunt, Cake, nant or other task runner beside msbuild. No custom build tasks or required nuget references.  

Check out [this great post on upgrading your solution .csproj files](https://natemcmaster.com/blog/2017/03/09/vs2015-to-vs2017-upgrade/) to the "new" format.

Great extension for VS2017 when working with project files https://marketplace.visualstudio.com/items?itemName=ms-madsk.ProjectFileTools

## The example features

- Automatic publish on builds from Visual Studio
- XML tranformation for config files and other xml on publish
- No dependencies on other than Visual Studio standard build targets
- PackageReferences for all dependencies
- Helix solution and Module templates for the [Sitecore Visual Studio Helix Templates extension](https://github.com/LaubPlusCo/helix-msbuild-example)