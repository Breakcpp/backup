---
title: 基于MacOs的TensorFlow安装
date: 2018-9-18
tags: Mechine Learing
---

# TensorFlow 简介
TensorFlow是Google在2015年11月份开源的人工智能系统（Github项目地址），是之前所开发的深度学习基础架构DistBelief的改进版本，该系统可以被用于语音识别、图片识别等多个领域。

官网上对TensorFlow的介绍是，一个使用数据流图(data flow graphs)技术来进行数值计算的开源软件库。数据流图中的节点，代表数值运算；节点节点之间的边，代表多维数据(tensors)之间的某种联系。你可以在多种设备（含有CPU或GPU）上通过简单的API调用来使用该系统的功能。TensorFlow是由Google Brain团队的研发人员负责的项目。

什么是数据流图(Data Flow Graph)
数据流图是描述有向图中的数值计算过程。有向图中的节点通常代表数学运算，但也可以表示数据的输入、输出和读写等操作；有向图中的边表示节点之间的某种联系，它负责传输多维数据(Tensors)。图中这些tensors的flow也就是TensorFlow的命名来源。

节点可以被分配到多个计算设备上，可以异步和并行地执行操作。因为是有向图，所以只有等到之前的入度节点们的计算状态完成后，当前节点才能执行操作。

## TensorFlow的特性
### 1 灵活性
TensorFlow不是一个严格的神经网络工具包，只要你可以使用数据流图来描述你的计算过程，你可以使用TensorFlow做任何事情。你还可以方便地根据需要来构建数据流图，用简单的Python语言来实现高层次的功能。

### 2 可移植性
TensorFlow可以在任意具备CPU或者GPU的设备上运行，你可以专注于实现你的想法，而不用去考虑硬件环境问题，你甚至可以利用Docker技术来实现相关的云服务。

### 3 提高开发效率
TensorFlow可以提升你所研究的东西产品化的效率，并且可以方便与同行们共享代码。

### 4 支持语言选项
目前TensorFlow支持Python和C++语言。（但是你可以自己编写喜爱语言的SWIG接口）

### 5 充分利用硬件资源，最大化计算性能


## TensorFlow安装
```
Install HomeBrew
$ ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)”
Install PIP
sudo easy_install pip
Install Tensorflow
pip install tensorflow==1.5
```
因为Tensorflow支持的numpy科学计算库不高于1.14.5 不小1.13.5所以我们安装1.14.5来确保Tensorflow 可以正确的使用.

```
Install numpy
pip install numpy==1.14.5
```
一般安装Tensorflow时候 会在gracio时候卡顿一段时间,所以我们优先下载可以更快的安装

```
Install grpcio
pip install grpcio==1.9
```

```python
测试(Test)
import tensorflow as tf
import numpy as np

# 使用 NumPy 生成假数据(phony data), 总共 100 个点.
x_data = np.float32(np.random.rand(2, 100)) # 随机输入
y_data = np.dot([0.100, 0.200], x_data) + 0.300

# 构造一个线性模型
# 
b = tf.Variable(tf.zeros([1]))
W = tf.Variable(tf.random_uniform([1, 2], -1.0, 1.0))
y = tf.matmul(W, x_data) + b

# 最小化方差
loss = tf.reduce_mean(tf.square(y - y_data))
optimizer = tf.train.GradientDescentOptimizer(0.5)
train = optimizer.minimize(loss)

# 初始化变量
init = tf.initialize_all_variables()

# 启动图 (graph)
sess = tf.Session()
sess.run(init)

# 拟合平面
for step in xrange(0, 201):
    sess.run(train)
    if step % 20 == 0:
        print step, sess.run(W), sess.run(b)

# 得到最佳拟合结果 W: [[0.100  0.200]], b: [0.300]
```

## 基于 VirtualEnv 的安装
首先, 安装所有必备工具

```
# 在 Linux 上:
$ sudo apt-get install python-pip python-dev python-virtualenv

# 在 Mac 上:
$ sudo easy_install pip  # 如果还没有安装 pip
$ sudo pip install --upgrade virtualenv
接下来, 建立一个全新的 virtualenv 环境. 为了将环境建在 ~/tensorflow 目录下, 执行:

$ virtualenv --system-site-packages ~/tensorflow
$ cd ~/tensorflow
然后, 激活 virtualenv:

$ source bin/activate  # 如果使用 bash
$ source bin/activate.csh  # 如果使用 csh
(tensorflow)$  # 终端提示符应该发生变化
在 virtualenv 内, 安装 TensorFlow:

(tensorflow)$ pip install --upgrade <$url_to_binary.whl>
接下来, 使用类似命令运行 TensorFlow 程序:

(tensorflow)$ cd tensorflow/models/image/mnist
(tensorflow)$ python convolutional.py
# 当使用完 TensorFlow
(tensorflow)$ deactivate  # 停用 virtualenv
$  # 你的命令提示符会恢复原样
```