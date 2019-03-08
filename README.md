
# Sitecore Helix Solution MS Build Example  

This solution shows how you can use msbuild and the new dotnetcore project format in a Sitecore Helix solution.  

No Gulp, Grunt, Cake, nant or other task runner beside msbuild. No custom build tasks or required nuget references.  

## The example features

- Automatic publish on builds from Visual Studio
- XML tranformation for config files and other xml on publish
- No dependencies on other than Visual Studio standard build targets
- PackageReferences for dependencies
- Helix solution and Module templates for the [Sitecore Visual Studio Helix Templates extension]([https://github.com/LaubPlusCo/helix-msbuild-example](https://marketplace.visualstudio.com/items?itemName=AndersLaublaubplusco.SitecoreHelixVisualStudioTemplates))

> ___
> NOTICE: This example only works in Visual Studio 2017 v15.6 or later)
> ___

## Introduction

The example utilize a Directory.Build.props and a Directory.Build.target file on solution level to load common properties and targets.

By separating the msbuild setup from the .csproj files these only contain project related setup and deviations from the solution-wide setup.


### Automatic publishing from Visual Studio

Publishing is controlled through properties set in [Publish.Properties.props](https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/props/Build.Properties.props)

```xml
    <!-- PublishRootDirectory 
            Your Sitecore website root directory, used for publishing and xml transformations 
              Note: Override this property in .csproj or local Directory.Build.props file for specific publish targets -->
    <PublishRootDirectory>C:\Websites\Sitecore.Solution\Website</PublishRootDirectory>

    <!-- AutoPublishOnBuild
          Toogles automatic-publishing on builds, including from VS 
          see ./build/targets/Website.AutoPublish.targets for details -->
    <AutoPublishOnBuild>true</AutoPublishOnBuild>

    <!-- RunXmlTransformsOnPublish
          Toggles transforming xml files post publishing, 
          see ./build/targets/Website.TransformXml.targets for details -->
    <RunXmlTransformsOnPublish>true</RunXmlTransformsOnPublish>
```

To override these publishing settings locally you can create a `Publish.Properties.props.user` file in /build/props

Please look at these files for details on the solution setup:

- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/src/Feature/DummyContent/website/Feature.DummyContent.csproj
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/Directory.Build.props
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/Directory.Build.targets
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/props/Build.Content.props
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/props/Build.PackageReferences.props
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/props/Build.Properties.props
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/props/Publish.Properties.props
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/targets/Website.AutoPublish.targets
- https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/targets/Website.TransformXml.targets

### Visual Studio Project and Solution template

Configure the VS extension to use the ./HelixTemplate folder as template directory or copy these to your template directory.  

When using the solution template, unzip the module template first (the extension does not support templates in a template for apparent reasons).

Upcoming release of the VS extension add relative path support for project templates, allowing you to easily keep these under source control per solution without having to change your settings.




## Useful Links and Resources

- [How to upgrade your solution .csproj files](https://natemcmaster.com/blog/2017/03/09/vs2015-to-vs2017-upgrade/) to the "new" format.
- [VS2017 Extension for working with project files](https://marketplace.visualstudio.com/items?itemName=ms-madsk.ProjectFileTools)
- [Sitecore Visual Studio Helix Templates extension Git repo](https://github.com/LaubPlusCo/helix-msbuild-example)