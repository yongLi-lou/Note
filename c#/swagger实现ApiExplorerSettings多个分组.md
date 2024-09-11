# swagger实现ApiExplorerSettings多个分组

```c#
using System;

[AttributeUsage(AttributeTargets.Method, AllowMultiple = false)]  // 保证这是一个方法级别的特性
public class MultiGroupApiExplorerSettingsAttribute : Attribute
{
    public string[] GroupNames { get; set; }

    public MultiGroupApiExplorerSettingsAttribute(params string[] groupNames)
    {
        GroupNames = groupNames;
    }
}

```

program代码：

```c#
 // 修改 DocInclusionPredicate
    c.DocInclusionPredicate((docName, apiDesc) =>
    {
        // 先检查是否存在 MultiGroupApiExplorerSettingsAttribute
        var multiGroupAttribute = apiDesc.CustomAttributes().OfType<MultiGroupApiExplorerSettingsAttribute>().FirstOrDefault();
        if (multiGroupAttribute != null)
        {
            // 如果存在，则根据 GroupNames 进行匹配
            return multiGroupAttribute.GroupNames.Contains(docName);
        }

        // 如果没有 MultiGroupApiExplorerSettingsAttribute，检查默认的 ApiExplorerSettings
        var apiExplorerSettings = apiDesc.CustomAttributes().OfType<ApiExplorerSettingsAttribute>().FirstOrDefault();
        if (apiExplorerSettings != null)
        {
            return apiExplorerSettings.GroupName == docName;
        }

        // 如果没有找到任何特性，则默认返回 false，表示不包含在该文档中
        return false;
    });
```

配置

```c#
[MultiGroupApiExplorerSettings("finance", "order")]
```