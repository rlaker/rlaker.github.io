---
tags: til
---

Today I learnt about [Shiny for Python](https://shiny.posit.co/py/), a way to make slick web apps in Python. I had a vague knowledge of [streamlit](https://streamlit.io/) as a way to make web apps in Python. The general idea is that you can write a script in Python, and then turn it into a web app at a later date but adding extra streamlit commands. However, as explained in this [talk](https://youtu.be/ijRBbtT2tgc?si=EuAyTOhItzt_0Hza), the entire (potentially costly) script needs to be re-run to refresh the web page.

Instead, Shiny (originally written for the R language) asks the user to design the web app from the start, rather than modifying an existing script. This is done by decorating functions the necessary functions, so that only they will be re-run. This type of [reactive programming](https://shiny.posit.co/py/docs/reactive-programming.html) opens up a range of technical possibilities. 

The Shiny gallery includes a [Wordle clone](https://shinylive.io/py/app/#wordle) and [ChatGPT playing 20 questions with itself](https://youtu.be/0ovCLxttJGE?si=84UxrXJaoZUGAtFr&t=1051).

My mind was particularly blown by the 3D graph demo in this video:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ijRBbtT2tgc?si=OsXR_xidthlQDquX&amp;start=1467" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

