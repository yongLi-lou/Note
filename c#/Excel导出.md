# Excel导出

```c#
public MemoryStream ExportFile()
        {
            HSSFWorkbook hssfworkbook = new HSSFWorkbook();

            ISheet sheet1 = hssfworkbook.CreateSheet(SheetName);
            IRow rowHeader = sheet1.CreateRow(0);

            if (ExportData != null)//datatable类型
            {
                int columnIndex = 0;
                foreach (DataColumn column in ExportData.Columns)
                {
                    rowHeader.CreateCell(columnIndex).SetCellValue(column.Caption);
                    columnIndex++;
                }

                if (ExportData.Rows.Count > 0)
                {
                    int rowIndex = 1;
                    foreach (DataRow rowData in ExportData.Rows)
                    {
                        IRow row = sheet1.CreateRow(rowIndex);

                        for (int i = 0; i < columnIndex; i++)
                        {
                            row.CreateCell(i).SetCellValue(rowData[i].FormatToString());
                        }

                        rowIndex++;
                    }
                }

                for (int i = 0; i < columnIndex; i++)
                    sheet1.AutoSizeColumn(i);
            }

            MemoryStream file = new MemoryStream();
            hssfworkbook.Write(file);
            file.Seek(0, SeekOrigin.Begin);
            return file;
```

```c#
var ms = exportExcel.ExportFile();

                byte[] buffer = new byte[1024 * 2];
                buffer = ms.GetBuffer();
                return File(buffer, "application/ms-excel", $"StudyOut{DateTime.Now.ToString("yyyyMMddHHmmss")}.xlsx");//FileContentResult
```

