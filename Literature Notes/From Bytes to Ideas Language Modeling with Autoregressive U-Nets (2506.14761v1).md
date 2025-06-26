---
paper id: 2506.14761v1
title: "From Bytes to Ideas: Language Modeling with Autoregressive U-Nets"
authors: Mathurin Videau, Badr Youbi Idrissi, Alessandro Leite, Marc Schoenauer, Olivier Teytaud, David Lopez-Paz
publication date: 2025-06-17T17:55:11Z
abstract: "Tokenization imposes a fixed granularity on the input text, freezing how a language model operates on data and how far in the future it predicts. Byte Pair Encoding (BPE) and similar schemes split text once, build a static vocabulary, and leave the model stuck with that choice. We relax this rigidity by introducing an autoregressive U-Net that learns to embed its own tokens as it trains. The network reads raw bytes, pools them into words, then pairs of words, then up to 4 words, giving it a multi-scale view of the sequence. At deeper stages, the model must predict further into the future -- anticipating the next few words rather than the next byte -- so deeper stages focus on broader semantic patterns while earlier stages handle fine details. When carefully tuning and controlling pretraining compute, shallow hierarchies tie strong BPE baselines, and deeper hierarchies have a promising trend. Because tokenization now lives inside the model, the same system can handle character-level tasks and carry knowledge across low-resource languages."
comments: ""
pdf: "[[Assets/From Bytes to Ideas Language Modeling with Autoregressive U-Nets (2506.14761v1).pdf]]"
url: https://arxiv.org/abs/2506.14761v1
tags: []
---
