---
title: "In London you are always within 878m of a pub"
layout: single
excerpt: ""
tags: [visualisation]
---

I went to a PyData meetup the other day and heard Ben Guerin tell the story behind his viral website [ismypubfucked.com](https://www.ismypubfucked.com/), which does what it says on the tin (his joke). That got me thinking, at any given time in London, how far away from a pub am I?

Well, to answer that we need to create a fine grid of query points across London and then find the closest pub at each point by comparing distances. With a 5km radius around Buckingham palace I ended up with around 63,000 points to check against all 3631 pubs I could find on OpenStreetMap. That's way too many distance calculations, if only there was a better way to find the nearest neighbouring pub...

Obviously this is a solved problem. We just need to create a binary tree to represent our points and then traverse the tree in a clever way to find the nearest neighbour. After converting the lat/lon of the pubs into radians we also need to tell the BallTree that we want to calculate the distance with the Haversine formula since we are all living on a big ball.

```
from sklearn.neighbours import BallTree

tree  = BallTree(pubs, metric = 'haversine')

nearest_neighbour = tree.query(query_point, k=1)
```

Normally I would leave it at that, but since I just got Claude code I thought I might as well create some cool diagrams to prove how much of a speed improvement we can get by using this data structure. By creating a BallTree we have our 3631 pubs as the leaf nodes of the tree, and 3630 internal nodes that represent circles that enclose all the pubs in the leaves at deeper levels. All the nearest neighbour algorithm has to do is progress down the tree, checking the internal nodes at each level. For an internal node with centre $C$ and radius $R$, the distance:

$$\textrm{distance}(\textrm{query}, C) - R$$

represents the minimum distance to the query point for the pubs within this circle. If this distance is larger than the closest distance found so far, we can ignore this entire branch of the tree!

For our example, we only checked 41 out of 7261 nodes of the tree, which is visualised in the plot below. Interestingly, we actually had to check two completely separate branches of the tree all the way down to the leaf node. 

<iframe src="/files/pubs_london/tree.html" width="800px" height="600px" style="border:none; display:block; margin:0 auto;"></iframe>

Now we have a quick way to find the closest pub, we can create our grid within 5 km of the palace and create a map like this. You can clearly see the effects of Hyde park and Regent's park, but because of the many pubs looking over the Thames this is not visible at all!

<iframe src="/files/pubs_london/pub_desert.html" width="800px" height="600px" style="border:none; display:block; margin:0 auto;"></iframe>


We have our answer! You are always within 878m of a pub in London, with the furthest point from any pubs being the football pitches in Regent's park.

> You can find the code in this [repository](https://github.com/rlaker/london_pub_desert)