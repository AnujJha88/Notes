---
paper id: 1706.03762v7
title: Attention Is All You Need
authors: Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, Illia Polosukhin
publication date: 2017-06-12T17:57:34Z
abstract: The dominant sequence transduction models are based on complex recurrent or convolutional neural networks in an encoder-decoder configuration. The best performing models also connect the encoder and decoder through an attention mechanism. We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely. Experiments on two machine translation tasks show these models to be superior in quality while being more parallelizable and requiring significantly less time to train. Our model achieves 28.4 BLEU on the WMT 2014 English-to-German translation task, improving over the existing best results, including ensembles by over 2 BLEU. On the WMT 2014 English-to-French translation task, our model establishes a new single-model state-of-the-art BLEU score of 41.8 after training for 3.5 days on eight GPUs, a small fraction of the training costs of the best models from the literature. We show that the Transformer generalizes well to other tasks by applying it successfully to English constituency parsing both with large and limited training data.
comments: 15 pages, 5 figures
pdf: "[[Assets/Attention Is All You Need (1706.03762v7).pdf]]"
url: https://arxiv.org/abs/1706.03762v7
tags:
---
## Notes
RNNs do a sequential processing of the data and produce a sequence of hidden states as a function of the previous hidden state i.e. $h_{t+1}=f(h_{t})$ and so what this does is it makes parallelization difficult, but for larger training corpuses we need to figure out a way to parallelize this computation as there are memory constraints that limit batching across examples, so you cannot use too many examples per batch.


There has been some results using factorization tricks and conditional computations which also improve the model performance. But the fundamental constraint of sequential computation i.e. this hidden state computation is still there.

Then comes in attention mechanism and it has been an integral part of sequence modeling tasks or seq2seq modeling which allows us to model dependencies between elements without having to worry too much about the distance between them.  These however are still used with RNNs.

Transformers dump recurrence and rely solely on attention to draw out dependencies between the input and output.

So there's self attention in the input and output both and then there is cross attention between the input and output..

About 12 hours of training on 8 P100s could get the model up to SoTA.

