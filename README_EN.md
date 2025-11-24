# Natural (♮)

Natural: A ModSharp preview library.

This repository aims to preview packages that will be included in ModSharp's first-party offerings.

Feel free to try them out and submit feedback.

Since these packages are all in preview, they are not available via NuGet. If you need to use them, please compile manually and add DLL references yourself.

Example configuration:
```xml
	<ItemGroup>
		<Analyzer Include="..\..\lib\Sharp.Generator.Sdk.dll" />

		<Reference Include="Sharp.Shared">
			<HintPath>..\..\lib\Sharp.Shared.dll</HintPath>
			<Private>false</Private>
			<ExcludeAssets>runtime</ExcludeAssets>
		</Reference>
	</ItemGroup>
```
> You can feed this to GPT to help you with the configuration.
