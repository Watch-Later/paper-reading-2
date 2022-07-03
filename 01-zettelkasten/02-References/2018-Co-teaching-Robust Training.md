---
title: Co-teaching-Robust Training(2018)
pdf_relpath: pdfs/2018-Co-teaching-Robust%20Training.pdf
status: todo
tags:
- noisy-label
- paper
---

[local pdf](../../../pdfs/2018-Co-teaching-Robust%20Training.pdf)

# Co-teaching-Robust Training(2018)

recent studies on the memorization effects of deep neural networks show that they would first memorize training data of clean labels and then those of noisy labels.

理论前提：训练时，先学到 clean label，然后学 noisy label。所以，训练的早期，low loss 的数据，大概率是 clean label

同一个 model 的不同初始化版本，因为随机数的存在，在早期是可以提供不同的 view 的，所以，相互 sync 训练结果，有意义。
