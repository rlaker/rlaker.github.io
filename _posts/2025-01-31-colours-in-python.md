---
title: "Finding better colour schemes in Python"
tags: [python, visualisation]
---


This [article](https://blog.datawrapper.de/create-good-color-palettes/), about how to find better colour schemes in Python, led me to [pypalettes](https://github.com/JosephBARBIERDARNAL/pypalettes?tab=readme-ov-file) - an easy way to get beautiful colour palettes into Matplotlib. Their colour palette [finder](https://python-graph-gallery.com/color-palette-finder/) lets you see a variety of graph styles and what each would look like to someone colour blind.

For an example, I really like the **Bay** colourmap, it reminds me of the [gruvbox](https://github.com/morhetz/gruvbox) theme.

![](/images/bay.png)

But what if I want a continuous colourmap? We can just add `type=continuous`!

```python
from pypalettes import load_cmap

load_cmap("Bay", cmap_type="continuous")
```

![](/images/bay_cont.png)

pypalettes allows you to easily define your own colour palettes and registers them with Matplotlib, fixing a problem that has always annoyed me!
