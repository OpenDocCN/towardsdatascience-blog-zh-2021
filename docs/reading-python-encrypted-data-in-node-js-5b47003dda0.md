# 读取 Node.js 中的 Python 加密数据

> 原文：<https://towardsdatascience.com/reading-python-encrypted-data-in-node-js-5b47003dda0?source=collection_archive---------17----------------------->

## 有时我们希望将受密码保护的数据传递给节点桌面或 web 应用程序。在本教程中，我们用 python 加密一个 JSON 文件，并在 Node.js 中解密它。

![](img/7974db5f01a8051a409c7bd1840726ab.png)

照片由 [Luemen Rutkowski](https://unsplash.com/@lulusphotography?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 背景

当开发具有许多潜在用途的软件时，我们可能会遇到这样的情况:一些用户使用潜在的敏感数据集，但其他用户不应该使用这些数据集。更有效的方法是对数据集进行密码加密，这样只有拥有专用密钥的人才能访问它，而不是依赖我们的用户来进行细致的文件权限管理。

本教程涵盖了用 python 对信息进行编码，然后用 nodejs / electron 对其进行解码。

# 数据

我们的数据可以是任何可序列化的对象——无论是字符串还是熊猫数据框。

在 JavaScript 中，我们会使用`JSON.stringify`函数。在 python 中，如果我们有一个数据框，我们可以尝试使用`.to_json()`函数。

在这个过程中，我们产生了一个我们想要加密的对象的字符串形式。

*如果我们正在字符串化一个 JSON 对象，我们需要记住在节点应用程序中使用* `*JSON.parse*` *将其转换回来。*

# 节点 JS 加密周期

在 NodeJS 中，我们使用“加密”库。这个可以用`npm i g crypto`全局安装。有了它，就有了一系列可用的加密算法。在本例中，我们选择了 AES-256-CBC ( **密码拦截器链接** ) block cypher 加密，这是一种对称加密算法，意味着同一密钥可用于我们数据的加密和解密。

作为“加密”的一部分，我们有`createCipheriv`加密功能和`createDecipheriv`解密功能。由于设置这些对象所需的大部分信息都是相同的，我们可以创建一个返回 encryptor 或 decrypter 对象的函数:

```
function get_crypto(key,encode){ // Create hashed key from password/key
    var m = crypto.createHash('md5').update(key)
    var key = m.digest('hex'); m = crypto.createHash('md5').update(key + key)
    var iv = m.digest('hex').slice(0,16);

    return encode? crypto.createCipheriv('aes-256-cbc', key, iv) : crypto.createDecipheriv('aes-256-cbc', key, iv)

}
```

## 加密

要在节点中加密，我们可以将数据字符串转换为二进制形式:

```
var data = new Buffer.from(value, 'utf8').toString('binary');
```

然后将其添加到密码中

```
cipher = get_crypto(key,true)encrypted = cipher.update(data, 'utf8', 'binary') + cipher.final('binary');var encoded = new Buffer.from(encrypted, 'binary').toString('base64');
```

## 解码

为了解码，我们遵循类似的过程，使用加密的字符串作为我们的输入:

```
decipher = get_crypto(key,false);decoded_text = (decipher.update(edata, 'binary', 'utf8') + decipher.final('utf8'));
```

# 使用 Python 加密

在 python 中加密和解密的过程非常相似，除了我们使用“pycrypto”库。这是用`pip install pycrypto`安装的。

我们首先用以下公式定义我们的加密算法:

```
def get_aes (password):
    m = md5()
    m.update(password.encode('utf-8'))
    key = m.hexdigest() m = md5()
    m.update((password*2).encode('utf-8'))
    iv = m.hexdigest()

    return AES.new(key, AES.MODE_CBC, iv[:BLOCK_SIZE])
```

以及填充/去填充函数的集合:

```
BLOCK_SIZE = 16def pad (data):
    data = ' '*BLOCK_SIZE + data
    pad = BLOCK_SIZE - len(data) % BLOCK_SIZE
    return data + pad * chr(pad)def unpad (padded):
    pad = ord(chr(padded[-1]))
    return padded[BLOCK_SIZE:-pad]
```

## 加密

为了加密，我们使用 aes 类的加密函数:

```
def _encrypt(password,data):
    data = pad(data)
    aes = get_aes(password) encrypted = aes.encrypt(data)
    return base64.urlsafe_b64encode(encrypted).decode('utf-8')
```

## [通信]解密

同样，解密功能可以逆转这一过程

```
def _decrypt(password,edata):
    edata = base64.urlsafe_b64decode(edata)
    aes = get_aes(password)

    return unpad(aes.decrypt(edata)).decode('utf-8')
```

# 把所有的放在一起

现在我们有了加密和解密这两个程序的工具，我们可以把这两个结合起来。

*注意:在提供的代码中，加密密钥在 python 脚本中作为明文提供，并作为参数传递给 node。这通常不是一个好主意。更好的方法是在每个目录的* `*.env*` *文件中设置一个关键变量，并通过 python 和 node 各自的‘dotenv’库进行访问。*

为了测试代码，我们导入 python 库，使用我们的密码(`key`)对我们的信息(`value`)进行编码，然后将其作为参数传递给我们的解密节点程序‘node decrypt’。然后，我们使用 os 库生成一个 shell，运行命令并读取返回的结果。

```
output = pe._encrypt(key,value) 
print('\nPython encrypt:\n', output)decrypt = pe._decrypt(key,output)
print('\nPython decrypt:\n', decrypt)cmd = 'node nodedecrypt ---key "%s" --estr "%s"'%(key,output)
# print(cmd)
node = os.popen(cmd).read()
print('\n\nNode Decrypt:\n', node)
```

如果一切正常，我们将从 python 解密和节点解密中获得两个相同的文本。

# 示例代码

可在应用程序中使用的模板代码位于:

<https://github.com/wolfiex/python-node-encryption>  

自述文件包含所有相关的安装命令，并将用户指引到运行该示例的`communicate.py`文件。

黑客快乐！