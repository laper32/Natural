# Natural (♮)

Natural: A ModSharp preview library.

本仓库旨在于预览即将进入ModSharp第一方的包。

请尽情试用，并提交反馈。

由于这些包皆为预览，故不提供Nuget方式。如果你需要使用，请手动编译并自行添加dll引用。
示例如下：
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
> 你可以喂给GPT让它帮你做。