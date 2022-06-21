# 修改appsetting节点值

```c#
var connstr = "";
var path = AppDomain.CurrentDomain.BaseDirectory + "\\appsettings.json";
                        StreamReader streamReader = File.OpenText(path);
                        using (JsonTextReader jsonTextReader = new JsonTextReader(streamReader))
                        {
                            JObject jsonObject = (JObject)JToken.ReadFrom(jsonTextReader);
                            jsonObject["FreeSql"]["ConnStr"] = connstr;
                            streamReader.Close();
                            string output = JsonConvert.SerializeObject(jsonObject, Formatting.Indented);
                            File.WriteAllText(path, output);
                        }
```

