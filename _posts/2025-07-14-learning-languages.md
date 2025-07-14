---
title: "Learning languages in 2025"
layout: single
excerpt: ""
tags: [code]
---

Despite being the proud owner of an A* in GCSE French, *je parle pas un mot*. Since I have joined a predominately French company I've decided to try learning it again, even if I just to understand the back of a wine bottle.

This aim has coincided with a requirement at work to 10x the speed of our Python API, which will probably require rewriting parts in another programming language.

Given its been >10 years since I attempted to learn any language, programming or natural, are there better ways to teach myself in 2025?

# French

## Duolingo

Duolingo isn't the greatest app in the world, but my god is it good at peer pressure. Having to do at least *some* French every day is a good way to keep up my motivation and build vocabulary. 

Luckily for me, [30% of English vocabulary is based on French](https://youtube.com/watch?v=TUL29y0vJ8Q&si=EXkpFPl6jOWpNNbz) so I can bluff my way past the easy levels. However, this app isn't very helpful for learning about grammar rules or how French people actually speak.

## Youtube

For that, Youtube is an absolute gold mine. I can watch:
- [grammar explanations](https://youtu.be/SApp5pEtCB4?si=OPeOpvtQjZT2-SZF)
- [people being interviewed in the street](https://youtu.be/YCuU4SjcS2A?si=QDmyp-w6hplCKcmI)
- [Jokes about the language](https://youtu.be/q3gxfqXTj_E?si=eha6TPlH925axXpX)

Youtube's auto-translation is so good that it opens up the chance to watch *unbelievably* French videos like:
- [A man in a beret who kills his rooster to make coq au vin](https://youtu.be/DW5DXfCF3hE?si=w4mHtGtyOMzoiuGE)
- [A different man in a beret showing me how Mont D'or is made](https://youtu.be/hnlijylU8Q4?si=8N-qzYtVEHK0pDx-)
- [a dude wearing a questionable number of knives](https://www.youtube.com/watch?v=urIwhx3uHug)

# Rust

I've been using Python for 10+ years and have learnt a lot of general programming concepts through it, e.g. Object oriented, iterators, threading. However, I think I need to learn a low-level language that I can turn to when I need performance.

For example, I was working on some code that merged two dataframes and then exploded the columns to something like 5 millions rows. When written in pure Python it would take 4.4 seconds compared to a ridiculous 0.15s in [Rust](https://www.rust-lang.org/). It has become a bit of a meme to "re-write this in Rust", so it was definitely on my radar but I couldn't tell why it was so different. 

## The Rust Book

[The rust book](https://rust-book.cs.brown.edu/ch00-00-introduction.html) is an incredible resource. Not only does it show you the syntax, but it has diagrams like this to explain concepts like [ownership and borrowing](https://rust-book.cs.brown.edu/ch04-02-references-and-borrowing.html)


![](/images/the_rust_book.png)

So, I'm not only learning about one new language but I'm being guided through concepts like heap and stack memory, pointers and everything else Python abstracts away. 

I will probably still use Python for 90% of what I do, but its nice to be able to see how another language handles common pain points in Python like error messages and packaging.

## Rustlings

[Rustlings](https://rustlings.rust-lang.org/) is the perfect partner to the Rust book, as it gives you 94 example scripts with compiler errors that you need to fix. This also demonstrates just how incredible the compiler is at helping you fix mistakes.

## Youtube

There is a lot of nonsense about "which programming language is the best", but I found this gem of a channel that explains why Rust is great

![](https://youtu.be/2hXNd6x9sZs?si=qhvfe5Pw_0HSSNkN)
