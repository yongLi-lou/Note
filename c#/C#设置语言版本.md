# C#设置语言版本

1. 在Visual Studio中，右键单击项目文件（.csproj文件），然后选择“编辑.csproj”。
2. 在.csproj文件中，添加以下行，将项目的语言版本设置为C# 9.0：

```
<PropertyGroup>
  <LangVersion>9.0</LangVersion>
</PropertyGroup>
```

1. 保存.csproj文件，然后重新生成项目。