---
tags: [til, python, visualisation]
excerpt: "Using shinylive to run Python apps within the browser"
---

I wrote about the [Shiny for Python](https://shiny.posit.co/py) framework in another blog [post](/shiny-web-apps), however, I had not quite grasped the power of [shinylive](https://shiny.posit.co/py/docs/shinylive.html). This uses [pyodide](https://pyodide.org/en/stable/) to run within the browser, allowing interactive web apps to run on static generated sites such as Github Pages or Netlify.

For example, try adjusting the coefficients *below* to model weekly seasonality. For further explanation, see the main blog [post](/linear-models-demystified)
<iframe src="/files/shiny_linear_model/shiny_linear_model.html" width="100%" height="1000px" style="border:none;"></iframe>
*This is actually running Python in your browser! Inspect the console for this page and you will see that Python packages have been installed.*

As demonstrated by Shiny's website, Python packages could run interactive demos of each function in their documentation, without the user actually installing the package. Users could also see how they can modify the example by editing the documentation directly!

The only downside is that only a few [select packages](https://pyodide.org/en/stable/usage/packages-in-pyodide.html) have been converted to work with Pyodide, but the data science big hitters are there: numpy, matplotlib and pandas

# How to include Shiny apps with GitHub Pages

After writing your shiny app, export it with the [shinylive](https://shiny.posit.co/py/docs/shinylive.html) package 

```shell
shinylive export app_folder site
```

You can then serve the site locally with 

```shell
python -m http.server --directory site 8008
```

All we need to do now is upload this folder to our GitHub Pages site. I decided to put it in the files folder, since I do not need to show the `index.html` app file directly. Instead, I include the app in posts by using an `iframe`

```html
<iframe src="/files/shiny_linear_model/shiny_linear_model.html" width="100%" height="1000px" style="border:none;"></iframe>
```


