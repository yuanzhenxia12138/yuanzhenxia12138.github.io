---
layout:     post
title:      个人博客构建(free)
subtitle:   github page + jekyll
date:       2018-8-27
author:     GY
header-img: img/post-bg-desk.jpg
catalog: true
tags:
    - Deep Leaning
---
## [神经网络](https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650742297&idx=4&sn=0f6ec85f71a00191e7a7d2c6b0321e8c&chksm=871ada67b06d537171f4715b75a36efd4686fe2386d803c86c9666d8276851192d5158c5c660&mpshare=1&scene=1&srcid=0523IxDYbrlca8pqXgldrrmT#rd)

* ANN(artificial neural network):神经网络具有以下三个部分：

1.  **结构**（Architecture）结构指定了网络中的变量和它们的拓扑关系。

1.  **激励函数**（Activity Rule）

1.  **学习规则**（Learning Rule）学习规则指定了网络中的权重如何随着时间推进而调整。


##  [DeepLearning（机器之心）](http://mp.weixin.qq.com/s/xUOkbXYsZAjjZN6hAsk2zw)

* 深度学习是机器学习的**分支**，它试图使用包含复杂结构或者由多重非线性变换构成的多个处理层 对数据进行高层抽象的算法 。简单的说，深度学习就是一个**函数集**.

### 基础

1. 数学基础：掌握数学分析、线性代数、**概率论**和凸优化四门数学课程包含的数学知识，熟知机器学习的基本理论和方法，是入门深度学习技术的前提。

2.  计算机基础：C++、python; ubuntu; CUDA

### 理论

1. 基础结构单元

全连接层、pooling层、dropout层、BN层、RNN/LSTM

* 卷积层/反卷积层

**卷积核大小**:较大的卷积核会使得网络难以发散,可以使用 1×1 的核来减少特征的数目

**卷积核size为奇数**：

保护位置信息：保证了锚点刚好在中间，方便以模块中心为标准进行滑动卷积，避免了位置信息发生 偏移。

padding时对称：保证padding 时，图像的两边依然相对称 。

**空洞卷积**（Dilated Convolution）在卷积核的权重之间使用空格。这使得网络不用增加参数数目就能够指数级地扩展感受野，也就是说根本没有增加内存消耗。已经证明，空洞卷积可以在微小的速度权衡下就能增加网络准确率。

2. 激活函数

很好的一个经验法则就是从 ReLU 开始。

3. Softmax等损失函数

softmax loss、sigmoid交叉镝

4. 网络训练方法，包括BP、Mini-batch SGD和LR Policy。

5. 深度网络训练中的两个至关重要的理论问题：梯度消失和梯度溢出。

### 经典网络结构

