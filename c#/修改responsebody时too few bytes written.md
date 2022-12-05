# 修改responsebody时too few bytes written 

代码:

```c#
//获取Response.Body内容
            var originalBodyStream = context.Response.Body;
            

                /*using (var responseBody = new MemoryStream())
                {*/
                var responseBody = new MemoryStream();
                    context.Response.Body = responseBody;

                    await _next(context);
            
                    var str = await GetResponse(context.Response);

                    var json = JsonConvert.DeserializeObject<Dictionary<string, object>>(str);
            if (json["code"].ToString() == "0")
            {


                var msg = json["msg"].ToString();
                var tran = db.FirstOrDefault(e => e.cn == msg);
                if (language != "cn")
                {
                    json["msg"] = tran.GetType().GetProperty(language).GetValue(tran);
                }


                var str_arr = Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(json));
                using (var str_stream = new MemoryStream(str_arr))
                {
                    

                    await str_stream.CopyToAsync(originalBodyStream,(int)responseBody.Length);
                }



            }
            else {
                await responseBody.CopyToAsync(originalBodyStream);
            }
            responseBody.Close();
                responseBody.Dispose();




 /// <summary>
        /// 获取响应内容
        /// </summary>
        /// <param name="response"></param>
        /// <returns></returns>
        public async Task<string> GetResponse(HttpResponse response)
        {
            response.Body.Seek(0, SeekOrigin.Begin);
            var reader = new StreamReader(response.Body);
            
                var text = await reader.ReadToEndAsync();
                response.Body.Seek(0, SeekOrigin.Begin);
            
           
                return text;
            
            
            
            
        }
    }
```



报错：Microsoft.AspNetCore.Server.Kestrel[13]
      Connection id "0HMMMCKQ13EEL", Request id "0HMMMCKQ13EEL:00000002": An unhandled exception was thrown by the application.
      System.InvalidOperationException: Response Content-Length mismatch: too few bytes written (546 of 552).

原因：原来的流大小是552修改完之后是546所以会报错

解决：规定长度

```c#
context.Response.ContentLength = str_arr.Length;
```

