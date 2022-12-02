# xml文件基本格式及利用C#读取xml文件

**1）xml格式**
[xml](https://so.csdn.net/so/search?q=xml&spm=1001.2101.3001.7020)文件全称为extensible markup language，可扩展标记语言。本文以下面这个xml文件来说明xml文件格式。

```xml
<?xml version="1.0" encoding="utf-8"?> 
<root>  
   <person name="WangYao" age="25"/>               
   <person name="Jobs" age="56"/>  
</root>
```

xml文件的第一行为声明，声明由“版本”和“编码格式”两部分组成。上述xml文件符合xml1.0规范，编码格式为“utf-8”。

起始标签和结束标签总是成对出现的，也可以使用简化写法，即“/>”，例如<part/>，xml解析器会翻译成<part> </part>，本文所举的xml文件就采用了简化写法。

所有的属性都必须在值的两侧加上单引号或者双引号。空元素可以简写在一对尖括号里面，例如<part> </part>或者<part/>。

一个元素可以有一个或者多个属性，如下所示。

```xml
<元素名 属性名="属性值" />
<元素名 属性名1="属性值1" 属性名2="属性值2" />
```

xml文件的注释采用<!--注释-->格式，xml文件声明之前不能有注释。

**2）C#读取xml文件**

C#代码如下。

```c#
// 添加引用
using System.Xml;
 
// 定义一个结构体
public struct Student
{
  public string Name;
  public string Age;
}
 
// 定义一个链表
public List<Student> Datas = new List<Student>();
 
 
// 按钮的消息响应函数
private void button1_Click(object sender, EventArgs e)
{
  XmlDocument doc = new XmlDocument();
  doc.Load("config.xml");    //加载Xml文件  
  XmlElement rootElem = doc.DocumentElement;   //获取根节点  
  XmlNodeList personNodes = rootElem.GetElementsByTagName("person"); //获取person子节点集合 
            
  // 遍历节点读取xml里面的数据             
  foreach (XmlNode node in personNodes)
  {
    Student studentData = new Student();
    studentData.Name = node.Attributes["name"].Value;
    studentData.Age = node.Attributes["age"].Value;
    Datas.Add(studentData); 
  }
}
```

