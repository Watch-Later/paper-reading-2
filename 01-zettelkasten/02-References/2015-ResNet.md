---
Alias: ResNet
title: Deep Residual Learning for Image Recognition
authors:
- Kaiming He
- X. Zhang
- Shaoqing Ren
- Jian Sun
fieldsOfStudy:
- Computer Science
meta_key: 2015-ResNet
numCitedBy: 95314
pdf_relpath: pdfs/2015-ResNet.pdf
ref_count: 61
status: todo
tags:
- cnn-model
- gen-from-ref
- paper
venue: 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR)
year: 2015
---

[local pdf](../../../pdfs/2015-ResNet.pdf)

# ResNet

## 阅读价值

论文评价：

1. 作者的写作，很厉害。值得学习。
2. 实验做的太好了，把 residual learning 拿过来，效果非常好。
3. 给了一些直观的解释，5 年后回看，大家不太认同作者当时的解释。

阅读目的：

1. 理解 residual learning 的网络结构 & 实验结果。会用。
2. 理解 ResNet50 的网络结构，会算 FLOPS、内存占用。做计算硬件和框架的，都爱举这个例子。
3. #TODO 手写 ResNet18，在 cifar10 上手动训练一下。

个人理解：deep learning 还是实验 driven 的领域。事后给理论解释。

ResNet 为什么训练的快？
因为加了 identify mapping 项以后，求导时，梯度保持的更好。没加 identity mapping 时，求导结果是连乘，数非常小。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3wwkikhxbj20o80cudh2.jpg)

SGD solver 的精髓是，你要能一直跑得动。即，需要梯度一直够大。
收敛是没有意义的，只是 SGD train 不动了。

为什么 ResNet 1202-layer 在 cifar10 上 overfit 不严重？

1. 这还是一个 open question。现在的 transform 模型，也存在相同的问题。
2. 一个解释，加了 identify mapping shortcut 以后，使得模型本身的复杂度降低。当模型能更快的找到不那么复杂的权重时，就不会再去找更复杂的。

## Introduction

提出问题：网络的深度很重要，但深了以后，很难训练。

其中一个问题是：梯度爆炸/梯度消失。通过 normalized initialization + Batch Norm 可以把网络加深几十层。

引入新的问题：当网络更深以后，train error 反而会变差。

这是不合理的。假设新加的层都是 identity mapping，那么，deeper 的网络至少不应该比浅的更差。实验结果却并非如此。

本文思路：通过残差学习（residual learning）解决这个问题。

个人理解：

1. deeper 的网络，为什么学不到 identity mapping，是个现象，作者也说不清楚。
2. 既然学不到，那么就把 identify mapping 加到网络结构上，不用去拟合了。原本的子网结构，去学习残差。如果学到了 0，那么合起来就是 identify mapping。这比直接学 identify mapping 容易。
3. 原本的子网结构，改为学习残差，学习的难度应该基本不变。

上面是数学原理，实现上，就是加个 shortcut connection。这个方法，在 90 年代有不少 paper 讨论过，并不是全新的东西。

这个改动，基本没有引入新的参数，所以，不会使训练变得更难。

## Related Work

### Residual Representation

传统机器学习、计算机视觉领域，用的其实挺多的。

Gradient Boosting （GBDT，一种决策树算法）就是通过残差去学习一个网络。

计算机视觉中，解偏微分方程（PDE）也会使用类似的残差表示方法。

上述方法都暗示了：a good reformulation or preconditioning can simplify the optimization.

### Shortcut Connections

很多工作，都尝试在网络结构里加入一些直连的结构。

个人观点：这有点像 cpu pipeline 里加入 [流水线寄存器](../05-Notes%20Block/MIPS%20CPU%20Pipeline%20Introduction.md)。分层的设计里，当问题复杂了以后，基本都会开始加层间的 shortcut connection。区别只是怎么加。

同时期的工作，比如 highway network，也在做类似的事情。加了 gate function。这些 gate 是 data-dependent，且有参数。可以通过参数，把 gate 关掉。

个人观点：这个很有意思，highway network 为什么效果不好？他们为什么没有想到用 residual learning。可见，作者的工作，也不是读起来的那么容易想到。

## Network Architectures

### building block

主要是 2 部分：

1. 学习残差的 network：F(x)
2. 学习 identify mapping 的部分：x

残差部分，是可以灵活设置的。比如，下图的 2 种方案。

左侧：2 层 conv + 1 层 ReLU。
右侧：针对 deeper network 设计的 building block。后面详细解释。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3w54nfzpbj20ry0faq4a.jpg)

### Identity Mapping by Shortcuts

对 50-layer 及以上，很重要。概念上，不太好理解，我第一遍没理解到位。误以为是没什么用的数学废话。

注意公式 2 里，projectio(投影) 的概念。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3whda28zdj20oo09cta2.jpg)

