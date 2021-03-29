# 使用 xUnit 编写 ASP.NET Core WebAPI单元测试

本文使用xUnit对ASP.NET Core WebAPI做单元测试，使用HttpClient的同步和异步请求，下面详细介绍xUnit的使用过程：

***\*一、创建示例项目\****

![img](https://img2018.cnblogs.com/blog/994830/201904/994830-20190405205537413-621107953.png)

模板为我们自动创建了一个ValuesController控制器，保留里面的一个Get请求和Post请求方法，代码如下：

```
    [Route("api/[controller]")]
    [ApiController]
    public class ValuesController : ControllerBase
    {
        // GET api/values/5
        [HttpGet("{id}")]
        public ActionResult<string> Get(int id)
        {
            return $"value:{id}";
        }

        // POST api/values
        [HttpPost]
        public ActionResult<string> Post(dynamic obj)
        {
            return $"姓名：{obj.Name},年龄：{obj.Age}";
        }
    }
```

使用.NET Core创建一个xUnit单元测试项目，如图：

![img](https://img2018.cnblogs.com/blog/994830/201904/994830-20190405210044843-1097005797.png)

项目的模板页已经为我们添加好了xUnit的引用，不需要我们手动去NuGet导入了，现在从NuGet中添加Microsoft.AspNetCore.App和Microsoft.AspNetCore.TestHost，xUnit项目并添加WebAPI的项目引用，如下图所示：

![img](https://img2018.cnblogs.com/blog/994830/201904/994830-20190405210522687-967110398.png)

**二、编写单元用例**

 写单元测试一般有三个步骤：Arrange，Act 和 Assert。

- **Arrange** 是准备阶段，这个阶段是准备工作，比如模拟数据、初始化对象等；
- **Act** 是行为阶段，这个阶段是用准备好的数据去调用要测试的方法；
- **Assert** 是断定阶段，就是把调用目标方法返回的值和预期的值进行比较，如果和预期一致说明测试通过，否则为失败。

 新建一个单元测试类：ValuesTest.cs；用于对ValuesController进行单元测试。

 1、使用HttpClient进行Get请求测试，单元测试代码如下：

```
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.TestHost;
using Newtonsoft.Json;
using System.Net;
using System.Net.Http;
using System.Net.Mime;
using System.Text;
using System.Threading.Tasks;
using Xunit;
using Xunit.Abstractions;

namespace WebAPI.xUnit
{
    public class ValuesTests
    {
        public ValuesTests(ITestOutputHelper outputHelper)
        {
            var server = new TestServer(WebHost.CreateDefaultBuilder()
                .UseStartup<Startup>());
            Client = server.CreateClient();            
        }

        public HttpClient Client { get; }
        
        [Fact]
        public async Task GetById_ShouldBe_Ok()
        {
            // Arrange
            var id = 1;

            // Act
            var response = await Client.GetAsync($"/api/values/{id}");

            // Assert
            Assert.Equal(HttpStatusCode.OK, response.StatusCode);
        }
    }
}
```

这里我们通过 TestServer 拿到一个 HttpClient 对象，用它我们可以模拟 Http 请求。我们写了一个非常简单的测试用例，完整演示了单元测试的 Arrange，Act 和 Assert 三个步骤。

 2、使用HttpClient进行Post请求测试，并用ITestOutputHelper输出请求信息，单元测试代码如下：

 

```
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.TestHost;
using Newtonsoft.Json;
using System.Net;
using System.Net.Http;
using System.Net.Mime;
using System.Text;
using System.Threading.Tasks;
using Xunit;
using Xunit.Abstractions;

namespace WebAPI.xUnit
{
    public class ValuesTests
    {
        public ValuesTests(ITestOutputHelper outputHelper)
        {
            var server = new TestServer(WebHost.CreateDefaultBuilder()
                .UseStartup<Startup>());
            Client = server.CreateClient();
            Output = outputHelper;
        }

        public HttpClient Client { get; }
        public ITestOutputHelper Output { get; }

        [Fact]
        public async Task Post_ShouldBe_OK()
        {
            var content = new StringContent(JsonConvert.SerializeObject(new { Name = "cxt", Age = 22 }), Encoding.UTF8, MediaTypeNames.Application.Json);

            var response = await Client.PostAsync("/api/values", content);

            // Output
            var responseTest = await response.Content.ReadAsStringAsync();

            Output.WriteLine(responseTest);

            Assert.Equal(HttpStatusCode.OK, response.StatusCode);
        }
    }
}
```

 3、运行测试用例，得到测试结果。

在当前的方法内，如GetById_ShouldBe_Ok()、Post_ShouldBe_OK()代码块内右键->运行测试，或者打开测试资源管理器，运行所选测试

![img](https://img2018.cnblogs.com/blog/994830/201904/994830-20190405212120942-289316390.png)

绿色的勾表示已经测试通过。

下面查看Post请求的Output打印结果：

![img](https://img2018.cnblogs.com/blog/994830/201904/994830-20190405212259473-1204723959.png)

 

 通过上面两个测试用例，发现使用起来超级方便。