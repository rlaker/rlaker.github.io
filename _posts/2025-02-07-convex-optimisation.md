---
title: "Learning about convex optimisation"
layout: single
excerpt: "made beautiful with Manim"
tags: [til, visualisation]
---

The more I'm learning about optimisation problems, the more I am rediscovering my love for "ah ha" moments in mathematics. Like anyone from my generation, when I need to learn about a new topic I turn to YouTube. Fortunately for me, [3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw), and his amazing [Manim](https://www.manim.community/) code, have significantly lowered the barrier to creating beautiful mathematical explainers.

I can now **see** how the simplex algorithm works in this video 

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/E72DWgKP_1Y?si=VEe9PU2mDEaYDjJu" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


or be guided through a clear narrative to motivate interior point methods here

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/uh1Dk68cfWs?si=W_dsxZ9i-GFWsquO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


Explainers such as these are intensely satisfying because the authors allow me to discover the concepts for myself. I don't need to be force fed mathematical notation before we get to some grand reveal. In this case, I can appreciate how we have transformed a hard problem (the primal) into an easier one (the dual) which we can tackle with trusty solvers like Newton's method.

Armed with this knowledge, I feel less intimated when I hear about Lagrange multipliers in other sources. I know, at a high level, we are just creating an equivalent problem that is easier to solve.

> Shameless plug, but I have also used Manim to create a [short video](https://youtu.be/rI2yBMnZMpU?feature=shared) about my PhD. I entered it to a competition with a 3-minute limit, so it's much faster paced than the videos above...