## 创建模板配置

模板在 .NET 中通过模板根目录中的特殊文件夹和配置文件进行识别。 在本教程中，你的模板文件夹位于 working\templates\consoleasync 。

创建模板时，除特殊配置文件夹外，模板文件夹中的所有文件和文件夹都作为模板的一部分包含在内。 此配置文件夹名为“.template.config” 。

首先，创建一个名为“.template.config” 的新子文件夹，然后进入该文件夹。 然后，创建一个名为“template.json” 的新文件。 文件夹结构应如下所示。

控制台复制

```console
working
└───templates
    └───consoleasync
        └───.template.config
                template.json
```

使用你喜爱的文本编辑器打开 template.json 并粘贴以下 json 代码，然后保存。

JSON复制

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Me",
  "classifications": [ "Common", "Console" ],
  "identity": "ExampleTemplate.AsyncProject",
  "name": "Example templates: async project",
  "shortName": "consoleasync",
  "tags": {
    "language": "C#",
    "type": "project"
  }
}
```

此配置文件包含模板的所有设置。 可以看到基本设置，例如 `name` 和 `shortName`，除此之外，还有一个设置为 `project` 的 `tags/type` 值。 这会将模板指定为项目模板。 你创建的模板类型不存在限制。 `item` 和 `project` 值是 .NET 建议使用的通用名称，便于用户轻松筛选正在搜索的模板类型。

`classifications` 项表示你在运行 `dotnet new` 并获取模板列表时看到的“标记” 列。 用户还可以根据分类标记进行搜索。 不要将 json 文件中的 `tags` 属性与 `classifications` 标记列表混淆。 它们虽然具有类似的名称，但截然不同。 template.json 文件的完整架构位于 [JSON 架构存储](http://json.schemastore.org/template)。 有关 template.json 文件的详细信息，请参阅 [dotnet 创建模板 wiki](https://github.com/dotnet/templating/wiki)。

现在你已有一个有效的 .template.config/template.json 文件，可以安装模板了。 在安装模板之前，请务必删除无需在模板中包含的任何额外文件夹和文件，例如 bin 或 obj 文件夹。 在终端中，导航到 consoleasync 文件夹，并运行 `dotnet new install .\` 以安装位于当前文件夹的模板。 如果使用的是 Linux 或 macOS 操作系统，请使用正斜杠：`dotnet new install ./`。

.NET CLI复制

```dotnetcli
dotnet new install .\
```

此命令输出安装的模板列表，其中应包括你的模板。

控制台复制

```console
The following template packages will be installed:
   <root path>\working\templates\consoleasync

Success: <root path>\working\templates\consoleasync installed the following templates:
Templates                                         Short Name               Language          Tags
--------------------------------------------      -------------------      ------------      ----------------------
Example templates: async project                  consoleasync             [C#]              Common/Console
```