* [深度学习论文翻译](https://github.com/SnailTyan/deep-learning-papers-translation)

[**AlexNet**](http://note.youdao.com/noteshare?id=f51ba391a5dd6b62a8d28a78bd43ac9b&sub=4BDC40907A7A4D67A43173D33630C24A)

* 网络加深

VGG16 VGG19 MSRAnet

* 增强卷积模块功能

NIN GoogleNet Inception V3  Inception V4

**融合上述两类**

ResNet  Inception ResNet PVANet DenseNet PlayNet

1. ==**BaseModel**==:

先用ResNet-50来验证算法的有效性；

当该算法在ResNet-50上切实有效后，如果要追求算法速度(例如落地到移动端)，则将basemodel替换为Xception(较常用的是Xception-145)、ShuffleNet；如果要追求精度(例如发论文、打比赛刷榜)，则将basemodel替换为ResNet-101/ResNeXt/DPN。

2. Basemodel部分，一般直接导入现成训练好的。之后在自己的数据集上**fine-tune**整个网络

* 从分类任务到检测任务

YOLO SSD

SPP-net R-CNN  Fast R-CNN Faster R-CNN MASK R-CNN

FPN + Faster R-CNN

* 增加新的功能单元

Inception V2(BN) FCN FCN+CRF ST-Net CNN+RNN/LSTM GAN

### Benchmark

* [cnn-benchmarks](https://github.com/jcjohnson/cnn-benchmarks)

Inception 和 ResNet 的选择确实是速度和准确率的权衡：要**准确率**，用超深层的 **ResNet**；要**速度**，用 **Inception**。

* 人脸识别领域

LFW和MegaFace

* 图像识别领域与物体检测领域

ImageNet、Microsoft COCO

* 图像分割领域

Pascal VOC等

### 关注点

* 新的网络结构

* 新的优化方法

1. 使用 Adam，中间换到 SGD，能够以最容易的方式达到最好的准确率。

2. 《Learning gradient descent bygradientdescent》以及SWISH激活函数

3. 当前的优化方法或ReLU激活函

* 新的学习技术。深度强化学习和生成对抗网络（GAN）。

* 新的数据集

##  博客资源

* [设计卷积神经网络](https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650742208&idx=1&sn=d17bccd04b9991cf93486e43cf841631&chksm=871ad9beb06d50a83ed6f392609773e190c02acfc1a297a7ded84aaa4dd68a9888cd43c29467&mpshare=1&scene=1&srcid=0523VjrUpcVU5cPyztsBkBg1#rd)

使用巧妙的**卷积设计**来减少**运行时间和内存消耗**

1. [MobileNets](https://arxiv.org/pdf/1801.04381.pdf)使用**深度分离的卷积**来极大地减少运算和内存的消耗

2.  [XNOR-Net](https://arxiv.org/pdf/1603.05279.pdf)使用**二进制卷积**，也就是说，卷积运算只涉及两个可能的数值：0 或者 1。通过这种设计，网络可以具有**较高程度的稀疏性，易于被压缩**而不消耗太多内存。

3. [ShuffleNet](https://arxiv.org/pdf/1707.01083.pdf)使用**点组卷积**和**通道随机化**来极大地减少计算代价

4. [Network Pruning](https://arxiv.org/pdf/1605.06431.pdf)是为了减少运行时间和内存消耗而**删除 CNN 的部分权重**的技术


* [Blog1](https://blog.csdn.net/jningwei/article/category/6977224/4?)

* [Blog2](https://blog.csdn.net/zouxy09/article/details/8781543)

## 在线课程

* C231n

1. [Tutorial](http://cs231n.github.io/)

1. [translation](https://www.jianshu.com/p/182baeb82c71)

1. [assignment1-3](https://github.com/lightaime/cs231n)

## 文本资源

* [NN & DL](https://mooc.study.163.com/course/2001281002#/info)

* 西瓜书

* deepleaning( theory)

1. [English PDF](http://www.deeplearningbook.org/)


2. [Chinese PDF](https://exacity.github.io/deeplearningbook-chinese/)


3. [website](https://github.com/exacity/deeplearningbook-chinese)

##  扩展

* [如何优化神经网络超参数](https://towardsdatascience.com/gas-and-nns-6a41f1e8146d)

* [梯度下降](https://hackernoon.com/gradient-descent-aynk-7cbe95a778da)

*  [预训练模型](http://www.cnblogs.com/neopenx/p/4575527.html)

*  [2](https://mp.weixin.qq.com/s/O5zkZJsUa72lrQDCn5RyEw)

*  [Sodtmax](https://mp.weixin.qq.com/s/XBK7T1P7z3rm3o-3BDNeOA)

*  [LISM](https://mp.weixin.qq.com/s/sHqCkzIbe3bYDXAVn6rdlw)

*  [模型优化](https://mp.weixin.qq.com/s/KrtEFTJDqGeGxRZPVGU_IA)

*  [优化器](https://mp.weixin.qq.com/s/CQpIhVinDPhXpp70WhyYww)

*  [神经网络模型压缩和加速](https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247488630&idx=1&sn=894b06c31b37ccdad3e9bfdd7323a33f&chksm=96e9cbf6a19e42e0c666d6727430a39fe4e09db047c3cfc0465a34923b87a36dfbe7585fe339&mpshare=1&scene=1&srcid=0523pBxDJJl6nZkfa49Iqbbm#rd)

## 项目实践

*  [DL 项目合集](https://mp.weixin.qq.com/s/iszzOubuS0PJ35jQzddmSg)


* [开源 | Intel发布神经网络压缩库Distiller：快速利用前沿算法压缩PyTorch模型](https://mp.weixin.qq.com/s/A5ka8evElmcuHdowof7kww)

* [GluonCV ](https://github.com/dmlc/gluon-cv)


提供了 CV 方向的顶尖深度学习模型实现。

* [PyTorch 框架中的 model.summary() 功能](https://github.com/sksq96/pytorch-summary)

* [DL Project Template](https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247488856&idx=1&sn=73a95b4c4d68209ad049765dcb07dc4b&chksm=96e9cad8a19e43ce8bcfdbf51365e576b2f6b210533854364281b3cc22a87cf1c817f1d88360&mpshare=1&scene=1&srcid=05239L3FpsewcZQffooZRBAm#rd)

* [ Feature Tools：可自动构造机器学习特征的Python库](https://mp.weixin.qq.com/s/wIaDAewwOBMPRDr28DqGoQ)


## others

*  [动漫画着色项目](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650741014&idx=1&sn=c8af8b8fcba41a16e5575cc310e6c743&chksm=871add68b06d547e47f87989c9c47ca88c25a6a9216459877a5c661262a9a8ccea619358cc37&scene=0#rd)

*  [TensorEditor：可视化搭建网络工具 ](https://mp.weixin.qq.com/s/f3pRA_lyT5jfzWimy6CleA)

*  [数据增强库](https://github.com/aleju/imgaug)
