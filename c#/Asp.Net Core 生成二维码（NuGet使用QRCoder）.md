# [Asp.Net Core 生成二维码（NuGet使用QRCoder）](https://www.cnblogs.com/OneManStep/p/11365701.html)

**前言**

功能：调用web api 接口

1.获取 jpeg 格式的二维码 

2.获取中间带有logo 的二维码

\3. 下载 jpeg,svg 格式的二维码

需要的NuGet 包：

\> QRCoder(v1.3.6)

\> System.Drawing.Common(v4.5.1)

 

**正文**

**1. 准备项目**

创建ASP.NET Core Web Api 应用程序，添加上边说的两个包，并创建Services 文件夹，Services 文件夹中的类如下：

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190816171037001-1953391555.png)

 

**2. 功能：生成jpeg 格式 二维码，通过Api 来请求**

在 IQRCodeService 中添加定义的方法，返回的类型为Bitmap,引用Stytem.Drawing

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

using System.Drawing;

namespace QRCode.Api.Services.Interfaces
{
public interface IQRCodeService
{
　　Bitmap GetQRCode(string url, int pixel);

}
}

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

在QRCodeService 中继承 IQRCodeService接口，实现 GetQRCode 方法。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
using QRCode.Api.Services.Interfaces;
using QRCoder;
using System.Drawing;

namespace QRCode.Api.Services
{
    public class QRCodeService : IQRCodeService
    {
        #region  QRCode

        public Bitmap GetQRCode(string plainText, int pixel)
        {
            var generator = new QRCodeGenerator();
            var qrCodeData = generator.CreateQrCode(plainText, QRCodeGenerator.ECCLevel.Q);
            var qrCode = new QRCoder.QRCode(qrCodeData);

            var bitmap = qrCode.GetGraphic(pixel);

            return bitmap;
        }
        #endregion
    }
}    
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

上图： plainText 参数指的是 扫描二维码时显示的文本内容，pixel 参数指的是 像素

ECCLevel.Q 参数是 指：纠错程度，(The error correction level. Either L (7%), M (15%), Q (25%) or H (30%). Tells how much of the QR Code can get corrupted before the code isn't readable any longer.)

在Startup 中的ConfiguraServices 注入依赖

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public void ConfigureServices(IServiceCollection services)
        {
            services.AddTransient<IQRCodeService, QRCodeService>();

            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
        }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

在Controller 类中注入QRCodeservice 依赖，使用get 的请求方式，请求参数为plainText, pixel

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
　　　　 private readonly IQRCodeService _qrCode;　　　　 public ValuesController(IQRCodeService qrCode)
        {
            _qrCode = qrCode;
        }
        [HttpGet("qrCode")]
        public IActionResult Get(string plainText, int pixel)
        {
            if (string.IsNullOrEmpty(plainText))
            {
                return BadRequest("parameter is null");
            }
            if (pixel <= 0)
            {
                return BadRequest("pixel <= 0");
            }

            var bitmap = _qrCode.GetQRCode(plainText, pixel);
            var ms = new MemoryStream();
            bitmap.Save(ms, ImageFormat.Jpeg);

            return File(ms.GetBuffer(), "image/jpeg");
        }    
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

现在 运行代码 请求url:[https://localhost:44313/api/values/qrcode?plainText=there%20is%20qrcode&pixel=10](https://localhost:44313/api/values/qrcode?plainText=there is qrcode&pixel=10)

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190816175355716-629324684.png)

使用微信扫一扫的结果：显示的效果就是纯文字，如果plainText =https://www.········是一个网址，会自动打开这个网址

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190816175722066-1790163055.png)

 

 

如下图，从元数据中可以看出CreateQrCode方法 有多个重载，而实现Payload参数有多个载体，比如说Bookmark,Url,PhoneNumber,SMS,WIFI 等等 还有[更多载体](https://github.com/codebude/QRCoder/wiki/Advanced-usage---Payload-generators)

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190816212350408-280201860.png)

 

如下图：现在来使用WIFI 的载体应用一下，看一下效果

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190816181617565-2097725120.png)

在Controller 类中添加GetWIFIQRCode() , 我们还是调用QRCodeService类中的GetQRCode方法,因为上图中WIFI类重写了toString方法，我们直接使用ToString() 转换成plainText 这个参数

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        [HttpGet("wifi")]
         public IActionResult GetWIFIQRCode(int pixel)
        {
            if (pixel <= 0)
            {
                return BadRequest("pixel <= 0");
            }

            var payload = new WiFi("ssid","password",WiFi.Authentication.WPA);
            var bitmap = _qrCode.GetQRCode(payload.ToString(), pixel); // 还是调用QRCodeService 中的GetQRCode方法，把 Payload 载体换成string类型。
            var ms = new MemoryStream();
            bitmap.Save(ms, ImageFormat.Jpeg);

            return File(ms.GetBuffer(), "image/jpeg");
        }    
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

