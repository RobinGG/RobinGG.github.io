---
layout: page
title: 深度学习第一步：使用 Keras 学习 MNIST
tags: [Deep Learning]
---

今天尝试了一下用 Keras 框架去学习 mnist 的手写字。算是走出了深度学习的第一步。记录一下代码并加深一下理解

## 简短代码

~~~python
from keras.models import Sequential
from keras.layers.core import Dense, Dropout, Activation
from keras.optimizers import SGD
from keras.datasets import mnist

import numpy

model = Sequential()

model.add(Dense(512, input_shape=(784,)))
model.add(Activation("tanh"))
model.add(Dropout(0.2))

model.add(Dense(512))
model.add(Activation('tanh'))
model.add(Dropout(0.2))

model.add(Dense(10))
model.add(Activation('softmax'))

sgd = SGD(lr=0.01, decay=1e-6, momentum=0.9, nesterov=True)

model.compile(sgd, 'categorical_crossentropy')

(X_train, y_train), (X_test, y_test) = mnist.load_data()

X_train = X_train.reshape(X_train.shape[0], X_train.shape[1] * X_train.shape[2])
X_test = X_test.reshape(X_test.shape[0], X_test.shape[1] * X_test.shape[2])

Y_train = (numpy.arange(10) == y_train[:, None]).astype(int)
Y_test = (numpy.arange(10) == y_test[:, None]).astype(int)

model.fit(X_train, Y_train, batch_size=100, epochs=100, shuffle=True, verbose=1)

score = model.evaluate(X_test, Y_test, batch_size=100, verbose=1)
print score
~~~

## 代码分析
我们一行行一段段地进行代码的分析，毕竟初学者，慢慢来。

1. 定义模型
    ~~~python
    model = Sequential()
    ~~~

    这一行代码是将我们的神经网络的模型指定为序贯模型。序贯模型是一个比较基础的模型，在这个模型中，神经网络的每一层都是顺序连接的。

2. 加入网络层
    ~~~python
   model.add(Dense(512, input_shape=(784,)))
    model.add(Activation("tanh"))
    model.add(Dropout(0.2))

    model.add(Dense(512))
    model.add(Activation('tanh'))
    model.add(Dropout(0.2))

    model.add(Dense(10))
    model.add(Activation('softmax'))
    ~~~
    这一段代码是为我们的神经网络模型增加神经层。

    Dense 是常用的全连接层，第一参数为节点数，第二个参数为输入张量。

    Activation 对一个层的输出施加激活函数。需要说明的是最后一个激活函数可以将结果“归一化”。

    Dropout 可以随机断开神经元连接，防止学习结果过拟合。

3. 编译
    ~~~python
   sgd = SGD(lr=0.01, decay=1e-6, momentum=0.9, nesterov=True)

    model.compile(sgd, 'categorical_crossentropy')
    ~~~

    在正式训练前通过 compile 对神经网络进行配置，必须要指定的分别是损失函数和优化器。

4. 加载数据
    ~~~python
   (X_train, y_train), (X_test, y_test) = mnist.load_data()

    X_train = X_train.reshape(X_train.shape[0], X_train.shape[1] * X_train.shape[2])
    X_test = X_test.reshape(X_test.shape[0], X_test.shape[1] * X_test.shape[2])

    Y_train = (numpy.arange(10) == y_train[:, None]).astype(int)
    Y_test = (numpy.arange(10) == y_test[:, None]).astype(int)
    ~~~

    mnist 中的数据都是 28*28 的二维数组，这里处理为了长度为 784 的一维数组。这也就是为什么我们的第一个输入层是 784 个输入了。

5. 开始训练
    ~~~python
    model.fit(X_train, Y_train, batch_size=100, epochs=100, shuffle=True, verbose=1)
    ~~~

    batch_sise 是每次训练选取的样本数量；epochs 是训练一共进行的轮次；shuffle 指数据会随机打乱后进行再训练；verbose 是制定控制台输出内容的详细程度。

    在 mnist 数据集中一共有 60000 条数据，在我的 Macbook-Pro 按照代码中的设置，训练完毕大概要花费 10 分钟。

6. 评估训练结果
    ~~~python
   score = model.evaluate(X_test, Y_test, batch_size=100, verbose=1)
    print score
    ~~~

    在默认情况下，返回结果是测试误差的标量值。我们可以在 compile 的时候加上其他评估标准。

---
完整代码可以从 gist 获取：
[RobinGG/keras_mnist.py](https://gist.github.com/RobinGG/ee35535d0ce66381ef168ed2f279438b)

## 参考
1. [Keras 中文文档](https://keras-cn.readthedocs.io/en/latest/)
2. [Keras 浅尝之MNIST手写数字识别](http://friskit.me/2015/06/29/keras-mnist/)
3. [使用Keras构建一个简单的神经网络](http://blog.just4fun.site/others/keras-mnist-tutorial.html)