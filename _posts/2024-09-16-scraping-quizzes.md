---
title: "Using weekly quizzes to keep LLMs informed"
layout: single
excerpt: "and ideas for pub quiz preparation"
tags: [til, ideas]
---

One does not simply have enough computing resources to keep retraining Large Language Models (LLMs) in order to keep them informed of ongoings in the human world. However, the context window for LLM queries continues to expand, allowing more and more relevant information to be inserted into the prompt. Defining "relevant" is surprisingly difficult in a world where [533 new Wikipedia articles](https://en.wikipedia.org/wiki/Wikipedia:Statistics) are created each day. Most commonly, a Retrieval Augmented Generation (RAG) system is implemented, which stores the semantic meaning of sentences (using word embeddings) in a database, and then inserts the top $N$ snippets with similar meaning into the prompt. 

The [RealTimeQA project](https://realtimeqa.github.io/) takes a different approach. By scraping the weekly news quizzes, they effectively ellicit the help of human editors to sift through the noise. Specifically, they extract 10 questions from each of "The Week", "USA Today" and "CNN" and use the Google search API to collect the top 10 articles related to each question as well as the top 5 most relevant Wikipedia pages. This allows them to build an API like this:


```json
{
    "question_id": "20240119_4", 
    "question_date": "2024/01/19", 
    "question_source": "THE WEEK", 
    "question_url": "https://theweek.com/puzzles/quiz-of-the-week-13-19-january", 
    "question_sentence": "NHS Scotland this week advised people to do what to avoid slipping on ice during the winter chill?", 
    "choices": ["Wear specialised snow boots", "Drive or stay at home", "Walk like a penguin", "Spray liquid de-icer before taking each step"], 
    "evidence": "As an amber alert for snow was put in place in northwest Scotland and the Northern Isles, NHS Scotland said that \"penguins know best\" and advised adopting a penguin-like, waddling gait to prevent slips in icy conditions <a href=\"https://theweek.com/digest/nhs-tells-scots-to-walk-like-penguins\">Link</a>"

}
```

This got me thinking about creating an automated system to prepare for the weekly pub quiz. Could you create a model to predict the likelihood a story would be asked about in the quiz? By telling the model which questions came up each week, you could learn which kind of newspapers the question setter reads. My gut feeling would be that only a few questions a week will not be able to teach the model much given the sheer scale are stories out there, but maybe we could see [ALBERT](https://llmmodels.org/tools/albert/) on The Chase.