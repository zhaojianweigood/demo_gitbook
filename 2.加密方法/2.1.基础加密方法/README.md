
### itsdangerous 模块
```angular2html
from itsdangerous import JSONWebSignatureSerializer as JWSS
```

### sha1，sha224, sha256, sha384, sha512 返回info 和 signature, 本地有key, 使用sha1(info+key) 和signature比较
```angular2html
from hashlib import sha1, sha224, sha256, sha384, sha512
import json
info = {
    'id': 1,
    'user': 'Username'
    }
key = 'abc'
signature = sha1((json.dumps(info)+key).encode('utf-8'))
print(signature.hexdigest())
```

### md5
```angular2html
from hashlib import md5
import json
info = {
    'id': 1,
    'user': 'Username'
    }
key = 'abc'
signature = md5((json.dumps(info)+key).encode('utf-8'))
print(signature.hexdigest())
```

### AES-128-CBC，数据采用PKCS#7填充。, 微信案例
```angular2html
import base64
import json

from Crypto.Cipher import AES
def _unpad(s):
    return s[:-ord(s[len(s) - 1:])]

appId = 'wx4f4bc4dec97d474b'
sessionKey = 'tiihtNczf5v6AKRyjwEUhQ=='
iv = 'r7BXXKkLb8qrSNn05n0qiA=='
# 加密文本
encryptedData = 'CiyLU1Aw2KjvrjMdj8YKliAjtP4gsMZMQmRzooG2xrDcvSnxIMXFufNstNGTyaGS9uT5geRa0W4oTOb1WT7fJlAC+oNPdbB+3hVbJSRgv+4lGOETKUQz6OYStslQ142dNCuabNPGBzlooOmB231qMM85d2/fV6ChevvXvQP8Hkue1poOFtnEtpyxVLW1zAo6/1Xx1COxFvrc2d7UL/lmHInNlxuacJXwu0fjpXfz/YqYzBIBzD6WUfTIF9GRHpOn/Hz7saL8xz+W//FRAUid1OksQaQx4CMs8LOddcQhULW4ucetDf96JcR3g0gfRK4PC7E/r7Z6xNrXd2UIeorGj5Ef7b1pJAYB6Y5anaHqZ9J6nKEBvB4DnNLIVWSgARns/8wR2SiRS7MNACwTyrGvt9ts8p12PKFdlqYTopNHR1Vf7XjfhQlVsAJdNiKdYmYVoKlaRv85IfVunYzO0IKXsyl7JCUjCpoG20f0a04COwfneQAGGwd5oa+T8yO5hzuyDb/XcxxmK01EpqOyuxINew=='

# 解密步骤
sessionKey = base64.b64decode(sessionKey)
iv = base64.b64decode(iv)
cipher = AES.new(sessionKey, AES.MODE_CBC, iv)

encryptedData = base64.b64decode(encryptedData)
decrypted = json.loads(_unpad(cipher.decrypt(encryptedData).decode()))
print(_unpad(cipher.decrypt(encryptedData)))
print(cipher.decrypt(encryptedData))

assert decrypted['watermark']['appid'] == appId
print(decrypted['watermark']['appid'])
print(appId)

```

### JWT 加密
JSON Web Token  
>格式 header . payload . signature

>header : 类型， 算法

>payload: 内容

>signature: sha256(base64.b64encode('header') + '.' + base64.b64encode('payload'), 'SECRET_KEY')

>signature加密后类此: SwyHTEx_RQppr97g4J5lKXtabJecpejuef8AqKYMAJc

```angular2html
import base64
from hashlib import sha256
import json
header = {
    'type': 'JWT',
    'alg': 'sha256'
}
payload = {
    'id': 1,
    'user': 'UserName',
    'gender': 'boy',
    'birthday': '1997-12-12'
}
SECRET_KEY = 'abc'
header = base64.b64encode(json.dumps(header).encode('utf-8')).decode()
print(header)
payload = base64.b64encode(json.dumps(payload).encode('utf-8')).decode()
print(payload)
# HS256:   signature = HS256(header + '.' + payload,  SECRET_KEY)
signature = sha256((header + '.' + payload + SECRET_KEY).encode('utf-8'))
print(signature.hexdigest())
JWT = header + '.' + payload + '.' + signature.hexdigest()
print(JWT)
```

### RSA 加密
类如支付包对接时
使用openssl 生成rsa密钥
```angular2html
openssl
genrsa -out app_private_key.pem 2048  或者 genrsa -out app_private_key.pem 1024
rsa -in app_private_key.pem -pubout -out app_public_key.pem
exit
```
本地指定目录保存私钥， 开发平台指定位置上传公钥匙
```angular2html
import rsa
import json

(pubkey, privkey) = rsa.newkeys(1024)
print(pubkey.save_pkcs1())
print(privkey.save_pkcs1())

pubkey = pubkey.save_pkcs1()
privkey = privkey.save_pkcs1()
pubkey = rsa.PublicKey.load_pkcs1(pubkey)
privkey = rsa.PrivateKey.load_pkcs1(privkey)
data = {
    'id': 1,
    'user': 'UserName',
    'gender': '男',
}
# 公钥加密， 私钥解密
encryption = rsa.encrypt(json.dumps(data).encode('utf-8'), pubkey)
decryption = rsa.decrypt(encryption, privkey).decode()
print(json.loads(decryption))  # {'gender': '男', 'id': 1, 'user': 'UserName'}
```
