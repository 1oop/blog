+++
title = '前端反爬对抗：加密'
date = 2018-11-22T17:11:34+08:00
tags = [
    "爬虫",
    "反爬"
]
categories = [
    "爬虫"
]
+++

# 前端反爬对抗：加密

## 常见加密算法

请注意，这个对比主要集中在对称加密算法和一些非对称加密算法的几个关键维度，包括安全性、速度、密钥长度等。

| 加密算法  | 类型     | 密钥长度（常用） | 安全性    | 速度     | 使用场景       | 注释                |
|-----------|---------|-----------------|---------|---------|--------------|---------------------|
| AES       | 对称     | 128, 192, 256位  | 高      | 高      | 广泛使用       | 当前标准，抵抗大多数攻击 |
| DES       | 对称     | 56位            | 低      | 中      | 已被淘汰       | 容易受到暴力攻击        |
| 3DES      | 对称     | 168位           | 中      | 低      | 逐渐被AES取代  | 比DES安全，但更慢       |
| Blowfish  | 对称     | 32至448位       | 中      | 高      | 旧系统，小文件加密 | 可自定义密钥长度        |
| Twofish   | 对称     | 128, 192, 256位  | 高      | 中      | 比赛候选       | AES的竞争对手           |
| RSA       | 非对称   | 1024, 2048, 4096位 | 高    | 低      | 数字签名、密钥交换 | 计算密集，适用小数据加密  |
| ECC       | 非对称   | 160, 224, 256位  | 高      | 高      | 移动设备       | 较小的密钥提供强安全性    |
| DSA       | 非对称   | 1024, 2048, 3072位 | 高    | 中      | 数字签名       | 不支持加密，只用于签名    |
| SHA-256   | 散列函数 | -               | 高      | 高      | 数据完整性验证 | 广泛用于散列，非加密算法  |
| MD5       | 散列函数 | -               | 低      | 高      | 已被淘汰       | 不安全，避免使用          |

**安全性**：
- "高"表示算法被认为是非常安全的，并且能抵抗当前的所有已知攻击。
- "中"表示算法较为安全，但可能存在已知的理论攻击方法，或者密钥长度不足以保证长期安全。
- "低"表示算法已经被证实不安全，存在已知的有效攻击手段，或者密钥长度明显不足。

**速度**：
- "高"表示算法运行速度快，适合需要加密或解密大量数据的场景。
- "中"表示算法的速度适中，适合一般情况。
- "低"表示算法运行速度慢，计算密集，通常适用于小量数据。

**使用场景**：
- 描述了算法通常被用于哪些类型的应用中。

**注释**：
- 提供了额外的信息，如算法的特点、竞争对手、使用限制等。


在前端开发中，为了保障数据的安全性，通常会使用一些成熟的加密库来实现数据的加密和解密。下面是一些常用的前端加密算法库及其基本使用方法：

## 1. CryptoJS

**库介绍：**
CryptoJS 是一个纯JavaScript编写的加密标准库，支持多种加密算法，如AES、DES、Triple DES、Rabbit、RC4、MD5和SHA-1等。

**使用方式示例：**
```javascript
const CryptoJS = require("CryptoJS");
// AES 加密
var encrypted = CryptoJS.AES.encrypt('my message', 'secret key 123');

// AES 解密
var decrypted = CryptoJS.AES.decrypt(encrypted.toString(), 'secret key 123');
```

## 2. jsencrypt

**库介绍：**
jsencrypt 库可以在前端实现RSA加密，适用于需要非对称加密的场景。

**使用方式示例：**
```javascript
const JSEncrypt = require("JSEncrypt");

// 创建实例
var encrypt = new JSEncrypt();

// 设置公钥
encrypt.setPublicKey(`-----BEGIN PUBLIC KEY-----...`);

// 加密数据
var encrypted = encrypt.encrypt('my message');
```

## 3. forge

**库介绍：**
forge 是一个强大的前端加密库，它提供了包括AES、DES、RSA、EC、MD5和SHA等多种算法的实现。

**使用方式示例：**
```javascript
const forge = require("forge");

// AES 加密
var cipher = forge.cipher.createCipher('AES-CBC', 'myKey');
cipher.start({iv: 'myIV'});
cipher.update(forge.util.createBuffer('my message'));
cipher.finish();
var encrypted = cipher.output;

// AES 解密
var decipher = forge.cipher.createDecipher('AES-CBC', 'myKey');
decipher.start({iv: 'myIV'});
decipher.update(encrypted);
decipher.finish();
var decrypted = decipher.output.toString();
```

## 4. openpgp.js

**库介绍：**
openpgp.js 是一个开源的PGP（Pretty Good Privacy）加密库，它实现了OpenPGP协议的前端加密和解密功能。

**使用方式示例：**
```javascript
const openpgp = require("openpgp");

// 加密
openpgp.encrypt({
    message: openpgp.message.fromText('my message'),       
    publicKeys: (await openpgp.key.readArmored(pubkey)).keys
}).then(ciphertext => {
    encrypted = ciphertext.data;
});

// 解密
openpgp.decrypt({
    message: await openpgp.message.readArmored(encrypted),     
    privateKeys: [privKeyObj] 
}).then(plaintext => {
    console.log(plaintext.data);
});
```

## 5. bcrypt.js

**库介绍：**
bcrypt.js 是一个用于密码散列的库，它实现了bcrypt散列算法，适用于安全地存储用户密码。

**使用方式示例：**
```javascript
const bcrypt = require("bcrypt");
// 生成散列
var salt = bcrypt.genSaltSync(10);
var hash = bcrypt.hashSync('my password', salt);

// 验证密码
bcrypt.compareSync('my password', hash); // true
```

熟悉常见的加密库及其使用方式,可以快速分析网站加密参数的加密方式.