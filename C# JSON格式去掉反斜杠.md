# C# JSON格式去掉反斜杠

 原JSON字符串格式为（ReturnJson)： "\"[{\\\"KEY\\\":1,\\\"ID\\\":\\\"D001\\\",\\\"CL2\\\":\\\"TIM\\\",\\\"CL3\\\":\\\"CAT\\\",\\\"CL4\\\":\\\"TV\\\"},{\\\"KEY\\\":13,\\\"ID\\\":\\\"D445\\\",\\\"CL2\\\":\\\"WER\\\",\\\"CL3\\\":\\\"RT\\\",\\\"CL4\\\":\\\"EFG\\\"}]\""  

利用公式：

 ```c#
var json = new JsonSerializer().Deserialize(new JsonTextReader(new StringReader(ReturnJson)));
 ```

转换结果为：

[{"KEY":1,"ID":"D001","CL2":"TIM","CL3":"CAT","CL4":"TV"},{"KEY":13,"ID":"D445","CL2":"WER","CL3":"RT","CL4":"EFG"}] 