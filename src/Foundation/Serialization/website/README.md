# Foundation.Serialization

This module is included in the example to show how you can publish the content from PackageReferences.

The trick is to add the nuget content folder manually in the .csproj files as project content.

```xml
  <ItemGroup>
    <Content Include="$(NuGetPackageRoot)\Unicorn\4.0.8\content\**\*">
      <CopyToOutputDirectory>Never</CopyToOutputDirectory>
      <CopyToPublishDirectory>Always</CopyToPublishDirectory>
    </Content>
  </ItemGroup>
```

Notice the CopyToOutputDirectory Never - this ensures that the content folder from the nuget is not copied to bin\