# NPOI合并单元格后导致边框不完整

```c#
((HSSFSheet)sheet1).SetEnclosedBorderOfRegion(new CellRangeAddress(0, 0, 0, 3), BorderStyle.Thin, NPOI.HSSF.Util.HSSFColor.Black.Index);
```

