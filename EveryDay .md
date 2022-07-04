# Day 1

## 学习常用的网络结构：

### 1.残差网络：

在深层卷积神经网络中，梯度优化更困难，残差网络将输入值x经过权重层和激活函数得到的结果与原来的x的值相加，在经过一个激活函数得到输出结果，解决了难以训练的问题，提高训练速度。具体有两种方式：

![image-20220629122704349](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220629122704349.png)
左图对应输入通道为64时，用两个(3,3)的卷积，中间不改变通道数，最后**相加激活**得到输出。右图对应于输入通道为256时，先用一个(1,1)的卷积把**通道数缩小为1/4**，然后在这个通道数上进行(3,3)的卷积，最后再经过一个(1,1)的卷积改为原通道，然后**相加激活**的到输出。

**ResNet34中使用的是左图的残差结构，ResNet50/101/152使用的是右图的残差结构。**

### 2.深度可分离卷积：

在**轻量化的卷积网络**中应用广泛。它的结构如下图：

![image-20220629123205219](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220629123205219.png)

对于一个输入矩阵，我先用一个(3,3)的卷积，各个通道**不相加**，这个过程其实就是**分组卷积**，**分组的个数等于输入通道数**，之后再用(1,1)的卷积改变**输出通道数**。

深度可分离卷积**极大地降低了卷积的参数量**。

### 3.SE注意力机制：

**SE注意力机制**希望模型可以**自动学习到不同channel特征的重要程度**，因为很显然各个通道间**所包含特征的重要程度是不一样的**。它的结构如下：

![image-20220629123820679](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220629123820679.png)

通过一个**全局平均池化**，将输入的大小压缩成1*1，通道数不变，对这个一维矩阵做激励，得到的这样的一个一维矩阵相当于是各个通道的权重，将这个一维矩阵与原输入相乘，得到一个重要成不同的输出，这样就区分出了各个通道的重要性。

原始SE注意力机制的**压缩**和**激励**如下：

![image-20220629124755627](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220629124755627.png)

对**ResNet**和**MobileNet**使用**注意力机制**：

![image-20220629130544659](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220629130544659.png)

**ResNet**中的**SE注意力机制**放在了(1,1)卷积之后，激励部分使用的是**全连接层**。MobileNetV3中**SE注意力机制**放在了(1,1)卷积之前，并且激励部分使用的是**卷积层**。

------



## 论文泛读——方面级情感三元组的分析：

抽取方面词、观点词、关联情感--->跨域共享联合抽取(从句子中端到端提取aspect情感三元组)

### 1.跨域共享联合抽取：

首先利用基编码器学习单词级表示，并利用图卷积网络在句子的句法依存树上获取单词的依存信息，融合到跨度表示中；然后，列举并过滤句子中所有可能的跨度，生成候选跨度，这些候选跨度可以作为方面词和观点词共享；最后，将两个分别作为方面词和观点词的候选词及其对应的局部上下文输入分类器，一次性生成情感三元组。

### 2.GCN（图卷积神经网络）

解决图结构数据，不同于CNN、RNN模型，但是和CNN的本质一样，都是特征提取器，只不过他的对象是图数据。GCN用这些特征进行节点分类，图分类，边预测。

什么结构：有N个节点的N*D维节点特征组成的矩阵X和   各个节点之间的关系N * N维的矩阵A加上单位矩阵I在进行归一化    相乘，加上一个权重，再激活一下。

**即使不训练，完全使用随机初始化的参数W，GCN提取出来的特征就以及十分优秀了！**这跟CNN不训练是完全不一样的，后者不训练是根本得不到什么有效特征的。



# Day 2


MobilenetV3轻量级网络。

主要结构：

1.深度可分离卷积

2.SE注意力机制、

3.新型的激活函数：Hardswish激活函数
![image-20220630183931997](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220630183931997.png)

Hardswish函数在梯度上存在突变，对于训练的精度不利，但是在深层网络中影响较小，所以可以在浅层网络中可以用Relu函数代替。

4.修改网络结构：缩减卷积层并没有减少太多精度
从MobilenetV2到MobilenetV3的结构改动：V2使用了四层卷积再接一个平均池化；V3只用一个卷积层修改通道数直接接了平均池化层，大大减少网络参数量。
整个的V3网络是一个残差网络结构，内嵌SE注意力机制，通过1* 1卷积改变通道数.
![image-20220702123331510](https://github.com/nongfulv2/Assessment-of-the-summer-vacation/blob/main/My%20Photo/image-20220702123331510.png)

# Day 3

系统复习YOLO v3网络结构并总结，为便于复习查阅，见单独文件

# Day 4

系统复习YOLO v3网络结构并总结，为便于复习查阅，见单独文件