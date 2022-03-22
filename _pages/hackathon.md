---
permalink: /hack/
title: "Hackathon"
---

We recently entered (and won 🎉) a [Hackathon](https://2022.aihack.org) hosted by the Imperial College Data Science Society ([ICDSS](https://imperialdatasoc.co.uk/)).

Our code can be found on [GitHub](https://github.com/rlaker/Hackathon) <i class="fab fa-github"></i>. The state of the repo at time of submission can be found in the releases. However, we were very tired/rushed from the Hackathon format so we have tried to clean up the repo, and have added a few more features since the hackathon.

Here I will run through our solution, and try and give a [timeline](#timeline) for our progress throughout the 24 hours.

Our team consisted of:

- [Ronan Laker](https://github.com/rlaker) (Me)
- [Griffin Farrow](https://github.com/griffinfarrow)
- [Phil Moloney](https://github.com/Pmoloney2415)
- [Thomas Woolley](https://github.com/Woolley12345)

## The Problem

renewables are sporadic, need a way to store the energy and then deliver it to the customer.

That means that we have a battery storing charge, and we need to charge/discharge at certain points in the day.

Now, the price of energy varies throughout the day, so the question is, what strategy of charging/discharging will make the most profit over time.

### Energy price

initial data exploration showed that the price of energy was quite periodic/predictable throughout the day.

Here we show each day stacked on top of each other, where a clear sinusoidal pattern is present.

An interesting thing to note is the very large peaks later in the day, which could be a chance to make lots of profit.

## Our Solution

our initial thought was some kind of structured charge/discharge cycle, i.e. pick points from figure ?? and say every day we will charge/discharge here and hope that it follows the same pattern.

With such periodic data, this strategy would probably work, but it has some serious flaws and is not very flexible.

Since it was an AI hack, we turned our attention to ML. 

This problem didn't seem to fit into the classic ML problems of classification and regression. Since we are not trying to 

Although, we are PhD students, we have no focus/link to ML, therefore we weren't sure how to continue. We asked for a hint from the ICDSS, who suggested that Reinforcement learning could be useful

### Reinforcement Learning

So what even is reinforcement learning?

talk about an agent, with reference to a game, use the gifs from gym environments.

### Environment

talk about why gym is a thing.

so all we need to do is make an environment for an agent to play in, and then we can just use well established algorithms to find the best strategy.

### Training the Model

talk about OpenAI and PPO. Say this is an area I would like to explore a bit more, since all we did was model.learn()

which does show how mature the Python ML scene is.

## Results

show the trained vs untrained comparison

I really like this for showing how it initially takes random actions

## Room for improvement

go into detail about it getting too excited and selling early, which is a problem from us resetting to the start

so if we could make it wait for the peak, we could make much more money

one way to do this is to make it discharge slower, so that it does not run out of energy before selling right at the peak. 

we showed this as an example in the Hackathon by just changing the power to 0.5MW instead of 1 MW. You can see that this smooths out the profit, and actually increases it by £10,000!

ideally, we will incorporate a forecasting model, so the agent will know that a peak is about to happen and hopefully wait.


## Timeline

look through git graph 