# c#用正则表达式获取一个字符串对应的值

 例 ：1001|001|111 中的111

可以使用正则表达式 `(\d{3})$`，其中：

- `\d` 表示一个数字字符
- `{3}` 表示前面的表达式匹配 3 次，即匹配三个数字字符
- `()` 表示捕获分组，将匹配到的内容存储到匹配对象的一个分组中
- `$` 表示匹配到输入字符串的结尾

因此，`(\d{3})$` 匹配输入字符串的最后三个数字字符，并将其捕获到一个分组中。

```c#
using System;
using System.Text.RegularExpressions;

class Program
{
    static void Main()
    {
        string input = "1001|001|111";
        string pattern = @"(\d{3})$";

        Match match = Regex.Match(input, pattern);

        if (match.Success)
        {
            string value = match.Groups[1].Value;
            Console.WriteLine(value); // 输出 "111"
        }
    }
}

```

