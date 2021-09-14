# RSA公钥密钥XML--PEM互转

```c#
#region XML转PEM
        /// <summary>
        /// XML格式的公钥转PEM格式的公钥
        /// </summary>
        /// <param name="xmlpubkey">xml格式的公钥</param>
        /// <returns>pem格式的公钥</returns>
        public static string XMLToPEM_Pub(string xmlpubkey)
        {
            var rsa = new RSACryptoServiceProvider();
            rsa.FromXmlString(xmlpubkey);
            var p = rsa.ExportParameters(false);
            RsaKeyParameters key = new RsaKeyParameters(false, new BigInteger(1, p.Modulus), new BigInteger(1, p.Exponent));
            SubjectPublicKeyInfo publicKeyInfo = SubjectPublicKeyInfoFactory.CreateSubjectPublicKeyInfo(key);
            byte[] serializedPublicBytes = publicKeyInfo.ToAsn1Object().GetDerEncoded();
            string publicKey = Convert.ToBase64String(serializedPublicBytes);
            return Format(publicKey, true);
        }
        /// <summary>
        /// XML格式的私钥转PEM格式的私钥
        /// </summary>
        /// <param name="xmlprikey">xml格式的私钥</param>
        /// <returns>pem格式的私钥</returns>
        public static string XMLToPEM_Pri(string xmlprikey)
        {
            var rsa = new RSACryptoServiceProvider();
            rsa.FromXmlString(xmlprikey);
            var p = rsa.ExportParameters(true);
            var key = new RsaPrivateCrtKeyParameters(
                new BigInteger(1, p.Modulus), new BigInteger(1, p.Exponent), new BigInteger(1, p.D),
                new BigInteger(1, p.P), new BigInteger(1, p.Q), new BigInteger(1, p.DP), new BigInteger(1, p.DQ),
                new BigInteger(1, p.InverseQ));
            PrivateKeyInfo privateKeyInfo = PrivateKeyInfoFactory.CreatePrivateKeyInfo(key);
            byte[] serializedPrivateBytes = privateKeyInfo.ToAsn1Object().GetEncoded();
            string privateKey = Convert.ToBase64String(serializedPrivateBytes);
            return Format(privateKey, false);
        }
        /// <summary>
        /// 格式化公钥/私钥
        /// </summary>
        /// <param name="key">生成的公钥/私钥</param>
        /// <param name="type">true:公钥 false:私钥</param>
        /// <returns>PEM格式的公钥/私钥</returns>
        private static string Format(string key, bool type)
        {
            string result = string.Empty;

            int length = key.Length / 64;
            for (int i = 0; i < length; i++)
            {
                int start = i * 64;
                result = result + key.Substring(start, 64) + "\r\n";
            }

            result = result + key.Substring(length * 64);
            if (type)
            {
                result = result.Insert(0, "-----BEGIN PUBLIC KEY-----\r\n");
                result += "\r\n-----END PUBLIC KEY-----";
            }
            else
            {
                result = result.Insert(0, "-----BEGIN PRIVATE KEY-----\r\n");
                result += "\r\n-----END PRIVATE KEY-----";
            }

            return result;
        }
        #endregion
        #region PEM转XML
        /// <summary>
        /// PEM格式的密钥转XML格式
        /// </summary>
        /// <param name="pemkey">pem格式的密钥</param>
        /// <param name="isprikey">true：私钥;false：公钥</param>
        /// <returns>xml格式密钥</returns>
        public static string PEMToXML_All(string pemkey, bool isprikey)
        {
            if (isprikey)
            {
                pemkey = pemkey.Replace("-----BEGIN PRIVATE KEY-----", "").Replace("-----END PRIVATE KEY-----", "");
            }
            else
            {
                pemkey = pemkey.Replace("-----BEGIN PUBLIC KEY-----", "").Replace("-----END PUBLIC KEY-----", "");
            }
            string rsaKey = string.Empty;
            object pemObject = null;
            RSAParameters rsaPara = new RSAParameters();
            using (StringReader sReader = new StringReader(pemkey))
            {
                var pemReader = new Org.BouncyCastle.OpenSsl.PemReader(sReader);
                pemObject = pemReader.ReadObject();
            }
            //RSA私钥
            if (isprikey)
            {
                RsaPrivateCrtKeyParameters key = (RsaPrivateCrtKeyParameters)PrivateKeyFactory.CreateKey(Convert.FromBase64String(pemkey));
                rsaPara = new RSAParameters
                {
                    Modulus = key.Modulus.ToByteArrayUnsigned(),
                    Exponent = key.PublicExponent.ToByteArrayUnsigned(),
                    D = key.Exponent.ToByteArrayUnsigned(),
                    P = key.P.ToByteArrayUnsigned(),
                    Q = key.Q.ToByteArrayUnsigned(),
                    DP = key.DP.ToByteArrayUnsigned(),
                    DQ = key.DQ.ToByteArrayUnsigned(),
                    InverseQ = key.QInv.ToByteArrayUnsigned(),
                };
            }
            //RSA公钥
            else
            {
                RsaKeyParameters key = (RsaKeyParameters)PublicKeyFactory.CreateKey(Convert.FromBase64String(pemkey));
                rsaPara.Modulus = key.Modulus.ToByteArrayUnsigned();
                rsaPara.Exponent = key.Exponent.ToByteArrayUnsigned();
            }
            RSACryptoServiceProvider rsa = new RSACryptoServiceProvider();
            rsa.ImportParameters(rsaPara);
            using (StringWriter sw = new StringWriter())
            {
                sw.Write(rsa.ToXmlString(isprikey ? true : false));
                rsaKey = sw.ToString();
            }
            return rsaKey;
        }
        #endregion
```

