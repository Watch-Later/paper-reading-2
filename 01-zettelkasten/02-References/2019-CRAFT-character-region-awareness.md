---
title: Craft Character Region Awareness(2019)
highlight: CVPR2019
pdf_relpath: pdfs/2019-CRAFT-character-region-awareness.pdf
status: reading
tags:
- ocr
- paper
url: https://arxiv.org/pdf/1904.01941.pdf
year: 2019
---

[local pdf](../../../pdfs/2019-CRAFT-character-region-awareness.pdf)

# CFAFT - Character Region Awareness

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h323a9sek9j20ov05b3zi.jpg)

## 论文信息

https://arxiv.org/pdf/1904.01941.pdf

- https://github.com/clovaai/CRAFT-pytorch Official implementation
- https://github.com/backtime92/CRAFT-Reimplementation

第一作者 Yongmin Baek 来自韩国

全部作者来自 Clova AI Research, NAVER Corp，隶属于 Line 公司的 AI 部门

## Pro & Con

主要思想：从单词级标注到字符级标注，解决弯曲变形等问题。提出两个标签用于训练（字符内，字符间）region score 检测单个字母 affinity score 组合不同字母成单词

之前方法的缺陷：单词级别的bounding box 受限于检测任意形状的单词

解决方案：字符级的检测，为解决标注困难，使用合成图像的 ground truth, 并用过渡模型估计真实图片的字符级标注（弱监督，没有真实场景的 ground truth），提出亲缘关系表示

优势：普通场景最好，任意方向的、弯曲的或变形的文本时，保证了较高的灵活性。

对于较小的文字（I，这种一个字母的单词），效果较为不好

## 问题定义

对自然场景进行文字检测，可以处理水平文本行、倾斜的文本行、弯曲的文本行

- 输入：带有文字的自然场景图片
- 输出：文本框或类文本框

## 相关工作

作者将现有的文字检测任务区分为：

1.Regression-based text detectors

主要思想：使用之前的目标检测算法并进行一些调整，采用盒回归的方法。

代表性论文：

TextBoxes：修改卷积核和anchor boxes[18]

DMPNet：四边形滑动窗口[22]

Rotation-Sensitive Regression Detector(RSDD)：主动旋转卷积核，利用旋转不变性[19]

2.Segmentation-based text detectors

主要思想：使用分割的方法进行检测，寻找像素级别的文本区域。

代表性论文：

Multi-scale FCN [7], Holistic-prediction [37],PixelLink [4]：分割

SSTD [8]：使用注意力机制，综合分割和回归，减少特征层面的背景干扰增强文本区域

TextSnake[24]：预测文本区域和几何属性的中心线。

3.End-to-end text detectors

主要思想：把检测和识别串联在一起进行，使用识别结果提高检测精度。

代表性论文：

FOTS [21] and EAA [10]：串联流行的检测识别方法，端到端训练。

TextSpotter [25]：将识别模块看作语义分割问题。（不理解）

缺陷：单词为单位，但是单词可以根据不同标准（意义，空格，颜色）分割。分词边界的界定。

4.Character-level text detectors

主要思想：字符级的文本检测器。

代表性论文：

Zhang et al. [39]：使用MSER【27】的文本块候选集得到检测器。

缺陷:低对比度，曲率和光线反射的影响，鲁棒性不好

Yao et al. [37]：字母预测图，文本单词区域图，需要单词级注释的连接方向

Seglink [32]：搜索文本网格，与附加的链接预测关联

Mask TextSpotter [25]:预测字符级别概率图（文本识别而不是字符识别）

## 动机和思路

WordSup：弱监督的方式训练字符级检测器。

缺陷：使用矩形框标注，容易受摄像头视点变化引起字符的透视变形的影响，主干网络也会影响最终的性能（anchor的数量及大小）

## 方法

### 网络结构

backbone 是 VGG-16, 结构总体与U-Net一致

定义了两个得分；打破了矩形框标注的局限。

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h323t13bujj20cz0eagnf.jpg)

### Ground Truth 生成

使用合成图像进行弱监督训练，+ 真实图像微调

高斯分布标注，使用透视变换标注减少计算量（预设定一个各向同性高斯分布，通过头视变换到标注的bounding box）

affinity score的ground truth：相邻字符画对角线，对应四个三角形中心作为bounding box。

方法对于长文本友好，只关注字符内和字符间。

生成 Region score 和 Affinity score

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h323tv4i2qj20su0a7tbm.jpg)

字符分割的过程

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3243f6nm8j20t70ej0x4.jpg)

### Loss 函数

置信度。
对于一个单词级别的标注，其所在区域若记为R（w），字符个数若为l(w)，而裁切得到的字符总个数记为l^c(w)，定义置信度打分为
（如果预测的区域个数和原有字符个数相同，则打分为1）
![](https://tva1.sinaimg.cn/large/e6c9d24egy1h324bbc9l1j20d802o3yh.jpg)

pixel 级别置信度标注

逐一像素的打分规则

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h324bfy6jkj20ca02kq2v.jpg)

最终的 Loss 函数

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h324bkvqi0j20e101rt8p.jpg)

### 后处理

流程：使用神经网络生成mask，（设定两个得分的阈值）->连接组件标记->QuadBox获得

原理：根据区域打分Sr和联合打分Sa (region score & affinity score)来预测文本行

#### 矩形框

首先预测超参阈值来获得一个原图的mask，提取满足Sr大于阈值或Sa大于阈值的区域M(p)

而后利用CCL算法（opencv）获得连通区域，最后一步利用最小矩形框住每一个连通区域（opencv)

#### 任意形状框

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h324jcoi5gj20cz0bbwg3.jpg)

1. 指定一个从左到右的扫描方向
2. 根据扫描方向逐步得到每个文字的最大范围（图中蓝色箭头）
3. 蓝色箭头的中点连成线，获得局部连接（图中黄线）
4. 黄线每一个点的垂直方向，调整蓝色的箭头得到图中红色箭头，
5. 获得控制点（图中绿色点，收尾部分要外移）
6. 根据绿色点，得到多边形（图中紫色部分）

## 实验

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3248720ojj20so0gdjxr.jpg)

矩形框数据集上的测试结果

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3248ed6d4j20e407ign0.jpg)

多边形框数据集上的测试结果

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h3248zwfq9j20ec07dwfi.jpg)

## 实验效果图

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h329zx6i6uj20fv0a5dht.jpg)

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h329yryxpaj206m08c3z7.jpg)

## References

1.
