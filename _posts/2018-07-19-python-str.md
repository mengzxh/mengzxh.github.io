---
layout: post
title: 'Python字符串'
subtitle: ''
date: 2018-07-19
categories: 技术
cover: '/cover_img/material-2.png'
tags: Python
---






#### str、repr

Python打印所有的字符串时，都用引号将其括起来。通过例子可以发现：Python打印值时，保留其在代码中样子，而不是用户看到的样子。如果使用print函数，结果将不同。

```python
>>>"Hello, world!"
'Hello, world!'
>>>print("Hello, world!")
Hello, world!
# 加上换行符的编码\n，差别将更明显。
>>>"Hello,\nworld!"
'Hello,\nworld!'
>>>print("Hello,\nworld!")
Hello,
world!
```

通过两种不同的机制将转换成了字符串。你可通过使用函数str和repr直接使用这两种机制。使用str能以合理的方式将值转换为用户能够看懂的字符串。例如，尽可能将特殊字符串编码转换为相应的字符。然而，使用repr时，通常会获得值的合法Python表达式表示。

```python
>>>print(repr("Hello,\nworld!"))
'Hello,\nworld!'
>>>print(str("Hello,\nworld!"))
Hello,
world!
```

#### Unicode、bytes、 bytearray

(1) Unicode 相关知识请参阅字符编码笔记

(2) bytes对象 

* 创建bytes对象--可以直接使用b''创建(仅限ASCII包含符号)

  ```python
  >>>b'Hello world!'
  b'Hello world!'
  ```

* 将字符串编码为bytes

  ```python
  >>>'hello world!'.encode("ASCII")
  b'hello world!'
  >>>'你好'.encode("ASCII")
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
  # ASCII无法编码中文字符
  >>>'hello world!'.encode("utf-8")
  b'hello world!'
  >>>'你好'.encode("utf-8")
  b'\xe4\xbd\xa0\xe5\xa5\xbd'
  # 对于无法编码的字符可以选择忽略/替换
  >>>'hello world。'.encode("ASCII", "ignore")
  b'hello world'
  >>>'hello world。'.encode("ASCII", "replace")
  b'hello world?'
  ```

* 使用bytes函数创建(一般使用这种方法，使用utf-8编码一般不会出问题)

  ```python
  >>>bytes("hello world。", encoding="utf-8")
  b'hello world\xe3\x80\x82'
  ```

* 将bytes解码为字符串

  ```python
  >>>b'hello world!'.decode("ASCII")
  'hello world!'
  >>>b'\xe4\xbd\xa0\xe5\xa5\xbd'.decode("utf-8")
  '你好'
  ```

(3) bytearray对象(bytes的可变版)

* bytearray是可修改的字符串----常规字符串是不能修改的。然而，bytearray其实是为在幕后使用而设计的，因此 作为类字符串使用时对用户并不友好。例如，要替换其中的字符，必须将其指定为0～255的值。 因此，要插入字符，必须使用ord获取其序数值（ordinal value） 

  ```python
  >>>a = bytearray(b'hello')
  >>>a[1] = ord(b'u')
  >>>a
  bytearray(b'hullo')
  ```



#### 参考资料

Python基础教程-第3版(文字版) -- page-18



