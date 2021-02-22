# C# BitmapImage对象和byte[]之间的互转、BitmapImage和Bitmap互换

```c#
 /// <summary>
        /// byte[]转为BitmapImage
        /// </summary>
        /// <param name="byteArray"></param>
        /// <returns></returns>
        public static BitmapImage ToImage(byte[] byteArray)
        {
            BitmapImage bmp = null;
 
            try
            {
                bmp = new BitmapImage();
                bmp.BeginInit();
                bmp.StreamSource = new MemoryStream(byteArray);
                bmp.EndInit();
            }
            catch
            {
                bmp = null;
            }
 
            return bmp;
        }
 
        /// <summary>
        /// BitmapImage转为byte[]
        /// </summary>
        /// <param name="bmp"></param>
        /// <returns></returns>
        public static byte[] ToByteArray(BitmapImage bmp)
        {
            byte[] ByteArray = null;
 
            try
            {
                Stream stream = bmp.StreamSource;
                if (stream != null && stream.Length > 0)
                {
                    stream.Position = 0;
                    using (BinaryReader br = new BinaryReader(stream))
                    {
                        ByteArray = br.ReadBytes((int)stream.Length);
                    }
                }
            }
            catch
            {
                return null;
            }
 
            return ByteArray;
        }
```

将Bitmap转换成BitmapImage对象：

```c#

public BitmapImage ConvertBitmapToBitmapImage(Bitmap bitmap)
{
    MemoryStream stream = new MemoryStream();
    bitmap.Save(stream, ImageFormat.Bmp);
    BitmapImage image = new BitmapImage();
    image.BeginInit();
    image.StreamSource = stream;
    image.EndInit();
    return image;
}
```

```c#
 public static System.Drawing.Bitmap BitmapImage2Bitmap(BitmapImage bitmapImage)
        {
            using (System.IO.MemoryStream outStream = new System.IO.MemoryStream())
            {
                BitmapEncoder enc = new BmpBitmapEncoder();
                enc.Frames.Add(BitmapFrame.Create(bitmapImage));
                enc.Save(outStream);
                System.Drawing.Bitmap bitmap = new System.Drawing.Bitmap(outStream);
 
                return bitmap; 
            }
 
        }

```

