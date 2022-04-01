---
title: "Hackathon"
toc: true
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

The energy supplied by renewables is sporadic, and has to be stored in batteries when the demand is low. It is then ready to sell to the customer when demand then becomes high.

Now, the price of energy varies throughout the day, so the question is:

> What strategy of charging/discharging will make the most profit over time?

### Energy price

Initial data exploration showed that the price of energy was quite periodic/predictable throughout the day. This periodic nature is demonstrated in the figure below, where the price per day is superposed onto the same axes. The large spikes in the afternoon are quite obvious in this figure, and could be something to exploit later in the project.

![Superposed energy price per day in the entire dataset]("/images/hackathon/superposed.png")

_**Figure 1. Superposed energy price per day in the entire dataset. Spikes in the afternoon are interesting.**_

## Our Solution

Our initial thought was some kind of structured charge/discharge cycle, i.e. pick points from Figure [1](#energy-price) and say every day we will charge/discharge here and hope that it follows the same pattern.

With such periodic data, this strategy would probably work, but it has some serious flaws and is not very flexible.

Since it was an AI hack, we turned our attention to machine learning (ML). This problem didn't seem to fit into the classic ML problems of classification and regression that we have some basic knowledge of.

Although we are PhD students, we have no focus/link to ML, therefore we weren't sure how to continue. We asked for a hint from the [ICDSS](https://imperialdatasoc.co.uk/), who suggested that reinforcement learning could be useful...

## Reinforcement Learning

So what even is reinforcement learning?

Using the "pole" example from [OpenAI](https://gym.openai.com/docs/). We have an `agent` (the hand) who can take `actions` (left or right) in this `environment` (keeping a pole upright).

For each of these `actions`, we can `reward` the agent in the aim that they will learn a successful strategy. In this example, the `reward` will be the time that the pole is upright.

![Pole environment](/images/hackathon/cart_pole.gif)

So to summarise we want to teach the `agent` how the maximise the `reward`.

![]("/images/hackathon/flow_chart.png")

### Environment

In our case, we can adapt this example to our problem:

#### Agent

The `agent` is now our battery.

#### Actions

1. Buy (charging with some energy)
2. Hold (do nothing)
3. Sell (discharging some energy)

#### Reward

We now want to reward the `agent` for:

1. Selling when the price is high
2. Buying when the price is low

So achieve this we created a simple `reward` for each `action` that the `agent` took.

$$
reward = -\Delta E * (current\ price - expected\ price)
$$

For example, if we are **SELLING** then we will be discharging, so $\Delta E < 0$. If we sell when the price is higher than expected ($current\ price - expected\ price$ > 0) then we will have a positive `reward` ($>0$). If the price was lower than expected, then we would have a negative `reward` ($<0$), which is a punishment.

### Training the Model

Since we have systematically defined the `reward`, `agent`, `actions`, then we can use the base `environment` class from OpenAI's [Gym](https://gym.openai.com/docs/). This has the massive advantage of being compatible with OpenAI's [PPO algorithm](https://openai.com/blog/openai-baselines-ppo/) through the [Stable-baselines3](https://stable-baselines3.readthedocs.io/en/master/guide/examples.html) package. This was crucial as we could just write `model.learn()` and let the underlying Neural Network find a good strategy, which is not something we could have come up with in 24 hours!

## Results

![]("/images/hackathon/before_training.png)
![]("/images/hackathon/after_training.png)

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