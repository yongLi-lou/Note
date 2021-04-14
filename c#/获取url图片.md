# 获取url图片



```c#
System.Net.WebRequest webreq = System.Net.WebRequest.Create(logoPath);
                            System.Net.WebResponse webres = webreq.GetResponse();
                            using (System.IO.Stream stream = webres.GetResponseStream())
                                 {
                                var bitmap =Image.FromStream(stream), 15, 8);
                                }
```

