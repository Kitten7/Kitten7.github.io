# python-trade   wp

下载文件后发现：

![image-20200707231818022](D:\Typora Pictures\image-20200707231818022.png)

是一个prc文件？？pyc文件也就是一个二进制文件，是py文件编译之后生成的文件，是一种byte code，py文件变为pyc文件之后，速度提升了。

但是pyc文件不可以直接看到源码，那这就需要pyc文件反编译的工具，不过我用的是在线转换的：https://tool.lu/pyc/，如果是用命令下载的话：

`pip install uncompyle`

我使用的是在线工具进行转换的，转换之后就是下面这个样子：

```python
#!/usr/bin/env python
# encoding: utf-8
# 如果觉得不错，可以推荐给你的朋友！http://tool.lu/pyc
import base64

def encode(message):
    s = ''
    for i in message:
        x = ord(i) ^ 32#这个是亦或的符号，之前老是看成是xx次方的符号。。
        x = x + 16
        s += chr(x)
    
    return base64.b64encode(s)

correct = 'XlNkVmtUI1MgXWBZXCFeKY+AaXNt'
flag = ''
print ('Input flag:')
flag = input()
if encode(flag) == correct:
    print ('correct')
else:
    print ('wrong')
```

其实逆过来想一下，只要输入的flag经过加密之后和'XlNkVmtUI1MgXWBZXCFeKY+AaXNt'相等，最后就输出“correct”，flag也就拿到了。

试着运行一下：

![image-20200707234155365](D:\Typora Pictures\image-20200707234155365.png)

代码如下：

```python
import base64

def decode(message):
    s = ''
    imessage = base64.b64decode(message)

    for i in imessage:
        x = ord(i) - 16
        x = x ^ 32
        s += chr(x)

    return s

correct = 'XlNkVmtUI1MgXWBZXCFeKY+AaXNt'

flag = decode(correct)
print(flag)
```

运行看一下：

![image-20200707234301291](D:\Typora Pictures\image-20200707234301291.png)

nctf{d3c0mpil1n9_PyC}



咳咳，说一下，那个base64.b64decode就是转换为base64编码的一个方式吧，值得注意的是，除了需要在使用之前import base64，另外就是：

`Decode --------------解码(解的是base64加密的,可以是bytes-like object或者是ASCII string s)
Encode  ----------编码(用64个字符表示任意二进制数据，加密的是bytes-like object，返回的是 a bytes object)
两者得到的均是bytes 类型`

