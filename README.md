
# Sitecore Helix Solution MS Build Example  

This solution shows how you can use msbuild and the "new" dotnet project format in a Sitecore Helix solution.  

No Gulp, Grunt, Cake, nant or other task runner beside msbuild. No custom build tasks or required nuget references.  

## The example features

- Automatic publish on builds from Visual Studio
- XML tranformation for config files and other xml on publish
- No dependencies on other than Visual Studio standard build targets
- PackageReferences for dependencies
  - Example on including content files from PackageReferences, see Foundation.Serialization module
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

### Controlling what is published

The Content item group that control the project file content is configured in [./build/props/Build.Content.props](https://github.com/LaubPlusCo/helix-msbuild-example/blob/master/build/props/Build.Content.props)

Globs are used in this file to include common ressource files and exclude other.

For your solution you may need to add font ressource files, other image types or similar. These files can also be individually included as Content in a project by modifying the "Build Action" property on the file in Visual Studio or manually edit the .csproj file.

To prevent common packagereferences from being copied to the output directory and included in the publish, a custom target called RemovePrivatePackageReference is run after the ResolveReference target.

This target look for PrivateAssets = all on the packagereferences and stop any assemblies from these to be copied local. This does not prevent other packagereferences where assets are not set to be private, from copying their transient inherited dependencies so please note that system or sitecore assemblies can originate from other than direct packagereferences. If this become an issue, simply add a new target that deletes any such unwanted inherited assemblies from the temporary publish location AfterTarget="CopyAllFilesToSingleFolderForPackage"

### More details

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

### Website folder name convention

You may notice that the project root folder in this example is named `website` rather than `code`.

See [this pr from Rob Erlam](https://github.com/Sitecore/Helix.Docs/pull/15/files/a194b50dc59e01c8967f29079f9a8381043bdc98#diff-7e720abd1441590c56b5f15a190e9388) to the Helix docs. Naming the project root folder according to responsibility area in the solution - or really the publish target - makes much more sense than using the generic term `code`. I hope this pull request is accepted soon.

For existing solutions that follow the old convention of naming the folder `code` should of course stick to this until it make sense to change this. There are no buildtime dependencies on the naming of folders in this example as was the case with the Gulp tasks shown in the Habitat example.

The HelixTemplates included in this example use a replacement token for the folder name. Adjust the template manifest and template files to match your solution needs.

## Useful Links and Resources

- [How to upgrade your solution .csproj files](https://natemcmaster.com/blog/2017/03/09/vs2015-to-vs2017-upgrade/) to the "new" format.
- [VS2017 Extension for working with project files](https://marketplace.visualstudio.com/items?itemName=ms-madsk.ProjectFileTools)
- [Sitecore Visual Studio Helix Templates extension Git repo](https://github.com/LaubPlusCo/helix-msbuild-example)

# PLEASE CONTRIBUTE OR COMMENT

Please contribute to this example, comment on the approach, share your feelings..