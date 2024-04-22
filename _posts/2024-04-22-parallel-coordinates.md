---
title: "Visualising high dimensional data with Hiplot"
layout: single
excerpt: ""
tags: [til, visualisation]
---

Today I learnt about the **parallel coordinates** technique to visualise high dimensional data.

As demonstrated in this [talk](https://youtu.be/C9p7suS-NGk?si=tvpsTHYfemCvuuug), Hiplot is a tool that allows you to visualise high dimensional tabular data. Not only is this useful for exploring a new data set, but it can help you manually find cuts in the data to create a sort of dumb decision tree - useful as a target to beat with a fancier machine learning model later in the project.

The speaker (Vincent Warmerdam), who is also the creator of the amazing [calmcode project](https://calmcode.io/), demonstrates that such a tool can be used to [evaluate the best hyperparameters in a grid search](https://calmcode.io/course/hiplot/grid-search).

Try exploring how the house price in London varies with postcode, property type and number of bedrooms. Narrow down the range of each column, by dragging a box vertically, to see how the price changes (as represented by the colour and right most column) 

<iframe src="/files/london_house_prices_hiplot.html" width="100%" height="1400px" style="border:none;"></iframe>