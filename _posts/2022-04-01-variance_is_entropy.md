---
layout: post
title:  'Rethinking Correlation, Part 1 : Variance is Cross Entropy'
date:   2021-04-01
splash_img_source: /assets/img/traffic-332857_1920.jpg
splash_img_caption: Representative image. Image by <a href="https://pixabay.com/users/jonbonsilver-236141/">jonbonsilver</a> on Pixabay.
---
In this 3 part blog post, I would like to show a new way of looking at correlation/covariance. While [Rodgers](https://www.stat.berkeley.edu/~rabbee/correlation.pdf) already provides us with 13 different ways to look at correlation, I would like to show that correlation can also be viewed as cross entropy.

 We will first begin with variance, and show how variance can also be viewed as cross entropy. This will then easily allow us to apply the same logic to covariance, leading us to a new perspective on correlation. One that will show us more clearly its short comings. Finally, we will explore better ways to compute correlation. Let's begin.

 Variance is said to be the "spread" of the data from the mean, but is this really what it is? Does variance even makes sense for certain distributions, such as the Bernoulli Distribution, power law distributions with thick tails, or categorical distributions? If we really want to measure the spread of the data, is there a better way? We will show that variance is actually a special case of entropy, and that the ideal method of computing spread is with entropy.

The variance of a random variable $X$ is $Var(X) = E[(X-\mu)^2]$, where $\mu$ is the mean (assume finite mean). We will derive this expression, plus or minus some constants starting from cross entropy. Specifically, let $X \sim p(x)$, where $p(x)$ is the true but unknown distribution. We will then estimate $X$ using another distribution $q(x)$, then compute the cross entropy: $E_p[log(q))]$. Our estimated distribution $q(x)$ will be a standard normal $\mathcal{N}(\mu,1)$, shifted by $\mu$; the mean of our random variable. 

The cross entropy of a distribution $p(x)$ with an estimated shifted normal distribution $q(x)$ is

$$ CE(p,q) = -E_p[log(q)]$$

Substituting for $q(x) = \frac{1}{\sqrt{2\pi}}e^{-\frac{1}{2}(x-\mu)^2}$, we get 

$$ CE(p,q) = -E_p[log(\frac{1}{\sqrt{2\pi}})] - E_p[log(e^{(-\frac{1}{2}(x-\mu)^2})]$$

Simplifying further we get

$$ CE(p,q) = log(\sqrt{2\pi}) + \frac{1}{2}E[(x-\mu)^2]$$

The first term is just a constant we can ignore, the second term is precisely Variance! So we have 

$$ CE(p,q) = log(\sqrt{2\pi}) + \frac{1}{2}Var(X)$$

When $p=q$, variance is equal to Entropy plus or minus some constants!

Cross entropy is used to measure the similarity between 2 distributions. It is commonly used in machine learning when fitting a parametrized distribution to some data. To compute the "spread" of a distribution, Entropy is a better measure. Variance can be a good proxy when you don't know the true distribution $p(x)$ and you have good reason to believe the distribution can be well approximated with a standard normal distribution. Otherwise variance doesn't make sense for distributions that are not well approximated via a normal distribution. 

To summarize, if you want to measure the spread of some data, use Entropy. Variance is a special case of Entropy, it is the cross entropy of a standard normal with the true distribution. When you use variance, you are making the assumption that a standard normal distribution can be a good estimate for the true distribution. 