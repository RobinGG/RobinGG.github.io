---
layout: page
title: 深度学习第二步：使用卷积神经网络学习 MNIST
tags: [Deep Learning]
---

自从上次卖出了[第一步](../keras-mnist/)以后就老想着迈第二步。今天这第二步算是迈出来了：就是通过卷积神经网络学习 MNIST。

关于什么是卷积神经网络往上有大量的博客、论文说明。比如：[Wiki: 卷积神经网络](https://zh.wikipedia.org/wiki/%E5%8D%B7%E7%A7%AF%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C)

## 简短代码

~~~python
from keras.datasets import mnist
from keras.layers import Conv2D, Flatten, Dense, MaxPooling2D
from keras.models import Sequential
from keras.utils import np_utils

(x_train, y_train), (x_test, y_test) = mnist.load_data()
print "x_train shape: {}".format(x_train.shape)
print "y_train shape: {}".format(y_train.shape)
x_train = x_train.reshape(x_train.shape[0], 28, 28, 1)
x_test = x_test.reshape(x_test.shape[0], 28, 28, 1)

y_train = np_utils.to_categorical(y_train, 10)
y_test = np_utils.to_categorical(y_test, 10)

print "x_train shape: {}".format(x_train.shape)
print "y_train shape: {}".format(y_train.shape)

model = Sequential()

model.add(Conv2D(6, (3, 3), activation="tanh", input_shape=(28, 28, 1)))
# model.output_shape should be (None, 26, 26, 6)
print model.output_shape
# model.add(Activation("tanh"))
model.add(MaxPooling2D(pool_size=(2, 2)))
# model.output_shape should be (None, 13, 13, 6)
print model.output_shape

model.add(Conv2D(16, (2, 2), activation="tanh"))
# model.output_shape should be (None, 12, 12, 16)
print model.output_shape
# model.add(Activation("tanh"))
model.add(MaxPooling2D(pool_size=(2, 2)))
# model.output_shape should be (None, 6, 6, 16)
print model.output_shape

model.add(Flatten())
# model.output_shape should be (None, 576)
print model.output_shape

model.add(Dense(256, activation="tanh"))
# model.output_shape should be (None, 256)


model.add(Dense(10, activation="softmax"))
# model.output_shape should be (None, 10)
print model.output_shape

model.compile("adadelta", "categorical_crossentropy", metrics=['accuracy'])

model.fit(x_train, y_train, batch_size=128, epochs=12, validation_data=(x_test, y_test))

score = model.evaluate(x_test, y_test, verbose=1)
print('Test score:', score[0])
print('Test accuracy:', score[1])

model.save("keras_mnist_cnn.h5")
~~~

## 代码改动

这一次与[上一次]((../keras-mnist/))的代码大致相同，主要是有几个不同的网络层：Conv2D，MaxPooling2D 和 Flattern

### Conv2D

~~~python
model.add(Conv2D(6, (3, 3), activation="tanh", input_shape=(28, 28, 1)))
# model.output_shape should be (None, 26, 26, 6)
print model.output_shape
~~~

Conv2D 就是今天的主角卷积层了。函数的主要作用就是做 2 维的张量做卷积，其实就是对图片做卷积了。输出是卷积核对图像做卷积后的特征值。

在展示的这段代码中，第一个参数定义了卷积核的数量是 6，所以对应的也会有 6 个卷积后的特征值输出。第二个参数定义了卷积核的大小是 3*3。第三个顾名思义是激活函数。最后一个参数是输入的“shape”，这里引用一段[中文翻译的官方文档](https://keras-cn.readthedocs.io/en/latest/layers/convolutional_layer/):
> ‘channels_last’模式下，输入形如（samples，rows，cols，channels）的4D张量
> 

在我实践了一下以后发现实际上 samples 不需要定义在 shape 里面。所以 input_shape 就变成了 (28,28,1)。两个 28 分别表示 MNIST 数据集里面数据的高和宽，最后一个 1 则表示“channel”。对于彩色图片来说，一般使用 RGB 模式来表示颜色，就具有三通道。但是 MNIST 里面的数据只有黑色，单通道。

### MaxPooling2D

卷积之后就是池化层。在经过池化之后，经过卷积得出来的特征就会被模糊掉细节，保留一个整体的特征。个人不是很理解其中深层次的原因，所以就不在这里输入展开了。

### Flattern

Flattern 层可以讲多维的数据展开成为一维数据。有利于后续的全联接层处理

---

完整代码可以从 gist 获取：
[keras_mnist_cnn.py](https://gist.github.com/RobinGG/61528d27fb81c9c5240128fbf8f3fc83)

## 参考
1. [Keras 中文文档](https://keras-cn.readthedocs.io/en/latest/)
2. [Keras上实现卷积神经网络CNN](http://blog.csdn.net/marsjhao/article/details/68490105)
3. [3D Visualization of a Convolution Neural Network](scs.ryerson.ca/~aharley/vis/conv/
)