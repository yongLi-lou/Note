# JWT NETCore



chrome自带base64解密方法：atob（‘’）

### 三种方法获取token内容

1. ```c#
   //自带方法
   private static JwtSecurityTokenHandler handler { get; set; }
   handler.ValidateToken(encodedJwt, validationParameters, out securityToken);
   ```

2. ```c#
   //User是ClaimsPrincipal
   var sub=User.FindFirst(e=>e.Type=='')?.Value
   ```

3. ```c#
   //注入IHttpContextAccessor
   var claims=_accessor.HttpContext.claims
   var claimsvalues=(from item in claims select item.Value).ToList()
   ```

### 生成Token

```c#
public static string IssueJwt(TokenModelJwt tokenModel)
 {
     string iss = Appsettings.app(new string[] { "Audience", "Issuer" });
     string aud = Appsettings.app(new string[] { "Audience", "Audience" });
     string secret = AppSecretConfig.Audience_Secret_String;

     //var claims = new Claim[] //old
     var claims = new List<Claim>
         {
          /*
          * 特别重要：
            1、这里将用户的部分信息，比如 uid 存到了Claim 中，如果你想知道如何在其他地方将这个 uid从 Token 中取出来，请看下边的SerializeJwt() 方法，或者在整个解决方案，搜索这个方法，看哪里使用了！
            2、你也可以研究下 HttpContext.User.Claims ，具体的你可以看看 Policys/PermissionHandler.cs 类中是如何使用的。
          */

             

         new Claim(JwtRegisteredClaimNames.Jti, tokenModel.Uid.ToString()),
         new Claim(JwtRegisteredClaimNames.Iat, $"{new DateTimeOffset(DateTime.Now).ToUnixTimeSeconds()}"),
         new Claim(JwtRegisteredClaimNames.Nbf,$"{new DateTimeOffset(DateTime.Now).ToUnixTimeSeconds()}") ,
         //这个就是过期时间，目前是过期1000秒，可自定义，注意JWT有自己的缓冲过期时间
         new Claim (JwtRegisteredClaimNames.Exp,$"{new DateTimeOffset(DateTime.Now.AddSeconds(1000)).ToUnixTimeSeconds()}"),
         new Claim(ClaimTypes.Expiration, DateTime.Now.AddSeconds(1000).ToString()),
         new Claim(JwtRegisteredClaimNames.Iss,iss),
         new Claim(JwtRegisteredClaimNames.Aud,aud),
         
         //new Claim(ClaimTypes.Role,tokenModel.Role),//为了解决一个用户多个角色(比如：Admin,System)，用下边的方法
        };

     // 可以将一个用户的多个角色全部赋予；
     // 作者：DX 提供技术支持；
     claims.AddRange(tokenModel.Role.Split(',').Select(s => new Claim(ClaimTypes.Role, s)));



     //秘钥 (SymmetricSecurityKey 对安全性的要求，密钥的长度太短会报出异常)
     var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(secret));
     var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

     var jwt = new JwtSecurityToken(
         issuer: iss,
         claims: claims,
         signingCredentials: creds);

     var jwtHandler = new JwtSecurityTokenHandler();
     var encodedJwt = jwtHandler.WriteToken(jwt);

     return encodedJwt;
 }
```









## API保护

### 为什么要保护API

- 防泄露
- 防攻击

	- 1.防伪装攻击（案例：在公共网络环境中，第三方 有意或恶意 的调用我们的接口）
	- 2.防篡改攻击（案例：在公共网络环境中，请求头/查询字符串/内容 在传输过程被修改）
	- 3.防重放攻击（案例：在公共网络环境中，请求被截获，稍后被重放或多次重放）

- 收益化

### 设计原则

- 1.轻量级
- 2.易于开发、测试和部署
- 3.适合于异构系统（跨操作系统、多语言简易实现）
- 4.所有写操作接口（增、删、改 操作）
- 5.非公开的读接口（如：涉密/敏感/隐私 等）

### 加密算法

签名算法过程：

1.对除签名外的所有请求参数按key做的升序排列,value无需编码。（假设当前时间的时间戳是12345678）

例如：有c=3,b=2,a=1 三个参，另加上时间戳后， 按key排序后为：a=1，b=2，c=3，_timestamp=12345678。

2 把参数名和参数值连接成字符串，得到拼装字符：a1b2c3_timestamp12345678

3 用申请到的appkey 连接到接拼装字符串头部和尾部，然后进行32位MD5加密，最后将到得MD5加密摘要转化成大写。

示例：假设appkey=test，md5(testa1b2c3_timestamp12345678test)，取得MD5摘要值 C5F3EB5D7DC2748AED89E90AF00081E6 。



常见的加密方式：

DES加密算法：DES加密算法是一种分组密码，以64位为分组对数据加密，它的密钥长度是56位，加密解密用同一算法。DES加密算法是对密钥进行保密，而公开算法，包括加密和解密算法。这样，只有掌握了和发送方相同密钥的人才能解读由DES加密算法加密的密文数据。因此，破译DES加密算法实际上就是搜索密钥的编码。对于56位长度的密钥来说，如果用穷举法来进行搜索的话，其运算次数为256。
随着计算机系统能力的不断发展，DES的安全性比它刚出现时会弱得多，然而从非关键性质的实际出发，仍可以认为它是足够的。不过，DES现在仅用于旧系统的鉴定，而更多地选择新的加密标准。

