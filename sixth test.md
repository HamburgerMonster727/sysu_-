---
title: "第六次项目:使用python求解"
date: 2020-11-11
lastmod: 2020-11-11
draft: false
categories: ["中文", "项目"]
author: "梁冠轩"
---

# 第六次项目:使用python求解

## 1.选择2个高等数学上的作业，如泰勒分解、公式化简、解方程等

### （1）多项式求解再求值

```python
import sympy
x,y = sympy.symbols("x y")
a = sympy.solve([x + y - 0.2,x + 3*y -1],[x,y])
x = a[x]
y = a[y]
re = x**2+4*x*y +4*y**2
print(re)  #多项式求解再求值
```

输出为：0.360000000000000

### （2）拆分多项式

```python
import sympy
s = sympy.symbols("s")
ans = 1/(s**3+s**2+s+1)
print(sympy.apart(ans)) #拆分多项式
```

输出为：-(s - 1)/(2*(s**2 + 1)) + 1/(2*(s + 1))

## 2.选择2个线性代数上的作业，如求dot、逆矩阵等，最好会解方程

### （1）矩阵乘法，矩阵转置

```python
import numpy as np
a = [[1,0],[1,1]]
b = [[1,2],[2,3]]
print(a)
print(b)
print(np.matmul(a,b)) #矩阵乘法
print(np.transpose(a)) #矩阵的转置
```

输出为：

```
[[1, 0], [1, 1]]
[[1, 2], [2, 3]]
[[1 2]
 [3 5]]
[[1 1]
 [0 1]]
```

### （2）矩阵的逆矩阵

```python
import numpy as np
a = np.array([[1,2],[3,4]])
print(np.linalg.inv(a)) #矩阵求逆矩阵
A = np.matrix(a)
print(A.I) #另一种方法
```

输出为：

```
[[-2.   1. ]
 [ 1.5 -0.5]]
[[-2.   1. ]
 [ 1.5 -0.5]]
```