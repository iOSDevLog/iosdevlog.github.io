---
layout: post
title: "google Colaboratory"
author: iosdevlog
date: 2018-02-06 11:31:31 +0800
description: ""
category: ai
tags: []
---

<https://colab.research.google.com>

<https://colab.research.google.com/notebook>

# welcome
---

<https://colab.research.google.com/notebooks/welcome.ipynb>


```
# -*- coding: utf-8 -*-
"""Colaboratory 简介

Automatically generated by Colaboratory.

Original file is located at
    https://colab.research.google.com/notebooks/welcome.ipynb

## 欢迎使用 Colaboratory！

Colaboratory 是一种数据分析工具，可将文字、代码和代码输出内容合并到一个协作文档中。
"""

print 'Hello, Colaboratory!'

"""借助 Colaboratory，您只需点击一下鼠标，即可在浏览器中执行 TensorFlow 代码。下面的示例展示了两个矩阵相加的情况。

$\begin{bmatrix}
  1. & 1. & 1. \\
  1. & 1. & 1. \\
\end{bmatrix} +
\begin{bmatrix}
  1. & 2. & 3. \\
  4. & 5. & 6. \\
\end{bmatrix} =
\begin{bmatrix}
  2. & 3. & 4. \\
  5. & 6. & 7. \\
\end{bmatrix}$
"""

import tensorflow as tf
import numpy as np

with tf.Session():
  input1 = tf.constant(1.0, shape=[2, 3])
  input2 = tf.constant(np.reshape(np.arange(1.0, 7.0, dtype=np.float32), (2, 3)))
  output = tf.add(input1, input2)
  result = output.eval()
  print result

"""Colaboratory 包含很多已被广泛使用的库（例如 [matplotlib](https://matplotlib.org/)），因而能够简化数据的可视化过程。"""

import matplotlib.pyplot as plt

x = np.arange(20)
y = map(lambda x: x + np.random.randn(1), x)
a, b = np.polyfit(x, y, 1)
plt.plot(x, y, 'o', np.arange(20), a*np.arange(20)+b, '-');

"""Colaboratory 可与 Google Cloud BigQuery 结合使用。

[BigQuery 记事本范例](/notebook#fileId=/v2/external/notebooks/bigquery.ipynb)。
"""
```