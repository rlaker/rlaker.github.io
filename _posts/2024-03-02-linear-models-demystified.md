---
title: "An interactive guide to linear models"
layout: single
excerpt: "Exploring how to use linear models for timeseries prediction"
toc: true
toc_sticky: true
tags: [python, statistics]
---

This post, and associated [notebook](https://github.com/rlaker/linear_model_tutorial), will cover linear regression along and some of the fancy tricks that can make it an incredibly useful tool. By starting with simple linear models, rather than neural networks, we can learn about some of the associated techniques (e.g. [avoiding overfitting](#avoiding-overfitting)) in a cleaner way.

# Interactive figure

I have created an interactive visualisation below to explain the underlying concepts and make tools like [prophet](https://facebook.github.io/prophet/) seem less like magic... For example, try adjusting the coefficients *below* to model weekly seasonality with one of three linear models ([dummy](#dummy-variable), [radial basis function](#radial-basis-functions) or [Fourier](#fourier-components)). For further explanation, see the [seasonality section](#seasonality)
<iframe src="/files/shiny_linear_model/shiny_linear_model.html" width="100%" height="1000px" style="border:none;"></iframe>
*This is actually running Python in your browser! Should take about 30 seconds to load, see [this previous post](https://www.ronanlaker.com/running-python-apps-in-the-browser/) or [Shinylive](https://shiny.posit.co/py/docs/shinylive.html)*


# Straight line fit

Before researching for this blog post, I naively assumed that linear regression is restricted to a straight line fit, of the form: 

$$y = m \times x + c$$

where $y$ is our target variable, $x$ is the feature, $c$ is the intercept and $m$ is the coefficient for $x$.

However, it turns out that the "linear" in linear regression actually refers to the relationship between the target and feature variables, i.e. each feature variable has a single constant coefficient to describe its relationship to $y$. This means that we can rewrite our model in terms of vectors and matrices, allowing us to extend the straight line to many dimensions: 

$$ \vec{y}  = \mathbf{X} \vec{\beta} $$

where $\vec{\beta}$ is a vector of $n$ coefficients ($n \times 1$):

$$ 
\vec{\beta} = \begin{pmatrix}
\beta_{0} \\
\vdots \\
\beta_{n}
\end{pmatrix}
$$

and $\mathbf{X}$ is a ($ i \times n$) matrix containing the feature variables:

$$
\mathbf{X} = \begin{pmatrix}
1 & x^{(0)}_{1} & \cdots & x^{(0)}_{n-1} \\
\vdots & \vdots & \ddots & \vdots  \\
1 &  x^{(i)}_{1} & \cdots & x^{(i)}_{n-1}
\end{pmatrix}
$$

When we "fit" our model to the data, we are changing the values of the coefficients ($\vec{\beta}$) with the aim of minimising the error between the observed data and our predictions. This is normally represented by the [mean squared error](https://en.wikipedia.org/wiki/Mean_squared_error) (MSE).

With a bit of linear algebra, we can [solve this equation analytically](https://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/13/lecture-13.pdf), leading to $\hat{\beta}$ minimising the mean squared error:

$$ \hat{\beta} = (\mathbf{X}^{T} \mathbf{X})^{-1} \mathbf{X}^{T} \vec{y} $$

Later on, we will see how we can avoid [overfitting](#avoiding-overfitting) by modifying the loss function to include some form of [regularisation](https://en.wikipedia.org/wiki/Regularization_(mathematics)).

After fitting our model, the coefficients can instantly tell us the effect of each of our feature variables. For example, we could say that for every $1^{\circ}$ temperature increase we expect the chance of rain to decrease by X amount. Such a simple statement is notoriously hard to make when using more complicated models, like neural networks. 

# Seasonality

> But what if our data shows some structure?

For example, I have created some fake data that has a clear weekly seasonality. How can we possibly model this with a straight line?
Well, with some clever feature engineering tricks, we can create a whole host of new features to model complex situations like this.

![](/files/linear_model/plain_seasonality.png)


## Dummy variable

<img alt="Demonstration of how changing the coefficients affects the dummy model" width ="400" height="900" src="/files/linear_model/dummy.gif">

*Demonstration of dummy variables with the [figure at the top of the page](#interaction-figure)*

<!-- <iframe src="/files/linear_model/shiny_linear_models/dummy_model/index.html" width="100%" height="1000px" style="border:none;"></iframe> -->
<!-- *You can try and "fit" this dummy model by adjusting the coefficients* -->

Using our intuition, we might think it is sensible to try and calculate the contribution from each day of the week. This can be represented by creating a dummy variable for each day, where $x_{\textrm{Monday}}$ is only equal to $1$ on a Monday and $0$ everywhere else. We can then scale this new feature with a coefficient, $\beta_{\textrm{Monday}}$, that represents **the average $y$ on a Monday**.

This new feature can be created with the following code:

```python
def dummy(x, start, width = 1):
    # repeat every 7 days
    x_mod = x % 7
    # Create a boolean array where True is set for elements within the specified range
    condition = (x_mod >= start) & (x_mod < start + width)

    # Convert the boolean array to an integer array (True becomes 1, False becomes 0)
    return condition.astype(int)
```

While these dummy variables are useful for demonstrating that coefficients effectively represent the height of each feature, the final result does not look natural. The step-like shape means that we are expecting a significant change as soon as it goes 1 minute past midnight!

## Radial Basis Functions 

<!-- <iframe src="/files/linear_model/shiny_linear_models/rbf_model/index.html" width="100%" height="1000px" style="border:none;"></iframe> -->
<!-- *You can try and "fit" this RBF model by adjusting the coefficients AND the width* -->

<img alt="Demonstration of how changing the coefficients affects the radial basis function model" width ="400" height="900" src="/files/linear_model/radial.gif">


To create a smoother seasonality pattern, we can replace the step function with a repeating Gaussian distribution centered around each day of the week. This is referred to as a [radial basis function](https://en.wikipedia.org/wiki/Radial_basis_function):

$$
y = e^{- (x - \textrm{center})/(2 \times \textrm{width})}
$$

This now lets the influence of a single day seep into adjacent days, leading to a much more pleasing final fit. 

We can create the new features with the code below:

```python
def rbf(x, width, center):
    # repeat every 7 days
    x_mod = x % 7
    center_mod = center % 7
    
    # Original Gaussian
    gauss = np.exp(-((x_mod - center_mod)**2) / (2 * width))
    
    # Gaussian shifted by +7
    gauss_plus = np.exp(-((x_mod - (center_mod + 7))**2) / (2 * width))
    
    # Gaussian shifted by -7
    gauss_minus = np.exp(-((x_mod - (center_mod - 7))**2) / (2 * width))
    
    # Sum the contributions
    return gauss + gauss_plus + gauss_minus
```

You may have noticed that we know have an extra variable `width`, which controls the width of our individual Gaussian functions. This variable is not actually part of the Linear regression, but is used to generate the features that the model learns from. This is therefore a [hyperparameter](https://en.wikipedia.org/wiki/Hyperparameter_(machine_learning)) that we can [tune](https://towardsdatascience.com/hyperparameter-tuning-for-machine-learning-models-1b80d783b946).

> For example, try changing the width parameter in the [top figure](#interactive-figure) to see how it affects the MSE

## Fourier components

<!-- <iframe src="/files/linear_model/shiny_linear_models/fourier_model/index.html" width="100%" height="1000px" style="border:none;"></iframe> -->
<!-- *You can try and "fit" this Fourier model by adjusting the coefficients AND the number of orders* -->

<img alt="Demonstration of how changing the coefficients affects the Fourier model" width ="400" height="900" src="/files/linear_model/fourier.gif">

We can also model seasonality with Fourier components, which less prone to overfitting than the [radial basis functions](#radial-basis-functions) above.

This trick works because any function can be represented as a sum of infinitely many sine and cosine waves added together, known as a [Fourier transform](https://www.jezzamon.com/fourier/index.html), as demonstrated below.

<iframe src="https://3b1b-posts.us-east-1.linodeobjects.com/content/lessons/2019/fourier-series/fourier_intro_2.mp4" width="560px" height="315px" style="border:none;"></iframe>

Therefore, we can create a new feature for each of these sine and cosine waves, with each coefficient representing the amplitude of that individual sine wave. This is equivalent to the coefficients that can be found by performing a Fourier transformation. The only difference is that we cannot have infinitely many sine waves in our linear regression, however, this is actually helpful to [avoid overfitting](#avoiding-overfitting)!

The figure below displays the first and second order Fourier components that are used in the [interactive figure](#interactive-figure)

![](/files/linear_model/fourier_orders.png)

# Weighting

> What if I don't trust old data as much as recently recorded data?

Many fitting libraries, such as [scikit-learn](https://scikit-learn.org/stable/index.html), allow you to specify an `importance` to each of the data points. Therefore, we can give exponentially decaying importance to older measurements as a way of ignoring potentially misleading historic data. The amount of "forgetfulness" then becomes another `hyperparameter` in our model.  

![](/files/linear_model/weighted_fit.png)

# Interaction terms

> what if the seasonality changed over time? 

In the figure below, the amplitude of the seasonality component is increasing each week. Not to worry, we can just create several extra features representing the interaction of day of the week with time. However, this kind of feature engineering can become tedious, hindering our flow. We now want to describe our models with a [statistical language](https://cran.r-project.org/doc/manuals/R-intro.html#Formulae-for-statistical-models). For example, a straight line fit ($y = m \times x + c$) would be written as

```
y ~ 1 + x
```

with the `1` representing the intercept, with a coefficent being automatically gerenated for each variable, `x`. This might seem like overkill for such a simple model, but the real beauty becomes apparent with more complex problems...

Returning to the changing seasonality problem, we might start by saying there is an interaction between time, `t`, and the first Fourier cosine component, `cos_1`:

`y ~ 0 + t*cos_1`

The `*` operator will automatically generate coefficients for `t`, `cos_1` and the interaction between `t:cos_1`

> Note: In the [notebook](https://github.com/rlaker/linear_model_tutorial), the [patsy](https://patsy.readthedocs.io/en/latest/formulas.html) package is used to convert these statistical formulas into a feature vector

![](/files/linear_model/simple_interaction.png)

After fitting this formula, we get three coefficients that we can interpret as:
- For every 1 unit of `t`, y will reduce by 0.01 (negative slope straight line)
- The base amplitude for the `cos_1` variable is -0.41
- For every unit of `t` this amplitude will reduce by another -0.04 (making the amplitude larger) 

Including the full interaction terms is as easy amending the formula like so:

`y ~ t * (cos_1 + sin_1 + cos_2 + sin_2)`

which will autogenerate 9 features and coefficients

![](/files/linear_model/interaction_fit.png)

However, there is no free lunch in modelling. Although we can now create hundreds of features using this statistical language, we now need to keep control of them...

# Avoiding overfitting

One of the hardest parts of fitting any model, either simple or complex, is making sure we don't overfit. We want to capture the general shape of the observed data, but do not want to perfectly predict each point, as this will just be fitting the noise rather than learning the underlying pattern.

![](/files/linear_model/overfit.png)

One way to combat overfitting is with a technique called [regularisation](https://statquest.org/regularization-part-1-ridge-regression/), which adds a penalty term to the regression formula. Since the model will be penalised for each variable it adds, only the most useful features are included. This can be seen by comparing the figures above (regular fit) and below (with regularisation).

The exact form of the penalty can change, with the two main being [LASSO](https://en.wikipedia.org/wiki/Lasso_(statistics)) and [RIDGE](https://en.wikipedia.org/wiki/Ridge_regression) regression having a linear and quadratic penalty term, respectively. 

> An alpha parameter is often used to specify the trade-off between the model's performance on the training set and its simplicity. So, increasing the alpha value simplifies the model by shrinking the coefficients.

![](/files/linear_model/not_overfit.png)

# Bayesian framework

> At this point, you might be wondering what are the uncertainties on these coefficients? 

While we can solve linear regression with matrix operations (as shown [earlier](#straight-line-fit)), this is not the best way to understand the sensitivity in our coefficients. Instead, we can fit our model within a Bayesian framework. Not only will this automatically provide an uncertainty estimate in the form of a posterior distribution, but we can actually incorporate domain knowledge into our fitting (through the prior distribution). 

To explain how, we first need a quick crash-course in [Bayes' theorem](https://betterexplained.com/articles/an-intuitive-and-short-explanation-of-bayes-theorem/) (or read this [introduction](https://koaning.io/posts/introduction-to-inference/)). 

Bayes' theorem allows us to mathematically update the probability of an event happening, $P(E)$, based on some new data, D.

$$
P(E|D) =  \frac{P(D |E) \times P(E)}{P(D)}
$$

For example, we might be playing a game of heads or tails where I have to guess the coin face. I give you the benefit of the doubt and initially believe that there is a 50/50 chance of the coin being fair, $P(Fair) = 0.5$, which is called my `prior` assumption.

> If you get three heads (3H) to start the game, do I still believe the coin is fair?

If the coin is fair, then on each coin flip the probability of getting heads is 50%. So the probability of $n$ heads in a row is equal to 

$$
P(H|\textrm{Fair}) = 0.5^{n}
$$

We will assume that if the coin is not fair the probability of $n$ heads is 

$$
P(H|\textrm{Not Fair}) = 0.75^{n}
$$

On each coin flip, I can update my beliefs to get a `posterior` probability, i.e. $P(Event\|Data)$. Doing the maths, we can say that:

$$
P(\textrm{Not Fair}|3H) = \frac{0.75^{3} \times 0.5}{0.75^{3} \times 0.5 + 0.5^{3} \times 0.5} = 77\%
$$

So, maybe we need to have a chat...

Using this analogy, we start off by assuming a prior distribution for each of our linear regression coefficients (usually just a Gaussian). We can then use Bayes' theorem to update our beliefs of these coefficients, given the observed data. With each new data point, we get a better understanding of the posterior distribution for each function!

This gets even more fun when you realise we don't have to use the standard Gaussian priors. We can actually set up the priors to include knowledge we already have about the problem, i.e. we might already have some idea of what the seasonality should be, allowing a fit that might [not be normally possible with limited data](https://juanitorduz.github.io/short_time_series_pymc/).

![](/files/linear_model/posterior_coefficients.png)

The above figure shows the final coefficients along with the 94% confidence interval. This allows us to quote things as "we are 94% confident that Sunday gives an extra 0.746 to 1.071 units". We can also make statements such as "we are the least sure about the 7th day". Interestingly, the model is very confident on the amount of noise in the data (the `y_sigma` parameter), probably because I made this fake data with a single noise parameter...


# Comparison to the prophet model

The [prophet model](https://facebook.github.io/prophet/) from Facebook provides many of these functionalities straight out of the box, and does such a good job of abstracting this complexity away that it kind of seems like magic. However, it is important to realise that prophet is just using some of the tricks from earlier:
- Is a linear model
- Models weekly seasonality with [radial basis function](#radial-basis-functions) dummy variables
- Models yearly seasonality with [Fourier](#fourier-components) components (which you can change the order of with the `yearly_seasonality` parameter)
- Can fit in a [Bayesian framework](#bayesian-framework), giving uncertanties in the final predictions

To be fair, prophet also has a few tricks up its sleeves: it can model increases in sales on holidays in that region; handle logistic growth (along with changepoints) as well as deal with gaps in the data.

I would personally use prophet to rapidly experiment with seasonality, holidays and extra regressors. I would then re-create the model in PyMC to add more custom features, see this [example](https://www.pymc.io/projects/examples/en/latest/time_series/Air_passengers-Prophet_with_Bayesian_workflow.html) of how to do this.

As an demonstration of how simple prophet is, we can condense most of the ideas in this blog post to the following code:

```python
m = Prophet(
    weekly_seasonality = True,
    yearly_seasonality=5
)

m.add_country_holidays(country_name='USA')

m.fit(df)

future = m.make_future_dataframe(periods=365)

forecast = m.predict(future)
forecast[["ds", "yhat", "yhat_lower", "yhat_upper"]].tail()
```

![](/files/linear_model/prophet.png)

We can also break down each of these components to extract more information

![](/files/linear_model/prophet_components.png)

For a more detailed tutorial, see the [official documentation](https://facebook.github.io/prophet/docs/seasonality,_holiday_effects,_and_regressors.html#modeling-holidays-and-special-events)

# Acknowledgments

This post, and associated [notebook](https://github.com/rlaker/linear_model_tutorial), is heavily inspired by this fantastic [talk](https://youtu.be/68ABAU_V8qI?si=gHJaD7s88sX3YpWE) by [Vincent Warmerdam](https://koaning.io/), along with this notebook investigating cycling patterns in [Seattle](https://jakevdp.github.io/blog/2014/06/10/is-seattle-really-seeing-an-uptick-in-cycling/).
