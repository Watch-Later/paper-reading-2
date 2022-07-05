---
Alias: AlexNet
title: ImageNet classification with deep convolutional neural networks
authors:
- A. Krizhevsky
- Ilya Sutskever
- Geoffrey E. Hinton
fieldsOfStudy:
- Computer Science
meta_key: imagenet-classification-with-deep-convolutional-neural-networks
numCitedBy: 80944
pdf_relpath: pdfs/2012-AlexNet.pdf
ref_count: 50
status: done
tags:
- gen-from-ref
- paper
- cnn-model
urls:
- https://papers.nips.cc/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html
venue: Commun. ACM
year: 2012
---

[local pdf](../../../pdfs/2012-AlexNet.pdf)

# ImageNet classification with deep convolutional neural networks

## 阅读价值

1. 开创性的 paper，但文章写的一般，阅读价值不大。
2. 主要讲实验怎么做的，缺少认知和理解。给了分析的部分，现在回看，也都不太准确。

## 10 年后回看 AlexNet

根据李沐的 paper reading 整理。

1. 训练输入是 raw RGB value，也就是 end to end。不再需要计算机视觉的抽特征了。很重要，也影响了后续的研究方向，但作者没有强调。
2. 作者最后的一张图（下图的右图），体现深度学习抽特征的能力，信息量很大，但也没有强调。图片说明：最后一层输出的 4098 维向量，相似向量对应的图片，也是相似的。换个角度理解，deep learning 是在做信息压缩，压成适合机器理解的向量特征。
3. paper 认为创新是 3 个 dirty trick，分别是：数据增强、ReLU、dropout。现在回看，都没那么重要，原理分析也不太对。
4. 整体评价，实验做得好，paper 写的很一般，更像是一个技术报告。实验里有非常多值得挖掘的信息，作者留机会给了同行。做好实验，也能成为奠基之作。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3vt29bqg5j215g0n448c.jpg)

## 八卦 from 李沐

source:

1. 二作 Ilya 到 Google 做报告，标题大概叫：3 个 dirty tick
	1. 用了 3 个 dirty trick：数据增强、ReLU、dropout
	2. 效果很好，赢了 imagenet
	3. 原理，基本没有讲。
2. 这篇 paper 也是类似的风格，没有解释新的认知、新的理解。导致，机器学习届，对这个工作不感冒，两三年内，关注 deep learning 的人不多。
3. 计算机视觉的人，喜欢 follow。因为，他们喜欢刷榜，这个工作对刷榜很有用。
4. 2012 年的时候，GPU 在机器学习届，用的比较多了。CUDA 是 2007 年出的。作者用 GPU，不是很创新的工作。但，用 2 个 GPU 做，的确是比较复杂。

## 李沐 3 遍阅读法

[how-to-read-ai-paper](../05-Notes%20Block/how-to-read-ai-paper.md)

读摘要，信息量：

1. 比 imagenet 第 2 名好非常多，很牛逼。achieved a winning top-5 test error rate of 15.3%, compared to 26.2% achieved by the second-best entry.
2. 摘要质量不高，更像是 tech report，而不是 paper。（整篇文章也如此

读 conclusion。这篇 paper 没写，但有 discussion。

1. 强调了 depth 很重要，这个是正确的。但是，为什么 depth 很重要，文章给的理由很牵强。
2. 这篇文章之前，deep learning 基本都是做无监督，因为，有监督的数据集上，打不过传统机器学习。这篇文章没有用 unlabeled data。证明了，deep learning 在监督学习中可以比传统机器学习好，直接影响了未来几年的 deep learning 研究方向。（bert 和 GAN 模型出来后，无监督的方向才重新得到了重视。
3. 如果有钱 & 机器，作者想去做 video。实际上，直到现在，业界在 video 上做的也不好，太吃算力。另外，video 很多都是有版权的。

## Introduction

更大的 dataset，很重要。

