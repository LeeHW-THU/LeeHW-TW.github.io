---
layout:     post
title:      "「DL」——「Keras入门」—— MNIST"
subtitle:   " 记录打码。 "
date:       2019-03-13 
author:     "LeeHW"
header-img: "img/post-bg-digital-native.jpg"
catalog: true
tags:
    - DeepLearning
---

## 环境

![Keras](https://img.shields.io/badge/Keras-2.2.4-green.svg)![Numpy](https://img.shields.io/badge/Numpy-1.16.0-green.svg)![python](https://img.shields.io/badge/python-3.6.0-green.svg)![Matplotlib](https://img.shields.io/badge/matplotlib-3.3.0-green.svg)

---

## 代码

### 	Step1——引入包

```python
import keras
from keras.models import Model,Sequential #贯序模型
#引入需要使用的层
from keras.layers import Input,Dense,Dropout,Activation 
from keras.optimizers import SGD
from keras.datasets import mnist #使用keras自带的MNIST数据
import numpy as np
import matplotlib.pyplot as plt
```

### 	Step2——数据预处理

```python
(x_train, y_train), (x_test, y_test) = mnist.load_data() # 下载mnist数据集
#
print(x_train.shape,y_train.shape) 
# (60000, 28, 28) (60000,) 训练集
print(x_test.shape,y_test.shape)
# (10000, 28, 28) (10000,) 测试集

x_train = x_train.reshape(60000,784) # 将图片摊平，变成向量
x_test = x_test.reshape(10000,784) 
print(x_train.shape)
#(60000, 784)
print(x_test.shape)
#(10000, 784)

#对数据进行归一化处理
x_train = x_train / 255
x_test = x_test / 255


#对y标签进行处理——onehot
y_train = keras.utils.to_categorical(y_train,10)
y_test = keras.utils.to_categorical(y_test,10)

```

### 	Step3——构建神经网络

```python
model=Sequential() #定义模型采用贯序模型方式
#输入层
model.add(Dense(512, activation = 'relu',input_shape=(784,)))
model.add(Dropout(0.2))

#隐藏层
model.add(Dense(512, activation = 'relu'))
model.add(Dropout(0.2))

#隐藏层
#分成10类
model.add(Dense(10, activation = 'softmax'))

#打印模型概况
model.summary()

```

### 	Step4——网络优化器

```python
#定义SGD参数

# lr 学习率
# momentum 动量
# decay 每次更新后的学习率衰减值
# nesterov 确定是否使用Nesterov动量
sgd=SGD(lr=0.01,decay=1e-6,momentum=0.9,nesterov=True)

#定义优化器
#使用刚刚定义的SGD优化器，损失函数使用交叉熵
model.compile(optimizer=sgd,loss='categorical_crossentropy',metrics=['accuracy'])
#（可选）定义绘制acc-loss曲线的函数
class LossHistory(keras.callbacks.Callback):
    def on_train_begin(self, logs={}):
        self.losses = {'batch': [], 'epoch': []}
        self.accuracy = {'batch': [], 'epoch': []}
        self.val_loss = {'batch': [], 'epoch': []}
        self.val_acc = {'batch': [], 'epoch': []}

    def on_batch_end(self, batch, logs={}):
        self.losses['batch'].append(logs.get('loss'))
        self.accuracy['batch'].append(logs.get('acc'))
        self.val_loss['batch'].append(logs.get('val_loss'))
        self.val_acc['batch'].append(logs.get('val_acc'))

    def on_epoch_end(self, batch, logs={}):
        self.losses['epoch'].append(logs.get('loss'))
        self.accuracy['epoch'].append(logs.get('acc'))
        self.val_loss['epoch'].append(logs.get('val_loss'))
        self.val_acc['epoch'].append(logs.get('val_acc'))

    def loss_plot(self, loss_type):
        iters = range(len(self.losses[loss_type]))
        #创建一个图
        plt.figure()
        # acc
        plt.plot(iters, self.accuracy[loss_type], 'r', label='train acc')#plt.plot(x,y)，这个将数据画成曲线
        # loss
        plt.plot(iters, self.losses[loss_type], 'g', label='train loss')
        if loss_type == 'epoch':
            # val_acc
            plt.plot(iters, self.val_acc[loss_type], 'b', label='val acc')
            # val_loss
            plt.plot(iters, self.val_loss[loss_type], 'k', label='val loss')
        plt.grid(True)#设置网格形式
        plt.xlabel(loss_type)
        plt.ylabel('acc-loss')#给x，y轴加注释
        plt.legend(loc="upper right")#设置图例显示位置
        plt.show()

#创建一个实例LossHistory
history = LossHistory()
```

### 	Step5——训练模型

```python
model.fit(x_train, y_train, batch_size=128, epochs=25,validation_data=(x_test,y_test),callbacks=[history])#callbacks传入history

```

### 	Step6——模型评估

```python
score = model.evaluate(x_test,y_test)
print("loss:",score[0])
print("accu:",score[1])
history.loss_plot('epoch')#打印结果图
```

![keras_MNIST](http://lihongwei.site/img/keras_MNIST.png)

---

## 结语

新手适用。

源码链接：[Keras_MNIST](https://github.com/LeeHW-TW/Try_DL/blob/master/Learn_MNIST.py)