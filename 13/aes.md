AES
===

## 1. AES

- 秘钥: 加解密使用相同秘钥
- 填充: 分块(128bit=16bytes)加密,不够的部分填充, NoPadding: % 128bit = 0, ZeroPadding: 明文最后位为0会有问题,PKCS7Padding,差几个字节就补几个几
- 模式: ECB: 每块加密独立(同时相同内容加密结果相同), CBC: iv,明文块加密前做异或,后明文和前明文所得密文做异或
- 加密结果: Hash / Base64

```
# PKCS7Padding需要确认一下讲的有没有问题:
0 1 2 3 4 5 6 7 8 9 0 1 2: 3 3 3
0 1 2 3 4 5 6 7 8 9 0 1: 4 4 4 4
```

## 2. AES加密JS示例

```
var CryptoJS = require("crypto-js");
var key = "ABC123456789";  // 建议key和iv长度为16/32字节对应128/256bit
var iv = "1234567812345678";

function encrypt(text) {
    // CryptoJS.enc.Utf8.parse: 秘钥和iv都需要转化为128bit
    return CryptoJS.AES.encrypt(text, CryptoJS.enc.Utf8.parse(key), {
        iv: CryptoJS.enc.Utf8.parse(iv),
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    })
}

function decrypt(text) {
    var result = CryptoJS.AES.decrypt(text, CryptoJS.enc.Utf8.parse(key), {
        iv: CryptoJS.enc.Utf8.parse(iv),
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    }) return result.toString(CryptoJS.enc.Utf8)
}
var text = ”nickmyb";
var encoded=encrypt(text)
console.log(encoded.toString());
console.log(decrypt(encoded))
```

# 13-2