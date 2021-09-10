# netcore日志中间件

```c#
public class LogMiddleware
    {
        private readonly RequestDelegate _next;
        protected static NLog.Logger log = NLog.LogManager.GetCurrentClassLogger();
        private SortedDictionary<string, object> _data;
        private Stopwatch _stopwatch;
        public LogMiddleware(RequestDelegate next)
        {
            _next = next;
            _stopwatch = new Stopwatch();
            
        }

        public async Task Invoke(HttpContext context)
        {
            _stopwatch.Restart();
            _data = new SortedDictionary<string, object>();

            HttpRequest request = context.Request;
            _data.Add("request.url", string.Format(request.Path.ToString()));
            _data.Add("request.headers", request.Headers.ToDictionary(x => x.Key, v => string.Join(";", v.Value.ToList())));
            _data.Add("request.method", request.Method);
            _data.Add("request.executeStartTime", DateTimeOffset.Now.ToString("yyyy-MM-dd HH:mm:ss.fff"));

            // 获取请求body内容
            //body里是否有值
            if (request.ContentLength > 0)
            {
                request.EnableBuffering();

                Stream stream = request.Body;
                byte[] buffer = new byte[request.ContentLength.Value];
                stream.Read(buffer, 0, buffer.Length);
                _data.Add("request.body", Encoding.UTF8.GetString(buffer));

                request.Body.Position = 0;
            }
            //url里是否有值
            if (request.QueryString.HasValue)
            {
                _data.Add("request.QueryString", request.QueryString.Value);
            }

            // 获取Response.Body内容
            var originalBodyStream = context.Response.Body;

            using (var responseBody = new MemoryStream())
            {
                context.Response.Body = responseBody;

                await _next(context);

                _data.Add("response.body", await GetResponse(context.Response));
                _data.Add("response.executeEndTime", DateTimeOffset.Now.ToString("yyyy-MM-dd HH:mm:ss.fff"));

                await responseBody.CopyToAsync(originalBodyStream);
            }

            // 响应完成记录时间和存入日志
            context.Response.OnCompleted(() =>
            {
                _stopwatch.Stop();
                _data.Add("elaspedTime", _stopwatch.ElapsedMilliseconds + "ms");
                var json = JsonConvert.SerializeObject(_data);
                /* var logEvent = new LogEventInfo();
                 logEvent.Level = NLog.LogLevel.Info;
                 logEvent.Message = JsonConvert.SerializeObject(json, Formatting.Indented);*/
                
                log.Debug(json, "api", request.Method.ToUpper());

                
                return Task.CompletedTask;
            });
        }
        /// <summary>
        /// 获取响应内容
        /// </summary>
        /// <param name="response"></param>
        /// <returns></returns>
        public async Task<string> GetResponse(HttpResponse response)
        {
            response.Body.Seek(0, SeekOrigin.Begin);
            var text = await new StreamReader(response.Body).ReadToEndAsync();
            response.Body.Seek(0, SeekOrigin.Begin);
            return text;
        }
    }


//拓展方法
public static class MyMiddlewareExtensions
    {
        public static IApplicationBuilder UseLogMiddleware(this IApplicationBuilder builder)
        {
            return builder.UseMiddleware<LogMiddleware>();
        }
    }








//startup
app.UseLogMiddleware();//使用自定义中间件
```

