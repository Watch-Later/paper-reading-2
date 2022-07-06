---
Alias: bert
title: BERT - Pre-training of Deep Bidirectional Transformers for Language Understanding
authors:
- Jacob Devlin
- Ming-Wei Chang
- Kenton Lee
- Kristina Toutanova
fieldsOfStudy:
- Computer Science
meta_key: bert-pre-training-of-deep-bidirectional-transformers-for-language-understanding
numCitedBy: 33754
pdf_relpath: null
ref_count: 60
status: todo
tags:
- gen-from-ref
- paper
venue: NAACL
year: 2019
---

# BERT - Pre-training of Deep Bidirectional Transformers for Language Understanding

## 阅读价值

与 GPT 和 ELMo 相关，区别是

1. GPT 是单向的，BERT 是双向的
2. ELMo 用的是 RNN，bert 是 Transformer

## Introduction

把 pre-train 模型做特征表示的时候，有 2 类策略：

1. 基于特征的。feature-based，例如 ELMo
2. 基于微调的。fine-tuning，例如 GPT

feature-based，把预训练的特征表示，作为额外的特征输入到模型里。

fine-tuning 的方法，与下游 task 相关的参数很少，只要在下游的任务上把预训练的参数 fine-tune 一下即可。

2 个方法，在预训练时，使用相同的目标函数，也都是单向的语言模型。

当前的技术，有很强的局限性。尤其是 fine-tuning 的方法，例如：

1. 做 sentence-level 的 task 上，看双向的信息是合法的。
2. 在 token-level 的task 上，比如 question answering，特别需要 2 个方向的上下文 context。

如果使用双向的信息，应该可以提升上述任务的表现。

改进思路：用 MLM，masked language model。带掩码的语言模型。

启发来自 1953 年的 paper。之前没有人做过带掩码的语言模型吗？

MLM 说明：随机的把输入中的一些 token 给遮住（mask），目标是去预测被遮住的 token。训练时，允许看左右的双向信息。

在 masked language model 外，引入了另一个任务：next sentence prediction。判断 2 个句子在原文是相邻的，还是随机的 2 个句子。

3 点贡献：

1. 展示了双向信息的重要性。
2. 不用对特定任务做特定的模型改动
3. 在 11 NLP task 上 SOTA，代码和模型都开源

## Related Work

skip

## Model Architecture

bert 分 2 步：

1. pre-training: 在没标注的数据上训练。
2. find-tuning:
	1. 用 pre-trained 参数初始化模型，在 task 相关的标柱数据上，监督训练。
	2. 每个下游 task，都有独立的 fine-tuned model



## References

