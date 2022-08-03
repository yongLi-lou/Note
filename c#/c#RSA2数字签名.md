# c#RSA2数字签名
```c#
public static string Sign(string contentForSign, string privateKey)
        {
            rsa.FromXmlString(privateKey);
            using (SHA256CryptoServiceProvider halg = new SHA256CryptoServiceProvider())
            {
                byte[] inArray = rsa.SignData(Encoding.UTF8.GetBytes(contentForSign), halg);
                return Convert.ToBase64String(inArray);
            }
        }

        
        public static bool VerifySignature(string encryptSource, string compareString, string publicKey)
        {
            try
            {
                rsa.FromXmlString(publicKey);
                byte[] rgbSignature = Convert.FromBase64String(encryptSource);
                SHA256Managed sHA256Managed = new SHA256Managed();
                RSAPKCS1SignatureDeformatter rSAPKCS1SignatureDeformatter = new RSAPKCS1SignatureDeformatter(rsa);
                rSAPKCS1SignatureDeformatter.SetHashAlgorithm("SHA256");
                byte[] rgbHash = sHA256Managed.ComputeHash(Encoding.UTF8.GetBytes(compareString));
                return rSAPKCS1SignatureDeformatter.VerifySignature(rgbHash, rgbSignature);
            }
            catch (Exception)
            {
                return false;
            }
        }

private string FormatSign(Dictionary<string, string> dic) {
            var _dic = GetParam(dic);
            var str=new StringBuilder();
            foreach (var item in _dic)
            {
                str.Append($"{item.Key}={item.Value}&");
            }
            str.Remove(str.Length - 1, 1);
            return str.ToString();


        }
        
        /// <summary>
        /// 参数按照ASCII码从小到大排序（字典序）
        /// </summary>
        /// <param name="paramsMap">参数</param>
        /// <returns></returns>
        public Dictionary<string, string> GetParam(Dictionary<string, string> paramsMap)
        {

            var vDic = paramsMap.OrderBy(x => x.Key, new ComparerString()).ToDictionary(x => x.Key, y => y.Value);

            return vDic;
        }
       
        /// <summary>
        /// 对比
        /// </summary>
        partial class ComparerString : IComparer<String>
        {
            public int Compare(String x, String y)
            {
                return string.CompareOrdinal(x, y);
            }
        }
```
### 为什么要用数字签名
1. 公钥和私钥的加密解密是相互的
2. 为了确定调用接口的人的身份
**发签人用私钥加密签名然后接收人用公钥解密验签**
**因为只有我知道我的私钥所以公钥匹配之后请求人一定是我**


