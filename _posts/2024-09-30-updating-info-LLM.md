---
title: "How would you bring an LLM up to date?"
layout: single
excerpt: "and steer its persona towards certain opinions"
tags: []
---

> The following essay was based on an interview question. I had fun doing this mini literature review of LLMs and how to keep their knowledge updated, but it was a bit of a shame to get the "pretend we haven't read this" treatment

# If Your Memory Has the Same Cutoff as GPT4o, what information would you include in the 10k tokens to bring you up to date?

Since it is a gargantuan effort to train an LLM and its billions of parameters, it is not feasible to continuously train models on new information. As demonstrated by [Cheng 2024](https://arxiv.org/html/2403.12958v1), even if a model is advertised with a known "knowledge cut-off date" the information encoded into the LLM can still be stale. By analysing the open source data LLMs were trained on (e.g. [CommonCrawl](https://commoncrawl.org/)), the authors found that duplicate articles which were "semantically equivalent but lexically near duplicates". This, in combination with outdated Wikipedia articles in the training data, meant that the "effective knowledge cut-off" was long before the advertised value. For an example of how ingrained this problem is, the authors state that "although the direct Wikipedia dump is included in the pre-training data, over 80% of the Wikipedia documents are from earlier versions (pre-2023)". 

So, how do we update the LLM to bring it up to date with the new knowledge and events in the world? 

## RAG

There is too much information being generated in the world to be continuously added to the 10k tokens (roughly 1 token for every 4 characters). Therefore, the question now becomes "what is the most relevant information?" that the LLM should be aware of. This can change given the question/task being asked by the LLM, which is the driving concept behind retrieval augmented generation ([RAG](https://en.wikipedia.org/wiki/Retrieval-augmented_generation)). One such variant of this technique works by preloading the semantic meaning of text as vectors in a database along with the whole "document". When a question is asked, the semantic meaning of the question is compared with the database and the top-k documents are included in the prompt to aid the LLM's text generation. For example, I have experimented with using such a system within my notetaking app [Obsidian](https://obsidian.md/). By allowing a plugin, such as the [AI-assistant](https://github.com/qgrail/obsidian-ai-assistant), to perform RAG on my 1000s of personal notes and effectively be my "second brain". This plugin simply makes it easier to put excerpts of your notes into an LLM, but I think it could be extended to use the inbuilt link features of Obsidian (similar to Wiki-links). The links between notes, which forms a graph, could allow the RAG system to not only find relevant excerpts, but be able to connect ideas together e.g. take top $k$ similar documents from the semantic meaning and also include $n$ connected notes.

While this vector database method can work well for some smaller problems, it would be hard to encode all news articles/new websites since Dec 2023 in this system. Instead, we could elicit the help of human editors to sift through the noise and return only the most relevant news/articles over time. This is the idea behind [realtimeQA](https://realtimeqa.github.io/), and the associated [paper](https://arxiv.org/abs/2207.13332). They have built a system that extracts multiple choice questions from 3 news sites, CNN, USA Today and The Week. An average of 10 questions a week per website gives them a corpus of 120 questions a month to keep track of the most important news, which by their inclusion in the quiz have been deemed as noteworthy by a human editor. 

> I wrote about this in more detail [here](www.ronanlaker.com/scraping-quizzes)

Their method attempts to retrieve documents from an external knowledge source in order to help it synthesis an answer. In their case, they use the top-5 Wikipedia articles from a Dense Passage Retrieval (DPR, [Karpukhin 2020](https://arxiv.org/abs/2004.04906)) and the top-5 news articles from the Google search API. This DPR process is very similar to the semantic vector database mentioned above, with the extra step of training on an open QA dataset. After extracting the most relevant articles, the LLM is prompted with the titles and first two paragraphs of those articles along with the publication date. The authors reported significant performance improvements vs the public GPT-3 model, but they also noted that the LLM would still hallucinate and invent an answer rather than telling the user it did not have enough evidence.

I think this is an interesting idea which could be easily extended to include more quiz sources, as long as they have a consistent multiple choice answer format. However, there will be an issue over time as the number of questions and evidence approaches the 10k token limit. At that point, how do I decide which are the important events?

## Timeline Summarization

We are now describing the task of  "timeline summarization" which is detailed extensively by [Ghalandari 2020](https://arxiv.org/abs/2005.10107) . By looking at news articles since Dec 2023 can we reconstruct a timeline of important events and provide snippets for the model to learn from.

The authors identify three high-level approaches to this task:
1. Direct summarization - select small subset of sentences to represent the whole timeline
2. Date wise - first selects important dates by looking at dates mentioned in articles, or the number of articles published per day, and then summarises each one
3. Event detection - detect events, rather than dates, and summarizes separately

I will focus on the latter, since it represents the state-of-the-art. Events are first identified by finding clusters within a graph of news articles. This graph is constructed by assigning an edge between two articles if they are published within 1 day of each other. The edge weight is set as the "cosine similarity between TF-IDF bag-of-words" for each article - which encodes how similar the themes of each article are. After using [Markov clustering](https://github.com/GuyAllard/markov_clustering), they can build a timeline by selecting the top $l$ most important events and using $k$ sentences to summarise them. This is especially useful for our problem, since we can balance the number of events against the detail given for each, so that we can maximise the information for our 10k token budget. 

Since we are defining the top $l$ most important events, we can also include some tailoring to the type of task being asked of the LLM. This would depend on the specifics of the problem/client but we could limit articles to be about AI if we want the model to know how this technology has developed since Dec 2023.

# What about a Persona, how does the information change?

If we change the objective and instead use the 10k tokens to give the LLM a persona, how does this information change? 

Much like how knowledge is encoded in an LLM model, [Santurkar 2023](https://arxiv.org/abs/2303.17548) has found that LLM's have an inherent bias and persona that can influence their answers/opinions. For example, they found that the models generally reflected the opinions of "people who are liberal, high income, well-educated, and not religious". This, and several other papers, then attempt to steer the personas of LLMs in order to make them less biased. However, we want to do the opposite, and try to steer the model to have the opinions of our target persona. 

Following [Hwang 2023](https://arxiv.org/abs/2305.14929), this can be achieved by prompting the model about:
1. Demographics - e.g. age, gender, 
2. Ideology - Political leanings (liberal, conservative) 
3. Opinions - "this persona thinks guns are dangerous"

The authors found that a combination of all these pieces of information led to the best results, specifically "incorporating the user’s previous opinions, up to 16 in total, along with demographic information, substantially enhances the performance in both overall and collapsed accuracy". However, they also noticed that "top-3 opinions yields similar performance the top-8 opinions" which demonstrates that we would not get much benefit from using all 10k tokens on past opinions (although this is something I would test myself). 

To demonstrate such an approach, I have written a 1.7k prompt for my own persona when applying for jobs. I have included:
- my demographic information (age, location, job title)
- CV information
- 8 example questions and answers from past interviews

Even in this simple demonstration, I was able to get answers I agreed with on topics not included in the prompt examples. The model still hallucinated when not provided enough information to answer a question which is an issue to be fixed (not easily I think). While this is just a simple example I can already imagine recruiters putting people's "work persona" in their companies environment to see how they would fit in with the culture.

In more general cases, I would use a targeted survey to gather information about the target persona. This survey would be designed to explore as much of the "opinion space" as possible so that a wide range of topics can be asked of the LLM. If that is not possible then I would try and use the same summarisation techniques from above, but limit my news articles to be from certain publications most likely read by that persona. i.e. can I replicate the opinions of a Guardian or Times reader? I could also data-mine things like podcast transcripts to find out the opinions/topics mentioned in each show. This could be useful information for advertisers, as they could see how well their message would work with each individual podcast listenership.

One potential problem with this approach would be the fact that humans are extremely good at filtering out information that doesn't match our world view. According to the model, all that information we gave it is as relevant, but our persona may potentially ignore most of it. I also wonder if it would be possible for an LLM to hold conflicting beliefs, like humans do, or would it try and be too logical? Given that we don't really know how knowledge is stored in LLMs ([3Blue1Brown video](https://youtu.be/9-Jl0dxWQs8?si=6jlUpCt-YTS_0tLg)) I would say that an LLM probably could hold conflicting beliefs, which is further supported by [Wang 2023](https://aclanthology.org/2023.findings-emnlp.795/) who demonstrated that LLMs struggle to defend their beliefs.

## Virtual Tokens

While we can steer the LLMs towards certain demographics using the techniques above, this does not take into account that members of the same demographic can have vastly different opinions. [Li 2024](https://arxiv.org/abs/2311.04978) offers another way to steer LLMs towards a persona, which they derive without the use of demographic information. Instead, they use survey responses to map out the "opinion space" and then perform collaborative filtering to embed users into the opinion space. Given these embeddings they can either steer the LLM towards a single persona, or a cluster of opinion which emerges naturally rather than being manually set as "Republican" or "Democrat" in the prompt. For example, with their unsupervised technique they were able to recover groups of opinions representing mostly white democrats with a postgraduate degree. Crucially, this cluster also contained a mix of demographics who also held the same opinions, which is not possible with the method of [Hwang 2023](https://arxiv.org/abs/2305.14929).

This collaborative filtering method allows a much more nuanced approach to opinion, but this also comes at the cost of using "virtual tokens" rather than the simple prompt discussed previously. These virtual tokens are learnt by using a separate "soft-prompting model" which attempts to minimise the loss between the questions and answers in the training dataset. This method of steering LLM persona was found to perform significantly (~60% success rate) better than the Context + Raw Q used in [Hwang 2023](https://arxiv.org/abs/2305.14929) (~35% success rate).

I like the flexibility, and efficacy, of this steering method but I would be very interested to see what the "virtual tokens" looked like. Do they look like real words to a human? or are they purely mathematical objects from the optimisation.