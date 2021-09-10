# net core中获取用户请求ip地址

方法一：通过注入来获取

先添加一个依赖注入

   services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
在控制器里边使用构造函数注入

   private readonly IHttpContextAccessor _httpContextAccessor;
   public TestController( IHttpContextAccessor httpContextAccessor) 
    {
            _httpContextAccessor = httpContextAccessor;
   }
获取

 //获取ip地址
 string ipaddress = _httpContextAccessor.HttpContext.Connection.RemoteIpAddress.ToString();


方法二：直接获取

 public void OnActionExecuting(ActionExecutingContext context)
 {
    //获取ip地址
    string ipaddress = context.HttpContext.Connection.RemoteIpAddress.ToString();
 }


但是这两种写法，使用了nginx后是无法访问的

使用了nginx后无法获取ip问题：

https://blog.csdn.net/qq_34897745/article/details/106093714