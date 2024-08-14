---
title: "Doing algebra with the meaning of words"
layout: single
excerpt: "how word embeddings can tell us about the relationship between words"
tags: [til, statistics]
---

Learning lots of crazy stuff about embeddings from this great [guide](https://lena-voita.github.io/nlp_course/word_embeddings.html). The basic idea is to represent every word in a language by a vector which encodes its **semantic meaning**. You can then find similar words/topics by looking for vectors that are close to each other.

It turns out that when you do these kind of word embeddings on large amounts of text there are many linear relationships between words. For example, the vector distance between **king** and **queen** is about the same as the distance between **man** and **woman**.

This means you can perform a kind of addition and subtraction about the meaning of words!

```
bar - alcohol + coffee = cafe
musician - music + science = researcher
```

This idea is similar to [how emojis are encoded](https://developers.mattermost.com/blog/all-about-emojis/), with the astronaut (👨‍🚀) emoji literally being represented as 👨/👩 + 🚀. This also used to encode all the different family combinations, e.g. 👨 + 👨 + 👧 = 👨‍👨‍👧

# Learning translations

The added bonus of this fact you can transfer what you learnt about one language to another, just need a small dictionary connecting the two languages and you can learn other translations for free!

![](https://lena-voita.github.io/resources/lectures/word_emb/analysis/cross_lingual_matching-min.png)
