---
permalink: /vis/
toc: true
toc_sticky: true
title: "Visualisations"
header:
    overlay_image: /images/FrozenIn.png
    overlay_filter: 0.2
    actions:
    - label: "GitHub"
      url: "https://github.com/rlaker/Visualisations"
excerpt: "A few visualisations I have made to explain space physics concepts"
---

<style>
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
</style>

- [What is Space Weather and Why Should You Care?](#what-is-my-phd-about) -  a video I made to explain my PhD
- [Space physics explanations](#space-physics-explanations) - select animations from the Space weather video
- [Interactive Orbits](/vis/orbits) - interactive figure to look at spacecraft orbits for reference.
- [Parker Solar Probe Corotation](#parker-solar-probe-co-rotation) - a gif to show how Parker Solar Probe co-rotates 

The code for each of these projects can be found on [GitHub](https://github.com/rlaker/visualisations/) <i class="fab fa-github"></i>.

## What was my PhD about?

<iframe width="560" height="315" src="https://www.youtube.com/embed/rI2yBMnZMpU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I made this video for the Faculty of Natural Science [Showcase](https://www.imperial.ac.uk/natural-sciences/research/showcases-seminars/2021/), to describe my PhD in 3 minutes (ish). I made this animation with [ManimCE](https://docs.manim.community/en/stable/).

This touches on space weather and how the Sun can influence life on Earth. I also explain the Sun's magnetic field and the concept of the Parker spiral, while introducing the 5 spacecraft that is use including Parker Solar Probe and Solar Orbiter.

I also created this [poster](/files/oceans_poster.pdf) to explain my second first-authored [paper](/files/Laker2021a.pdf). The idea is that we want to see how the solar wind varies with latitude, so we are using a team of spacecraft to do that. These spacecraft each have their own unique abilities, hence the [Ocean's 11](https://www.designmantic.com/blog/wp-content/uploads/2017/10/The-Ocean-Eleven.jpg) theme.

<embed src="/files/oceans_poster.pdf" width="560" height="750" />

<br>
<br>


## Space physics explanations

I have separated out some of the animations from the above video. Feel free to use in any presentations (with credit please).

### Magnetic field polarity

<img src="/files/polarity.gif" alt="Parker Solar Probe Corotation" class="center">

### Parker spiral

<img src="/files/SpinSolarWind_480p.gif" alt="Parker Solar Probe Corotation" class="center">

### Spacecraft Constellation

<img src="/files/spacecraft.gif" alt="Parker Solar Probe Corotation" class="center">


## [Interactive Orbits](/vis/orbits) 

I have created an [interactive plot](/vis/orbits) of spacecraft orbits using [Plotly](https://plotly.com/) , [astrospice](https://pypi.org/project/astrospice/) and [Spiceypy](https://spiceypy.readthedocs.io/en/main/). 

It might look a bit crazy to start with, but you can zoom in by dragging and remove spacecraft by clicking the legend.

It includes the following spacecraft:

- Parker Solar Probe
- Solar Orbiter
- BepiColombo
- Earth (which is close enough for Wind, ACE and DSCOVR)
- STEREO-A

## Parker Solar Probe Co-rotation

<img src="/files/psp_corotation_anti.gif" alt="Parker Solar Probe Corotation" class="center">

Inspired by [@matthen2](https://twitter.com/matthen2/), and adapting his Mathematica [code](https://pastebin.com/McQ5qwXr) to show why Parker Solar Probe's orbit exhibits a loop when viewed in a frame corotating with the Sun's surface.

The animation shows one orbit of Earth (blue) and 3 orbits of Parker Solar Probe. The orbit of PSP is merely an ellipse, rather than being based on gravity, as this allows the animation to loop. The Sun rotates 12 times in this animation, similar to the real rotation rate of 27 days.
 
The two spirals represent the interplanetary magnetic field, which due to the Sun's rotation follows an Archimedian spiral known as the "Parker Spiral".

When moving into the frame that rotates with the surface of the Sun, Parker Solar Probe's orbit displays a loop. When comparing to Earth, you see that Parker Solar Probe slows down in this frame, and is overtaken by Earth.
