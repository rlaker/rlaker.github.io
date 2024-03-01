---
tags: [til]
title: "Netflix personalises movie posters according to your preferences"
---

Today I learnt that Netflix take recommendations one step further, tailoring both the show **and** the artwork shown to the user.

As this [article](https://netflixtechblog.com/artwork-personalization-c589f074ad76) explains, a traditional recommendation algorithm would just say "you should watch these shows". But Netflix go one step further and ask themselves "how can we convince you that this show is worth watching?"

A simple example of this concept is showing you film posters with actors you recognise:

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*wRcWWaW0SDnT2xSI.)

Instead using a traditional A/B test to trial a new algorithm such as this, the authors describe how they use a [contextual bandit](https://towardsdatascience.com/an-overview-of-contextual-bandits-53ac3aa45034) algorithm to balance data exploration against providing the best experience for the user.


As they explain, there is a lot of nuance to this problem:
- you need to understand the causal effect of suggesting an artwork, i.e. would a user have clicked that show regardless of your proposed artwork?
- maintaining a recognisable image for that show. What if a user cannot find that artwork they saw last week?
- optimise across the whole screen. A poster of the main actor will not stand if a similar artwork is shown for all shows
- how to quickly learn the rules for a new title launch?

In this 2017 article, the authors say that a team of artists and designers create a wide range of images for each show. It would be interesting to learn how much of this has now been taken by generative AI.