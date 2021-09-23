# 使用JsonWebToken包实现JWT

```c#
/// <summary>
        /// 创建Token
        /// </summary>
        /// <returns>Token</returns>
        public static (string,string) CreateToken(QjCurrentUser qjUser)
        {



            // Creates a symmetric key defined for the 'HS256' algorithm
            var signingKey = SymmetricJwk.FromBase64Url("R9MyWaEoyiMYViVWo8Fk4TUGWiSoaW6U1nOqXri8_XU");

            // Creates a JWS descriptor with all its properties
            var descriptor = new JwsDescriptor(signingKey, SignatureAlgorithm.HS256);
            descriptor.Payload.Add("unique_name",qjUser.ToString());
            descriptor.Payload.Add("iss","1");
            descriptor.Payload.Add("aud","2");
            descriptor.Payload.Add("iat",DateTime.Now);
            descriptor.Payload.Add("exp",DateTime.Now.AddHours(18));
            descriptor.Payload.Add("email", "mgjterran@163.com");
            var writer = new JwtWriter();
            var token1 = writer.WriteTokenString(descriptor);
            descriptor = new JwsDescriptor(signingKey, SignatureAlgorithm.HS256);
            descriptor.Payload.Add("unique_name", qjUser.ToString());
            descriptor.Payload.Add("iss", "1");
            descriptor.Payload.Add("iud", "2");
            descriptor.Payload.Add("iat", DateTime.Now);
            descriptor.Payload.Add("exp", DateTime.Now.AddDays(10));
            descriptor.Payload.Add("email", "mgjterran@163.com");
            
            var token2 = writer.WriteTokenString(descriptor);
            return (token1, token2);


        }

        /// <summary>
        /// 验证密钥
        /// </summary>
        /// <param name="encodedJwt"></param>
        /// <returns></returns>
        public static QjCurrentUser ValidateToken(string encodedJwt)
        {

            var user = new QjCurrentUser();
            var key = SymmetricJwk.FromBase64Url("R9MyWaEoyiMYViVWo8Fk4TUGWiSoaW6U1nOqXri8_XU");
            var policy = new TokenValidationPolicyBuilder()
                            
                            .RequireAudience(Audience)
                            .RequireSignature(Issuer,key, SignatureAlgorithm.HS256)
                            .Build();
            if (Jwt.TryParse(encodedJwt,policy,out Jwt jwt))
            {
                jwt.Payload.TryGetClaim("unique_name",out JwtElement jwtElement);
              user= JsonConvert.DeserializeObject<QjCurrentUser>(jwtElement.GetString());
            }
            else
            {
                jwt.Dispose();
                return null;
            }
            jwt.Dispose();
            return user;

        }
```

