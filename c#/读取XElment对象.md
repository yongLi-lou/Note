# 读取XElment对象

```xml
<NewDataSet xmlns="">


		-<Table1 diffgr:hasChanges="inserted" msdata:rowOrder="0" diffgr:id="Table11">

			<MANDT>1</MANDT>

			<SPRAS>1</SPRAS>

			<MATKL>1</MATKL>

			<WGBEZ>1</WGBEZ>

			<WGBEZ60>1</WGBEZ60>

		</Table1>
    <Table1 diffgr:hasChanges="inserted" msdata:rowOrder="1" diffgr:id="Table12">

			<MANDT>1</MANDT>

			<SPRAS>1</SPRAS>

			<MATKL>1</MATKL>

			<WGBEZ>1</WGBEZ>

			<WGBEZ60>1</WGBEZ60>

		</Table1>

	</NewDataSet>
```

```c#
var data=element.Elements("NewDataSet").Elements()//这里获取到Table1
     foreach (var item in res)
            {
                var labal = item.Elements("WGBEZ").First().Value;
                var value = item.Elements("MATKL").First().Value;
                var WGBEZ60 = item.Elements("WGBEZ60").First().Value;
                

            }
```

