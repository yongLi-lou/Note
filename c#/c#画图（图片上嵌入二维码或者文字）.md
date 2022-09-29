# c#画图（图片上嵌入二维码或者文字）

1. 创建一个bitmap对象（bitmap就是画布）
2. 利用Graphics的 DrawImage方法在bitmap填充图片
3. 利用Graphics的 DrawString方法在bitmap填充文字
4. Graphics最后调用Flush（）
5. bitmap转MemoryStream

```c#
var bitmap = _iQRCode.GetQRCode("https://yingfuh5.yangche51.com/#/register?is_forget=0&recommender=" + _jwt.user_info.user_id, season.size.Value);

            var basePath = Path.GetDirectoryName(typeof(Program).Assembly.Location);
            //var img = Image.FromFile(basePath + "\\share.png");
           
            System.Net.WebRequest webreq = System.Net.WebRequest.Create(season.post_url);
            System.Net.WebResponse webres = webreq.GetResponse();
            System.IO.Stream stream = webres.GetResponseStream();
            
                var img = Image.FromStream(stream);
            
            
            var bmp = new Bitmap(img.Width, img.Height, PixelFormat.Format32bppArgb);
            using (Graphics g = Graphics.FromImage(bmp))
            {
                g.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBicubic;
                g.SmoothingMode = System.Drawing.Drawing2D.SmoothingMode.HighQuality;
                g.CompositingQuality = System.Drawing.Drawing2D.CompositingQuality.HighQuality;
                g.DrawImage(img, 0, 0,bmp.Width,bmp.Height);
            }
            var gr = Graphics.FromImage(bmp);
            imageHelp.AddLocationImg(ref gr,(float)season.x, (float)season.y, bitmap);
Font myFont = new Font("微软雅黑", 30, FontStyle.Bold);
            Brush bush = new SolidBrush(ColorTranslator.FromHtml("#000000"));//填充的颜色
            gr.DrawString("推荐人ID:" + _jwt.user_info.user_id, myFont, bush, 350, 50);
            gr.Flush();
           

            MemoryStream ms = new MemoryStream();
            bmp.Save(ms, ImageFormat.Jpeg);
            ms.Seek(0, SeekOrigin.Begin);

public class imageHelp
    {
        public static void AddLocationImg(ref Graphics g, float x, float y, Image QRcodePic)
        {
            //在背景图片上插入定位图片
            g.DrawImage(QRcodePic, x, y, QRcodePic.Width, QRcodePic.Height);
        }
    }
```

