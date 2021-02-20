# MemoryStream ReadTimeout”引发了“System.InvalidOperationException”

```c#
Image img = new Image();
                BitmapImage bi = new BitmapImage();
                bi.BeginInit();
                bi.StreamSource = imgInfo.ImgStream;
                bi.EndInit();
                img.Source = bi;
```

wpf多少用同一个MemoryStream初始化BitmapImage 时，引发异常：ReadTimeout”引发了“System.InvalidOperationException” 

只需将MemoryStream重新从头开始读取即可，是频繁读取流，而当前位置没有复位而已。



如下即可：

```c#
BitmapImage bi = new BitmapImage();
                bi.BeginInit();
                // 从头开始读取，否则重复使用，后面读取错误。
                imgInfo.ImgStream.Seek(0, SeekOrigin.Begin);
                bi.StreamSource = imgInfo.ImgStream;
                bi.EndInit();
                img.Source = bi;
```

