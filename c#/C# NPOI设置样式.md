# C# NPOI设置样式

1.创建样式对象

```c#
ICellStyle style2 = hssfworkbook.CreateCellStyle();
            //设置单元格的样式：水平对齐居中
            style2.Alignment = HorizontalAlignment.CenterSelection;
            style2.VerticalAlignment = VerticalAlignment.Justify;
            //新建一个字体样式对象
            IFont font2 = hssfworkbook.CreateFont();
            //设置字体加粗样式
            font2.Boldweight = 5;
            //使用SetFont方法将字体样式添加到单元格样式中 
            font2.FontHeightInPoints = 15;
            //边框设置

            style2.BorderTop = BorderStyle.Thin;
            style2.BorderLeft = BorderStyle.Thin;
            style2.BorderRight = BorderStyle.Thin;
            style2.BorderBottom = BorderStyle.Thin;
            style2.SetFont(font2);
```

2. 设置样式到对应单元格

   ```c#
   rowHeader.GetCell(0).CellStyle = style;
   ```

* 设置相同单元格多种字体

  ```c#
  var rts1 = new HSSFRichTextString("龙港市国有资本运营有限公司\n资产维修申请审批表\n\n" + DateTime.Now.ToString("yyyy年MM月dd日"));
                  rts1.ApplyFont(0,24,font);
                  rts1.ApplyFont(25, 36, font2);
                  rowHeader.CreateCell(0).SetCellValue(rts1);
  ```

3. 设置行高和列宽

   ```c#
   rowHeader.Height = 80 * 30;//1/20个点，所以要想得到一个点的话，需要乘以20。
   sheet1.SetColumnWidth(0, 30 * 256);//因为这个参数的单位是1/256个字符宽度，所以要乘以256才是一整个字符宽度。
   ```

4. 合并单元格

   ```c#
   //合并单元格
     /**
       第一个参数：从第几行开始合并
       第二个参数：到第几行结束合并
       第三个参数：从第几列开始合并
       第四个参数：到第几列结束合并
   **/
    CellRangeAddress region = new CellRangeAddress(15, 15 , 1, 2);
    sheet.AddMergedRegion(region);