- [Attention is All you Need](./attention-is-all-you-need.md)
- Dissecting Contextual Word Embeddings - Architecture and Representation
- Semi-Supervised Sequence Modeling with Cross-View Training
- [Semi-supervised sequence tagging with bidirectional language models](./semi-supervised-sequence-tagging-with-bidirectional-language-models.md)
- [Character-Level Language Modeling with Deeper Self-Attention](./character-level-language-modeling-with-deeper-self-attention.md)
- [QANet - Combining Local Convolution with Global Self-Attention for Reading Comprehension](./qanet-combining-local-convolution-with-global-self-attention-for-reading-comprehension.md)
- [GLUE - A Multi-Task Benchmark and Analysis Platform for Natural Language Understanding](./glue-a-multi-task-benchmark-and-analysis-platform-for-natural-language-understanding.md)
- [Skip-Thought Vectors](./skip-thought-vectors.md)
- MaskGAN - Better Text Generation via Filling in the ______
- [Contextual String Embeddings for Sequence Labeling](./contextual-string-embeddings-for-sequence-labeling.md)
- Multi-Granularity Hierarchical Attention Fusion Networks for Reading Comprehension and Question Answering
- [Deep Contextualized Word Representations](./deep-contextualized-word-representations.md)
- U-Net - Machine Reading Comprehension with Unanswerable Questions
- [Bidirectional Attention Flow for Machine Comprehension](./bidirectional-attention-flow-for-machine-comprehension.md)
- [Recursive Deep Models for Semantic Compositionality Over a Sentiment Treebank](./recursive-deep-models-for-semantic-compositionality-over-a-sentiment-treebank.md)
- [A unified architecture for natural language processing - deep neural networks with multitask learning](./a-unified-architecture-for-natural-language-processing-deep-neural-networks-with-multitask-learning.md)
- [Universal Language Model Fine-tuning for Text Classification](./universal-language-model-fine-tuning-for-text-classification.md)
- [Google's Neural Machine Translation System - Bridging the Gap between Human and Machine Translation](./google-s-neural-machine-translation-system-bridging-the-gap-between-human-and-machine-translation.md)
- One billion word benchmark for measuring progress in statistical language modeling
- [A Decomposable Attention Model for Natural Language Inference](./a-decomposable-attention-model-for-natural-language-inference.md)
- [TriviaQA - A Large Scale Distantly Supervised Challenge Dataset for Reading Comprehension](./triviaqa-a-large-scale-distantly-supervised-challenge-dataset-for-reading-comprehension.md)
- [Learned in Translation - Contextualized Word Vectors](./learned-in-translation-contextualized-word-vectors.md)
- [Supervised Learning of Universal Sentence Representations from Natural Language Inference Data](./supervised-learning-of-universal-sentence-representations-from-natural-language-inference-data.md)
- [SemEval-2017 Task 1 - Semantic Textual Similarity Multilingual and Crosslingual Focused Evaluation](./semeval-2017-task-1-semantic-textual-similarity-multilingual-and-crosslingual-focused-evaluation.md)
- [A large annotated corpus for learning natural language inference](./a-large-annotated-corpus-for-learning-natural-language-inference.md)
- Semi-supervised Sequence Learning
- context2vec - Learning Generic Context Embedding with Bidirectional LSTM
- SWAG - A Large-Scale Adversarial Dataset for Grounded Commonsense Inference
- [SQuAD - 100,000+ Questions for Machine Comprehension of Text](./squad-100-000-questions-for-machine-comprehension-of-text.md)
- [A Broad-Coverage Challenge Corpus for Sentence Understanding through Inference](./a-broad-coverage-challenge-corpus-for-sentence-understanding-through-inference.md)
- [Simple and Effective Multi-Paragraph Reading Comprehension](./simple-and-effective-multi-paragraph-reading-comprehension.md)
- Word Representations - A Simple and General Method for Semi-Supervised Learning
- [Learning Distributed Representations of Sentences from Unlabelled Data](./learning-distributed-representations-of-sentences-from-unlabelled-data.md)
- [An efficient framework for learning sentence representations](./an-efficient-framework-for-learning-sentence-representations.md)
- Domain Adaptation with Structural Correspondence Learning
- A Scalable Hierarchical Distributed Language Model
- Reinforced Mnemonic Reader for Machine Reading Comprehension
- [Distributed Representations of Sentences and Documents](./distributed-representations-of-sentences-and-documents.md)
- [GloVe - Global Vectors for Word Representation](./glove-global-vectors-for-word-representation.md)
- [How transferable are features in deep neural networks?](./how-transferable-are-features-in-deep-neural-networks.md)
- The Sixth PASCAL Recognizing Textual Entailment Challenge
- A Framework for Learning Predictive Structures from Multiple Tasks and Unlabeled Data
- [Neural Network Acceptability Judgments](./neural-network-acceptability-judgments.md)
- [Aligning Books and Movies - Towards Story-Like Visual Explanations by Watching Movies and Reading Books](./aligning-books-and-movies-towards-story-like-visual-explanations-by-watching-movies-and-reading-books.md)
- [Distributed Representations of Words and Phrases and their Compositionality](./distributed-representations-of-words-and-phrases-and-their-compositionality.md)
- [Extracting and composing robust features with denoising autoencoders](./extracting-and-composing-robust-features-with-denoising-autoencoders.md)
- The Seventh PASCAL Recognizing Textual Entailment Challenge
- Discourse-Based Objectives for Fast Unsupervised Sentence Representation Learning
- Automatically Constructing a Corpus of Sentential Paraphrases
- The PASCAL Recognising Textual Entailment Challenge
- The Winograd Schema Challenge
- [ImageNet - A large-scale hierarchical image database](./imagenet-a-large-scale-hierarchical-image-database.md)
- Class-Based n-gram Models of Natural Language
- [Bridging Nonlinearities and Stochastic Regularizers with Gaussian Error Linear Units](./bridging-nonlinearities-and-stochastic-regularizers-with-gaussian-error-linear-units.md)
- Introduction to the CoNLL-2003 Shared Task - Language-Independent Named Entity Recognition
- “Cloze Procedure” - A New Tool for Measuring Readability