σ 是激活函数，ReLU

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3whe9b7bzj20oe0cwdi7.jpg)

这里引入一个问题，F(x) 里的卷积会改变输出的维度数量，导致没法与 x 直接矩阵运算。

#TODO 下面的内容，还需要再理解一下。

数学上，方案有 2 个：
1. 给 x 乘个矩阵，改变维度。
2. 用 1x1 的卷积核给 x 改变维度。与 F(x) 里卷积核改变输出维度的原理相同。

一般用选 1 * 1 卷积核的方案，只对输入、输出维度不一致层使用。

实现上，3 个选择：

1. zero-padding shortcut
2. 只对输入、输出不一样的层做投影(projection)，其他层 identify mapping
3. 所有层都用投影，projection

实验结论，方案 2 最好。即，大家都选 ResNet B 方案。

### 计算量分析

参数量比 vgg 少很多。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3w6gp2kxwj20eq10c77v.jpg)

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3w6f5a2hjj210w0gmdjd.jpg)

图怎么看：

1. 第一个图，34-layer residual 一列，一组颜色是一组 conv，对应表格中的一大行。
2. 表格的 cell 里，一个矩阵表示一个 building block，* N 表示重复多少次。ReLU 的参数量，都省掉了。34 layer 的 block 是 2 个 3 * 3 的卷积核，64 输出 channel。
3. 50-layer 的 building block 复杂了一些。2 个 1 * 1， 中间夹了一个 3 * 3 卷积
4. layer 数的计算，以 34-layer 为例，(3+4+6+3) * 2 = 32, 然后 + 开始的 conv1 + 最后的 FC 层
5. FLOPS 的计算。
	1. 卷积层的 flops = 输入的高 * 宽 * 输入通道数 * 输出通道数 * kernel 的高 * kernel 的宽
	2. 全连接层的 flops
6. 内存/显存的计算。 #TODO

## 实验

### 实验细节设定

1. 短边随机缩放到 [256, 480] 做数据增强。crop 时可以提供更多的随机性
2. 不用 dropout，因为没有全连接层了。[improving-neural-networks-by-preventing-co-adaptation-of-feature-detectors](improving-neural-networks-by-preventing-co-adaptation-of-feature-detectors.md)

### ResNet 18-layer & 34-layer

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3wcgwocjqj21fw0i478a.jpg)

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3wd4i0u8nj20ps0a8wfl.jpg)

注：
1. 细的线，training error
2. 粗的线，validation error

主要结论：
1. 纵向。ResNet 34 比 ResNet 18 提升 2.8%，相比 plain net 提升显著。
2. 横向。ResNet 34 比 plain 34 高 3.5%，证明了在 deeper 的网络上，residual learning 有用。
3. 18-layer，精度没看到提升，但收敛速度明显快。说明，当网络不够深当时候，SGD solver 够用。当然，residual learning 也有用，能提升训练速度。

未解之谜：plain network，网络深了，为什么 training error 反而差。

1. 这不是 overfit。
2. 不是梯度消失引起的。加了 BN 层，理论上不会梯度消失。作者也验证了训练时的梯度传播正常。
3. 为什么会这样，reason ... will be studied in the future.

一些次要现象：
1. 刚开始，validation error 明显低于 training error，因为 validation 没做数据增强，噪音少。
2. 即使是最后的时候，training error 也一直波动，但 validation error 则稳定。
3. 跳的地方，是手动调了学习率，乘了 0.1，步伐改了。什么时间跳，很玄学。现在不用这个方法了。

### Deeper (layer-50+) bottleneck Architectures

主要是优化 training time 和计算量的考虑，引入了这个新设计。

deeper 了以后，可以把 channel 设置的更大，从而学到更详细的 feature。比如，从 64 提升到 256 通道。提升 4 倍，运算量的提升是平方关系，16 倍。

新的 building block，一共 3 个 layer。

- 1 * 1 降维
- 3 * 3 卷积抽特征
- 1 * 1 升维（restore）

对比 ResNet-34, ResNet-50，FLOPS 基本相等。

这个结构里，如果不用 identify mapping shortcut，而是用 projection，模型的大小和训练时间，要翻倍。因为，shortcut 连接的两端是高维的。

注意，1 * 1 的计算量虽然明显低，但计算的效率上跟 3 * 3 差别不大。所以，ResNet50 在实际训练上，比 ResNet 34 还是贵了不少。

结果上，
1. 相同的计算量，可以把层数从 34 提升至 50。最终精度也提升了。
2. layer-101/152 的精度，也在进一步提升。证明了这个方法的有效性。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3wvs8ffg2j20pk0zsq7b.jpg)

## References

