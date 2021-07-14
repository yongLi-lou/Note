# Excel使用NPOI导出图片

其中引用到的命名空间：

using NPOI.HSSF.UserModel;
using NPOI.SS.UserModel;
using NPOI.SS.Util;
using NPOI.XSSF.UserModel;
using System;
using System.Data;
using System.IO;

下面是插入 图片的方法   

```c#
public static void InsertToPicture(string picPath,string fileExt)
        {
            FileStream fs = new FileStream("C:\\Users\\win 10\\Desktop\\导入图片.xls", FileMode.OpenOrCreate, FileAccess.ReadWrite);
            //create sheet
            HSSFWorkbook hssfworkbook = new NPOI.HSSF.UserModel.HSSFWorkbook();
            ISheet sheet = hssfworkbook.CreateSheet("Sheet1");
 
            IDrawing patriarch = sheet.CreateDrawingPatriarch();
 
            //将图片文件读入一个字符串
            byte[] bytes = File.ReadAllBytes(picPath);
 
            int pictureIdx = sheet.Workbook.AddPicture(bytes, PictureType.JPEG);
 
            IClientAnchor anchor = null;
            IPicture pict = null;
 
            int dx1 = 0, dy1 = 0, dx2 = 1023, dy2 = 255;
            //(row1,col1) 是指图片位置的左上角     (row2,col2) 是指图片位置的右下角
            int col1 = 1, row1 = 12, col2 = 2, row2 = 14;
   
 
            //anchor = patriarch.CreateAnchor(dx1, dy1, dx2, dy2, col1, row1, col2, row2);
            把图片插到相应的位置
            //pict = patriarch.CreatePicture(anchor, pictureIdx);
            if (fileExt == ".xls")
            {
                // 插图片的位置  HSSFClientAnchor（dx1,dy1,dx2,dy2,col1,row1,col2,row2) 
                anchor = new HSSFClientAnchor(dx1, dy1, dx2, dy2, col1, row1, col2, row2);
                //把图片插到相应的位置
                pict = (HSSFPicture)patriarch.CreatePicture(anchor, pictureIdx);
            }
            else if (fileExt == ".xlsx")
            {
                anchor = new XSSFClientAnchor(dx1, dy1, dx2, dy2, col1, row1, col2, row2);
                //把图片插到相应的位置
                pict = (XSSFPicture)patriarch.CreatePicture(anchor, pictureIdx);
            }
 
            //插入图片自适应大小  
            //pict.Resize();
 
            //写入数据流
            hssfworkbook.Write(fs);
            fs.Flush();
            fs.Close();
        }
 
调用上面的方法：
 
  Code.Excel.NPOIExcel.InsertToPicture(@"D:\1.jpg", ".xls");
```

