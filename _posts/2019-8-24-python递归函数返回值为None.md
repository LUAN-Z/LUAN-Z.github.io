---
layout: post
title: "Python 递归函数返回值为 `None`"
subtitle: "递归函数无正确返回值"
date: 2019-08-24 10:20:00
author: "LUANXYZ"
header-img: "img/in-post/recursive.png"
tags:
  - Python
  - 问题与解决方法
---


>在调用有返回值的递归函数时，即使调用正确，每次返回的结果都是 `None`,但在函数内部调用 `print` 函数时可以打印正确的结果，但是 `return` 值都为 `None`。

## 问题

代码
```python

def get_response_html() -> dict:
    requset_url, headers, proxies = request_configure()
    response = requests.get(
        url=requset_url, headers=headers, proxies=proxies)
    json_response = json.loads(response.text)
    print(json_response["message"])
    if json_response["message"] == "error":
        get_response_html()
    else:
        return json_response

```

理想输出

```bash
error
error
error
error
success
{'has_more': False, 'message': 'success', 'data':
......
'next': {'max_behot_time': 1566577698}}

```

实际输出

```bash
error
error
error
error
success
None
```

>`return` 在不带参数的情况下（或者没有写 `return` 语句），默认返回 `None`。 `None` 是一个特殊的值，它的数据类型是 `NoneType`。 `NoneType` 是 Python 的特殊类型，它只有一个取值 `None`。它不支持任何运算也没有任何内建方法，和任何其他的数据类型比较是否相等时永远返回 `False` ，也可以将 `None` 赋值给任何变量。


## 解决方法

- 当函数没有显式 return，默认返回 None 值
- 当递归函数有 return 时，在递归的地方也要 return，不然永远返回的是 None

```python

def get_response_html() -> dict:
    requset_url, headers, proxies = request_configure()
    response = requests.get(
        url=requset_url, headers=headers, proxies=proxies)
    json_response = json.loads(response.text)
    print(json_response["message"])
    if json_response["message"] == "error":
        # 解决递归返回值为 None 的方法是在递归处加 return 返回结果
        return get_response_html()
    else:
        return json_response

```

输出

```bash
error
error
error
error
success
{'has_more': False, 'message': 'success', 'data':
......
'next': {'max_behot_time': 1566577698}}

```
