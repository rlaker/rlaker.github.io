---
permalink: /
title: "Home"
excerpt: "Home"
author_profile: true
redirect_from: 
  - /home/
  - /home.html
  - /about/
  - /about.html
---

Data scientist currently working at [VIOOH](https://www.viooh.com/) <i class="fas fa-server"></i>. Before that, I studied the solar wind for my PhD at Imperial College London <i class="fas fa-solid fa-sun"></i>, where I also completed my physics degree <i class="fas fa-graduation-cap"></i>.

Most up to date writing is now on the [blog](/blog) page, with most recent posts shown below:

<style>
  .latest-posts .post-date {
    font-size: 16px;
    color: #666;
    margin-left: 8px;
  }
</style>

<ul class="latest-posts">
  {% for post in site.posts limit:5 %}
    <li>
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
      <span class="post-date">
        {{ post.date | date: "%b %-d, %Y" }}
      </span>
    </li>
  {% endfor %}
</ul>


## [Projects](/projects)

* [High altitude balloon](/projects/hab) that recorded images and atmospheric data to an altitude of 35km
* [Hackathon](/projects/hackathon) using reinforcement learning to dis/charge a battery based on fluctuating energy price
* [Visualisations](/vis) made to explain space physics concepts
* [Obsidian](/projects/obsidian) tutorial aimed at use in academia
* [MSci project](/projects/msci) predicting solar wind conditions for Parker Solar Probe's first orbit

## [Cooking](/cooking)

You can also try out my experimental cooking [page](/cooking). This is the public face of my [Obsidian](https://obsidian.md/) vault and shows how I catalogue and take notes on recipes from around the internet. Ingredients connect to recipes, which connect to cookbooks, which connects back to recipes and... you get the idea. It's a bit buggy but you get the idea.

## [Publications](/publications)

Read my [publications](/publications) from my time studying the solar wind with the Parker Solar Probe and Solar Orbiter missions. A full list can be found at [Google Scholar](https://scholar.google.com/citations?user=59iEPNwAAAAJ) <i class="fas fa-graduation-cap"></i>.

I also created a short animation to describe my PhD (below), along with some other useful <a href="/vis/">visualisations</a>.

<iframe width="560" height="315" src="https://www.youtube.com/embed/rI2yBMnZMpU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


