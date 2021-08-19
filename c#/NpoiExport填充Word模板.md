# NpoiExport填充Word模板

```c#
/// <summary>
        /// 输出模板docx文档(使用字典)
        /// </summary>
        /// <param name="tempFilePath">docx文件路径</param>
        /// <param name="outPath">输出文件路径</param>
        /// <param name="data">字典数据源</param>
        public static MemoryStream Export(string tempFilePath, string outPath, Dictionary<string, string> data)
        {
            using (FileStream stream = File.OpenRead(tempFilePath))
            {
                XWPFDocument doc = new XWPFDocument(stream);
                //遍历段落                  
                foreach (var para in doc.Paragraphs)
                {
                    ReplaceKey(para, data);
                }
                //遍历表格      
                foreach (var table in doc.Tables)
                {
                    foreach (var row in table.Rows)
                    {
                        foreach (var cell in row.GetTableCells())
                        {
                            foreach (var para in cell.Paragraphs)
                            {
                                ReplaceKey(para, data);
                            }
                        }
                    }
                }
                //写文件
                MemoryStream file = new MemoryStream();
                doc.Write(file);
                file.Seek(0, SeekOrigin.Begin);
                return file;
            }
        }
        private static void ReplaceKey(XWPFParagraph para, Dictionary<string, string> data)
        {
            string text = "";
            foreach (var run in para.Runs)
            {
                text = run.ToString();
                foreach (var key in data.Keys)
                {
                    //$$模板中数据占位符为$KEY$
                    if (text.Contains($"${key}$"))
                    {
                        text = text.Replace($"${key}$", data[key]);
                    }
                }
                run.SetText(text, 0);
            }
        }
```