在简单的图片上，几万张图片的 dataset 一般够用。比如 cifar10，6 万张图。

## Dataset

ImageNet 数据集介绍，& 作者如何用于 deep learning。

ImageNet 共 15M 图片，22k 分类。

比赛，选了其中的越 1.2M 图片，1000 个分类，每个分类约 1000 图。50k validation 数据，150k test 数据。

2010 公布 test，之后就不公布 test 集合了。

数据处理：

1. 网络要求的输入是 256 * 256
2. 先把最短边 resize 到 256
3. 从中间 crop 256 * 256 的图片出来
4. 每个 pixel 都减去 mean activity（可以忽略），没有其他 trick

这里存在几个点：
1. 训练是在 raw RBG value 上做的，也就是 end to end。当时，大部分机器视觉，都是抽了特征在处理。比如 SIFT 特征。在 ImageNet 数据集上，也提供了一个版本的 SIFT 特征。
2. 如何 crop 256 * 256，其实是很重要的。也存在一些 trick。

## My Reference

- [(bilibili video) 李沐-AlexNet paper reading](https://www.bilibili.com/video/BV1ih411J7Kz/?spm_id_from=333.788&vd_source=1697bbf64aa697e049f71ddb4140612c)

## References

- ImageNet Classification with Deep Convolutional Neural
- Convolutional Deep Belief Networks on CIFAR-10
- High-Performance Neural Networks for Visual Object Classification
- [Multi-column deep neural networks for image classification](./multi-column-deep-neural-networks-for-image-classification.md)
- [Learning Multiple Layers of Features from Tiny Images](./learning-multiple-layers-of-features-from-tiny-images.md)
- Multi-column deep neural network for traffic sign classification
- Convolutional networks and applications in vision
- Metric Learning for Large Scale Image Classification - Generalizing to New Classes at Near-Zero Cost
- Best practices for convolutional neural networks applied to visual document analysis
- Learning methods for generic object recognition with invariance to pose and lighting
- Convolutional deep belief networks for scalable unsupervised learning of hierarchical representations
- What is the best multi-stage architecture for object recognition?
- [Improving neural networks by preventing co-adaptation of feature detectors](./improving-neural-networks-by-preventing-co-adaptation-of-feature-detectors.md)
- Caltech-256 Object Category Dataset
- Using very deep autoencoders for content-based image retrieval
- A High-Throughput Screening Approach to Discovering Good Forms of Biologically Inspired Visual Representation
- [ImageNet - A large-scale hierarchical image database](./imagenet-a-large-scale-hierarchical-image-database.md)
- High-dimensional signature compression for large-scale image classification
- Learning Generative Visual Models from Few Training Examples - An Incremental Bayesian Approach Tested on 101 Object Categories
- Convolutional Networks Can Learn to Generate Affinity Graphs for Image Segmentation
- Handwritten Digit Recognition with a Back-Propagation Network
- [LabelMe - A Database and Web-Based Tool for Image Annotation](./labelme-a-database-and-web-based-tool-for-image-annotation.md)
- Why is Real-World Visual Object Recognition Hard?
- [Neocognitron - A self-organizing neural network model for a mechanism of pattern recognition unaffected by shift in position](./neocognitron-a-self-organizing-neural-network-model-for-a-mechanism-of-pattern-recognition-unaffected-by-shift-in-position.md)
- [Rectified Linear Units Improve Restricted Boltzmann Machines](./rectified-linear-units-improve-restricted-boltzmann-machines.md)
- Shape-based object recognition in videos using 3D synthetic object models
- [Random Forests](./random-forests.md)
- Artificial neural networks applied to cancer detection in a breast screening programme
- Learning internal representations by error propagation
- Taylor expansion of the accumulated rounding error
- Lessons from the Netflix prize challenge
- Une procedure d'apprentissage pour reseau a seuil asymmetrique (A learning scheme for asymmetric threshold networks)
- Beyond Regression - New Tools for Prediction and Analysis in the Behavioral Sciences
