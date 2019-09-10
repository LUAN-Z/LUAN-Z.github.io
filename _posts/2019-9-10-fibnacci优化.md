---
layout: post
title: "Fibonacci 算法优化"
subtitle: ' Fibonacci python 解法'
date: 2019-9-10 23:30:00
author: "LUANXYZ"
header-img: "img/in-post/fibonacci.png"
tags:
  - 算法
---

## 一般解法

```python
def fibonacci(num: int) -> int:
    if num == 0:
        return 0
    if num == 1:
        return 1
    return fibonacci(num - 1) + fibonacci(num - 2)
```

## 优化解法

```python
def fibonacci_optimize(num: int):
    if num == 0:
        return 0
    if num == 1:
        return 1
    pre_res = [0, 1]
    for i in range(num - 1):
        res = pre_res[0] + pre_res[1]
        pre_res[0] = pre_res[1]
        pre_res[1] = res

    return res
```

## 比较

```python
NUM = 100
start = time.perf_counter()
# res = fibonacci(20)
res = fibonacci_optimize(20)
total_time = time.perf_counter() - start
print(res, total_time)
```

### 结果

| num  |结果 |时间/s(fibonacci) | 时间/s(fibonacci_optimize) |
| :--: | :-------------: | :---------------------: | :--------------------: |
|  5   | 5 | 3.85e-05 |8.7e-06|
|  10  | 55 | 0.0001164 |9.1e-06|
|  20  | 6765 | 0.019072699999999998 |1.67e-05|
|  50  | 12586269025 | ✘ |4.2e-05|
| 100  | 354224848179261915075 | ✘ |3.68e-05|
| 500  | res | ✘ |0.0007797|

res = 139423224561697880139724382870407283950070256587697307264108962948325571622863290691557658876222521294125

**两者的计算时间相差十分大，优化后的算法能大大提高 fibonacci 函数的计算时间，但是代价是什么呢？--空间计算式会占用比较大的内存，但是在现如今内存已不是算法首要考虑条件的背景下，以空间换时间也是一种很好的方法。**

