---
title: "Publishing my Obsidian cooking vault as a static site"
layout: single
excerpt: "Another thing Claude has solved for me"
tags: [code]
---

I have an Obsidian vault to store everything I know about cooking: recipes, ingredients, cookbooks and restaurant notes. Each has a YAML frontmatter and `[[wikilinks]]` that allow me to query the notes like a database. I publish a public version of these notes at [ronanlaker.com/cooking](https://www.ronanlaker.com/cooking), but the result was always buggy.

The old workflow required several convoluted steps:
1. A Python script to move only the relevant notes to a new folder and strip out some private content
2. Open the new folder with Obsidian and use the [Webpage HTML Export](https://community.obsidian.md/plugins/webpage-html-export) plugin to export HTML files to yet another folder
3. Push the final folder to Github as a repo named "cooking", so that it will appear at the right url when published with GitHub pages

The main problem is buggy rendering of [dataview](https://community.obsidian.md/plugins/dataview) plugin, which underpins the functionality of the vault. This workflow also needed a GUI to complete and was extremely slow for my 1000+ pages.

## The new architecture

I knew the exported website wasn't up to scratch for a while but had no idea how to get around the dataview issue, apart from waiting for the plugin authors to update their code. I laid out this predicament for Claude who then diligently prepared a new pipeline based on Python and Quartz v5.

Claude was able to write a full Python interpreter for the dataview queries, like:

```
table length, healthy, course
from "100 Recipes"
where icontains(cuisine, "french")
sort course,length asc
```

It's not the most complex query, but I was very pleasantly surprised that this worked straight away. I think the biggest improvement in AI is the fact that it writes tests at each stage of the development. After the Python code has converted the dataview queries to Markdown lists and tables, Quartz v5 then builds a fully functional static website with backlinks, interactive graph etc all thanks to greater [Obsidan compatability](https://quartz.jzhao.xyz/getting-started/whats-new#improved-obsidian-compatibility).

The final result is that I can now just run `make deploy` to build the website from scratch and push to GitHub.