- [Very Deep Convolutional Networks for Large-Scale Image Recognition](./very-deep-convolutional-networks-for-large-scale-image-recognition.md)
- Deep Residual Learning for Image Recognition Supplementary Materials
- [ImageNet classification with deep convolutional neural networks](./imagenet-classification-with-deep-convolutional-neural-networks.md)
- [Deeply-Supervised Nets](./deeply-supervised-nets.md)
- [Understanding the difficulty of training deep feedforward neural networks](./understanding-the-difficulty-of-training-deep-feedforward-neural-networks.md)
- [Delving Deep into Rectifiers - Surpassing Human-Level Performance on ImageNet Classification](./delving-deep-into-rectifiers-surpassing-human-level-performance-on-imagenet-classification.md)
- [Batch Normalization - Accelerating Deep Network Training by Reducing Internal Covariate Shift](./batch-normalization-accelerating-deep-network-training-by-reducing-internal-covariate-shift.md)
- [Network In Network](./network-in-network.md)
- [Learning Multiple Layers of Features from Tiny Images](./learning-multiple-layers-of-features-from-tiny-images.md)
- Deep Learning Made Easier by Linear Transformations in Perceptrons
- Convolutional neural networks at constrained time cost
- [FitNets - Hints for Thin Deep Nets](./fitnets-hints-for-thin-deep-nets.md)
- [Fully Convolutional Networks for Semantic Segmentation](./fully-convolutional-networks-for-semantic-segmentation.md)
- [Object Detection Networks on Convolutional Feature Maps](./object-detection-networks-on-convolutional-feature-maps.md)
- Fisher Kernels on Visual Vocabularies for Image Categorization
- [Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition](./spatial-pyramid-pooling-in-deep-convolutional-networks-for-visual-recognition.md)
- [Going deeper with convolutions](./going-deeper-with-convolutions.md)
- [OverFeat - Integrated Recognition, Localization and Detection using Convolutional Networks](./overfeat-integrated-recognition-localization-and-detection-using-convolutional-networks.md)
- [Faster R-CNN - Towards Real-Time Object Detection with Region Proposal Networks](./faster-r-cnn-towards-real-time-object-detection-with-region-proposal-networks.md)
- [Rich Feature Hierarchies for Accurate Object Detection and Semantic Segmentation](./rich-feature-hierarchies-for-accurate-object-detection-and-semantic-segmentation.md)
- [Highway Networks](./highway-networks.md)
- [Training Very Deep Networks](./training-very-deep-networks.md)
- The devil is in the details - an evaluation of recent feature encoding methods
- [ImageNet Large Scale Visual Recognition Challenge](./imagenet-large-scale-visual-recognition-challenge.md)
- [Caffe - Convolutional Architecture for Fast Feature Embedding](./caffe-convolutional-architecture-for-fast-feature-embedding.md)
- [Improving neural networks by preventing co-adaptation of feature detectors](./improving-neural-networks-by-preventing-co-adaptation-of-feature-detectors.md)
- On the Number of Linear Regions of Deep Neural Networks
- [Exact solutions to the nonlinear dynamics of learning in deep linear neural networks](./exact-solutions-to-the-nonlinear-dynamics-of-learning-in-deep-linear-neural-networks.md)
- Object Detection via a Multi-region and Semantic Segmentation-Aware CNN Model
- [Fast R-CNN](./fast-r-cnn.md)
- [Microsoft COCO - Common Objects in Context](./microsoft-coco-common-objects-in-context.md)
- [The Pascal Visual Object Classes (VOC) Challenge](./the-pascal-visual-object-classes-voc-challenge.md)
- [Long Short-Term Memory](./long-short-term-memory.md)
- [Aggregating Local Image Descriptors into Compact Codes](./aggregating-local-image-descriptors-into-compact-codes.md)
- Accelerated Gradient Descent by Factor-Centering Decomposition
- [Neural Networks for Pattern Recognition](./neural-networks-for-pattern-recognition.md)
- [Maxout Networks](./maxout-networks.md)
- Backpropagation Applied to Handwritten Zip Code Recognition
- Learning long-term dependencies with gradient descent is difficult
- [Rectified Linear Units Improve Restricted Boltzmann Machines](./rectified-linear-units-improve-restricted-boltzmann-machines.md)
- Centering Neural Network Gradient Factors
- Pushing Stochastic Gradient towards Second-Order Methods -- Backpropagation Learning with Transformations in Nonlinearities
- [Product Quantization for Nearest Neighbor Search](./product-quantization-for-nearest-neighbor-search.md)
- Fast Surface Interpolation Using Hierarchical Basis Functions
- [Vlfeat - an open and portable library of computer vision algorithms](./vlfeat-an-open-and-portable-library-of-computer-vision-algorithms.md)
- Locally adapted hierarchical basis preconditioning
- Pattern Recognition and Neural Networks
- [Et al].
- A multigrid tutorial
- Modern Applied Statistics with S-Plus
- Visualizing and Understanding Convolutional Neural Networks
- Untersuchungen zu dynamischen neuronalen Netzen
- A multigrid tutorial, Second Edition
- [Efficient BackProp](./efficient-backprop.md)
