---
title: "Finding better colour schemes in Python"
tags: [python, visualisation]
---


This [article](https://blog.datawrapper.de/create-good-color-palettes/), about how to find better colour schemes in Python, led me to [pypalettes](https://github.com/JosephBARBIERDARNAL/pypalettes?tab=readme-ov-file) - an easy way to get beautiful colour palettes into Matplotlib. Their colour palette [finder](https://python-graph-gallery.com/color-palette-finder/) lets you see a variety of graph styles and what each would look like to someone colour blind.

For an example, I really like the **Bay** colourmap, it reminds me of the [gruvbox](https://github.com/morhetz/gruvbox) theme.

![](/images/bay.png)

But what if I want a continuous colourmap? Well, pypalettes allows you to define your own colour palettes and registers them with Matplotlib. Therefore, we can just take the colours of the previous cmap and say I want the `type=continuous`

```python
from pypalettes import load_cmap, add_cmap

add_cmap(
	colors=load_cmap("Bay").colors,
	name="Bay_cont",
	cmap_type="continuous"
)
```

![](/images/bay_cont.png)