AES加密算法：ES加密算法是密码学中的高级加密标准，该加密算法采用对称分组密码体制，密钥长度的最少支持为128、192、256，分组长度128位，算法应易于各种硬件和软件实现。这种加密算法是美国联邦政府采用的区块加密标准，这个标准用来替代原先的DES，已经被多方分析且广为全世界所使用。

RSA加密算法：RSA加密算法是目前最有影响力的公钥加密算法，并且被普遍认为是目前最优秀的公钥方案之一。RSA是第一个能同时用于加密和数宇签名的算法，它能够抵抗到目前为止已知的所有密码攻击，已被ISO推荐为公钥数据加密标准。RSA加密算法基于一个十分简单的数论事实：将两个大素数相乘十分容易，但那时想要，但那时想要对其乘积进行因式分解却极其困难，因此可以将乘积公开作为加密密钥。



Base64加密算法：Base64加密算法是网络上最常见的用于传输8bit字节代码的编码方式之一，Base64编码可用于在HTTP环境下传递较长的标识信息。例如，在JAVAPERSISTENCE系统HIBEMATE中，采用了Base64来将一个较长的唯一标识符编码为一个字符串，用作HTTP表单和HTTPGETURL中的参数。在其他应用程序中，也常常需要把二进制数据编码为适合放在URL（包括隐藏表单域）中的形式。此时，采用Base64编码不仅比较简短，同时也具有不可读性，即所编码的数据不会被人用肉眼所直接看到。

- ES
- RSA
- Base64
- MD5

### 有哪些方式保护方式

- 非登录：自约定数据加密

	- （报文sha+密钥+加密算法）

		- 只作为Header传递：Md5
		- 整体作为报文传递：解密，SHA

- 登录：登录Session + cookie

	- 依赖浏览器

- 登录：JWT

	- 一切源于HttpContext，
		重点是 Claim
		移动端和SPA更好

		- 自定义鉴权

			- 比较灵活，可以随意设计

		- 官方定义

			- 正规，完整的写法

## JWT核心知识点

### 什么是 claim？

嗯，这么长个例子，其实和Claim没什么关系 ：）

门票上有什么？我们来假设一下

门票上有，姓名，票价，性别，地址，【等等身份证一类的东东】

好了，我们假设的门票就这样，从门票的第二行（姓名...）开始,每一行都是一个Claim

有了上面的铺垫，我们接下来正式介绍下Claim

释义
Claim 本意有

vt.声称;索取;断言;需要
vi.提出要求
n.索赔;声称;（根据权利而提出的）要求;断言
断言是比较准确的释义，另外可以理解成声明，每一条claim 都代表了一条票据的信息，比如示例票据上的姓名等等。claim 的基本组成是 type和value，上面票据中左侧的就是type右面就是value

在 .net core 基础类库中是含有Claim的实现类的，它的位置是

System.Security.Claims.Claim

### 什么是 JWT

JWT（读作 [/dʒɒt/]），即JSON Web Tokens，是一种基于JSON的、用于在网络上声明某种主张的令牌（token）。

- 头部(Header) Base64

	- "alg": "HS265"  算法
	- "typ": "JWT"  类型

- 载荷(Payload) Base64

	- 声明(Claims)

		- ClaimTypes 
		- JwtRegisteredClaimNames 

- 签名(Signature) 

	- Signature Verified

- 查看方式

	- jwt.io

		- Invalid Signature

	- 浏览器 Base64URLDecode 分别解密头部和载荷

	  atob() 方法用于解码使用 base-64 编码的字符串。
	  
	  base-64 解密使用方法是 Base64URLDecode

- 获取方式

	- jwtHandler.ReadJwtToken
	- ClaimsPrincipal User
	- IHttpContextAccessor _accessor

### HttpContext是如何通过claim鉴权的？——Bearer认证（持票人）

### 注册 JwtBearer 认证

### JWT的好处

- 服务无状态
- 解决跨域问题：这种基于Token的访问策略可以克服cookies的跨域问题。
- 服务端无状态可以横向扩展，Token可完成认证，无需存储Session。
- 系统解耦，Token携带所有的用户信息，无需绑定一个特定的认证方案，只需要知道加密的方法和密钥就可以进行加密解密，有利于解耦。
- 防止跨站点脚本攻击，没有cookie技术，无需考虑跨站请求的安全问题。

## 如何使用JWT

### 自定义操作

- 授权授权（AddAuthorization）

	- 无状态授权： [Authorize]
	- 有状态授权：[Authorize(Roles = "Admin")]

- 自定义认证 + 中间件

### 官方方案

- 授权（AddAuthorization）

	- 无状态授权： [Authorize]
	- 有状态授权：[Authorize("Permission")] 

- 认证（AddAuthentication）

	- 开启 Bearer 认证
	- 注册 JwtBearer 
或者  IdentityServerAuthentication

- 中间件（UseAuthentication）

	- app.UseAuthentication();