直接运行代码，二维码就不贴出来了，直接看扫描出来的截图：很明显，还真是tostring(), emmmm

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190816182234577-493993959.png)

 

 

**3 功能：在二维码中间加入头像(logo/image)**

跟上边步骤差不多，我直接贴代码，，，

在IQRCodeService 接口类中添加GetQRCodeWithLogo方法定义，如下代码

```
Bitmap GetQRCodeWithLogo(string plainText, int pixel, string logoPath);
```

在QRCodeService类中实现这个方法，这里多了一个logoPath参数，指的是添加的这个头像的路径

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        public Bitmap GetQRCodeWithLogo(string plainText, int pixel, string logoPath)
        {
            var generator = new QRCodeGenerator();
            var qrCodeData = generator.CreateQrCode(plainText, QRCodeGenerator.ECCLevel.Q);
            var qrCode = new QRCoder.QRCode(qrCodeData);

            var bitmap = qrCode.GetGraphic(pixel, Color.Black, Color.White, (Bitmap)Image.FromFile(logoPath), 15, 8);

            return bitmap;
        }    
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

上图中GetGraphic方法中有许多的参数， 

pixel 指的是像素，(Color.Black, Color.white 这两个参数看上边二维码图片就能知道 两个参数代表哪个区域的颜色)，下一个参数就是logo 图片 格式是Bitmap类型，后边两个参数分别指的是logo占二维码的百分比，范围是1-99，默认15，最后一个参数是 logo 边框宽度，整数类型，默认为6

当然这个GetGraphic方法还有很多重载，可以F12看元定义，也可以在这里[查看更多重载定义](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers)

 

接下来在Controller 类中添加get请求，内容跟之前大致一样，我使用的图片是直接读取的物理路径。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
         [HttpGet("logo")]
        public IActionResult GetQRCodeWithLogo(string plainText, int pixel)
        {

            if (string.IsNullOrEmpty(plainText))
            {
                return BadRequest("parameter is null");
            }
            if (pixel <= 0)
            {
                return BadRequest("pixel <= 0");
            }

            var logoPath = @"E:\EFCore\QRCode.Api\QRCode.Api\0000_2.jpg";
            var bitmap = _qrCode.GetQRCodeWithLogo(plainText, pixel, logoPath);
            var ms = new MemoryStream();
            bitmap.Save(ms, ImageFormat.Jpeg);

            return File(ms.GetBuffer(), "image/jpeg");
        }    
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

运行代码，请求url:[https://localhost:44313/api/values/logo?plainText=%20there%20is%20qrcode&pixel=20](https://localhost:44313/api/values/logo?plainText= there is qrcode&pixel=20)

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190817120454101-194109114.png)

 

QRCoder 还提供了很多不同用途的接口，可以生成不同用途的二维码，比如说svg格式，为Postscript打印机使用，打印出pdf等等，[了解更多用途](https://github.com/codebude/QRCoder/wiki/Advanced-usage---QR-Code-renderers)

 

**4 功能：生成svg格式的矢量二维码，并下载下来**

代码跟上边步骤相同，在IQRCodeService接口类中定义方法，在QRCodeService中实现GetSvgQRCode方法，参数相同，不同的是用的SvgQRCode实例，返回的是string类型。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        public string GetSvgQRCode(string plainText, int pixel)
        {
            var generator = new QRCodeGenerator();
            var qrCodeData = generator.CreateQrCode(plainText, QRCodeGenerator.ECCLevel.Q);
            var qrCode = new SvgQRCode(qrCodeData);

            return qrCode.GetGraphic(pixel);
        }    
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

在Controller 中添加get请求，相同的参数，保存svg到项目中，然后提供svg格式的下载

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
        [HttpGet("svg")]
        public IActionResult GetSvgQRCode(string plainText, int pixel)
        {
            if (string.IsNullOrEmpty(plainText))
            {
                return BadRequest("parameter is null");
            }
            if (pixel <= 0)
            {
                return BadRequest("pixel <= 0");
            }

            var svgQrCode = _qrCode.GetSvgQRCode(plainText, pixel);

            var rootPath = _hostingEnvironment.ContentRootPath;
            var svgName = "svgQRCode.svg";
            System.IO.File.WriteAllText($@"{rootPath}\{svgName}", svgQrCode);

            var readByte = System.IO.File.ReadAllBytes($@"{rootPath}\{svgName}");

            return File(readByte, "image/svg", svgName);
        }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

运行代码，请求url, 可以看到浏览器已经下载下来, 通过浏览器是可以打开这个svg 格式二维码。

![img](https://img2018.cnblogs.com/blog/1437738/201908/1437738-20190817122757203-1761060132.png)

 

我这里就写了这两三个例子，看着也很简单，这个QRCoder包使用轻便，还有很多不同的用途的，不同格式的用法，更多还请查看他们的使用文档：https://github.com/codebude/QRCoder/wiki