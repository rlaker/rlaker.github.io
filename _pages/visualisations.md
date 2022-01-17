---
permalink: /vis/
title: "Visualisations"
---

<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</style>

- [Interactive Orbits](/vis/orbits) - interactive figure to look at spacecraft orbits for reference.
- [What is Space Weather and Why Should You Care?](#what-is-my-phd-about) -  a video I made to explain my PhD
- [Parker Solar Probe Corotation](#parker-solar-probe-co-rotation) - a gif to show how Parker Solar Probe co-rotates 

## [Interactive Orbits](/vis/orbits) 

I have created an [interactive plot](/interactive orbits.html/) of spacecraft orbits using [Plotly](https://plotly.com/) , [astrospice](https://pypi.org/project/astrospice/) and [Spiceypy](https://spiceypy.readthedocs.io/en/main/). 

It includes the following spacecraft:

- Parker Solar Probe
- Solar Orbiter
- BepiColombo
- Earth (which is close enough for Wind, ACE and DSCOVR)
- STEREO-A

## What is my PhD about?

<iframe width="560" height="315" src="https://www.youtube.com/embed/rI2yBMnZMpU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I made this video for the Faculty of Natural Science [Showcase](https://www.imperial.ac.uk/natural-sciences/research/showcases-seminars/2021/), to describe my PhD in 3 minutes (ish). I made this animation with [ManimCE](https://docs.manim.community/en/stable/) and I the code can be found on [github](https://github.com/rlaker/visualisations/tree/main/Spaceweather%20video).

This touches on space weather and how the Sun can influence life on Earth. I also explain the Sun's magnetic field and the concept of the Parker spiral, while introducing the 5 spacecraft that is use including Parker Solar Probe and Solar Orbiter.

I also created this [poster](/files/oceans_poster.pdf) to explain my second first-authored [paper](/files/Laker2021a.pdf). The idea is that we want to see how the solar wind varies with latitude, so we are using a team of spacecraft to do that. These spacecraft each have their own unique abilities, hence the [Ocean's 11](https://www.redbubble.com/i/poster/Oceans-Eleven-by-Sun4FunShop/83681293.LVTDI) theme.

<embed src="/files/oceans_poster.pdf" width="560" height="750" />

<br>
<br>

# Parker Solar Probe Co-rotation

<img src="/files/psp_corotation_anti.gif" alt="Parker Solar Probe Corotation" class="center">

Inspired by [@matthen2](https://twitter.com/matthen2/), and adapting his Mathematica [code](https://pastebin.com/McQ5qwXr) to show why Parker Solar Probe's orbit exhibits a loop when viewed in a frame corotating with the Sun's surface. Code can be seen on [Github](https://github.com/rlaker/visualisations/) <i class="fab fa-github"></i>.

The animation shows one orbit of Earth (blue) and 3 orbits of Parker Solar Probe. The orbit of PSP is merely an ellipse, rather than being based on gravity, as this allows the animation to loop. The Sun rotates 12 times in this animation, similar to the real rotation rate of 27 days.
 
The two spirals represent the interplanetary magnetic field, which due to the Sun's rotation follows an Archimedian spiral known as the "Parker Spiral".

When moving into the frame that rotates with the surface of the Sun, Parker Solar Probe's orbit displays a loop. When comparing to Earth, you see that Parker Solar Probe slows down in this frame, and is overtaken by Earth.
