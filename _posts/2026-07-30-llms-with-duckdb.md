---
title: "Using duckdb with LLMs"
layout: single
excerpt: ""
tags: [code,data,til]
---

Part of my job is to explain why a large optimisation model made the decisions it did. To help speed up my own investigations into bugs, I built a shiny web app to translate the dense `.json` files the solver produces into a curated display of the objectives and the resources being allocated. As the tool developed, more and more technical plots/summaries were added that only really made sense to the team who built the model.

So, as with any internal business tool these days, we are now trying to implement an AI assistant. While I still believe in using visualisations over a generic chatbot, I do recognise that non-technical users would rather talk to an assistant than decipher a heatmap.

For the first prototype, presented during an internal hackathon, I focussed more on the vision than a full architecture design. The audience saw the assistant correctly identify why an example run failed, and how it could even edit the objectives to get a successful result out of the solver. What I didn't tell them was that I had basically just converted the data for a handful of resources into a string and sent that directly to the LLM. This was a quick, dirty solution suitable for a 2 day hackathon, but would be hopeless when scaled up to the tens of thousands of rows in a realistic problem.

The question of how to handle such large volumes of data, without sending (and paying for) millions of tokens to an LLM, bugged me for a while. But, as with all great ideas the answer looks obvious in hindsight, and of course someone smarter than me had already figured it out. [querychat](https://posit-dev.github.io/querychat/r/#how-it-works) is a Python (and R) package that allows you to "talk" with a dataset and integrates seamlessly into Shiny. Unfortunately, we wanted to use a custom [langgraph](https://www.langchain.com/langgraph) agent rather than the default `QueryChat` class in the package, but we have effectively implemented the same underlying idea.

Instead of sending our data to the LLM, we tell the LLM the schema of our locally stored data and then let the LLM write SQL to query the dataset. Internally, [duckdb](https://duckdb.org/docs/current/) is used to execute the SQL directly on Python objects! Since I had already transformed the `.json` files into structured pandas dataframes, duckdb can run any SQL query on them, without creating another copy

```python
import duckdb
import pandas as pd

pandas_df = pd.DataFrame({"a": [42]})
duckdb.sql("SELECT * FROM pandas_df")
```

Duckdb itself is directly importable into Python as a package with no dependencies, and works straight out of the box. I'm not the only one to be wowed by its ability to query data from any source ([1](https://www.jasonthorsness.com/25), [2](https://peterdohertys.website/blog-posts/dab-of-duck.html), [3](https://www.robinlinacre.com/recommend_duckdb)).

Once data sources are registered in duckdb, the LLM now has the ability to summarise the data in any way it thinks necessary, even making joins between the sources! All this flexibility and speed, without the data ever leaving our environment.
