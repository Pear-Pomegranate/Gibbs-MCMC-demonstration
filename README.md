# Gibbs-MCMC-demonstration
a simple illustration for Gibbs MCMC and its realization for a linear model.

# Gibbs Sampling for MCMC on linear model

This project show a straightforward implementation of using Gibbs sampling MCMC to estimate the hyperparameters of a linearmode for a given data set. The aim is to show how the algorithms are set up and derived. 


## Authors

- [@Pear-Pomegranate](https://github.com/Pear-Pomegranate)


## Appendix

###Gibbs Sampling tactic for MCMC on linear model

1. Initialize $a^{(0)}, b^{(0)}, \tau^{(0)}$
where $y = ax+b+\epsilon$, where the random noise $\epsilon \sim N(0,\sigma ^2 = 1/\tau)$. 
    
For this simple model, the conditional probalitity distribution function $f(\theta | D)$, i.e. posterior distribution can be worked out analytically. Here, the probability event $\{D\}$ stands for given (seen) the collected input data set $D$ as a condition (event). Event $\{\theta\}$ stands for the case when the hyperparamters in the proposed model is descibed by $\theta$. For linear model, data set $D$ is a set of $(x,y)$ coordinates in feature-target space.

The prior $f(\theta)$ is often assumed to be a normal distribution. The likelihood $f(D|\theta)$ is modeled as a normal due to the proposed modeling $y = ax+b+\epsilon$, where 
$\epsilon \sim N(0,1/\tau)$.


2. Sampling from $b^{(1)} \sim f(b| a^{(0)}, \tau^{(0)}, D)$, to obtain a sample for $b^{(1)}$.
    
To work out the posterior we start from the prior of $b$, and its p.d.f. is 

$$f(b) = \sqrt{\frac{\tau_0}{2\pi}}e^{-\frac{(b-\mu_0)^2\tau_0}{2}}$$
because for the proposed model $y_i \sim N(ax_i+b,1/\tau)$, therefore

$f(y_i,x_i| b,a,\tau) = \sqrt{\frac{\tau}{2\pi}}\exp(-\frac{\tau(y_i-ax_i-b)^2}{2})$

$$f(D|b,a,\tau) = \prod_{i=1}^N f(y_i,x_i| b,a,\tau)$$

As $f(b|D,a,\tau)\propto f(D|b,a,\tau) f(b)$, we can work out the conditional probability distribution

$$ (b|D,a,\tau)  \sim N(\frac{\tau_0 \mu_0+ \tau { \sum_ {i=1}^N } (y_i-ax_i)}{\tau_0+N\tau}, \frac{1}{ \tau_0+N \tau})
$$


To prove the above we only need to take the natural log of both sides of the p.d.f. and match their coefficients to a normal distribution, and in fact that is also how we obtained the mean and vairance of the posterior in the above expression.

Similarly, the other posterior are subject to

$$ (a|D,b,\tau) \sim 
N\left(\frac{\tau_1 \mu_1 + \tau \sum_{i=1}^{N} (y_i - b)x_i}{\tau_1 + \tau \sum_{i=1}^{N} x_i^2}, \frac{1}{\tau_1 + \tau \sum_{i=1}^{N} x_i^2}\right),
$$
where we have assumed 
$$ a \sim N(\mu_1,1/\tau_1)$$

For the posterior distribution of the variance of a normal distribution, we choose the prior to be the inverse Gamma distribution, so that the posterior is the inverse Gamma as well (This is a technique called conjugate prior). But we are using the precision variable instead of the variance, so the prior is subject to Gamma distribution.  
$ \tau \sim \Gamma(\alpha, \beta)$

$$
f(\tau) = \frac{\beta}{\Gamma(\alpha) } (\beta\tau)^{\alpha-1} e^{-\beta \tau}
$$

$$
\tau \mid D, b, a \sim \Gamma \left( \alpha + \frac{N}{2}, \beta + \frac{1}{2} \sum_{i=1}^{N} (y_i - a x_i - b)^2 \right)
$$

3. Sample $a^{(1)}$ from $f (a|D,b^{(1)},\tau^{(0)})$
4. Sample $\tau^{(1)}$ from $f (\tau|D,a^{(1)}, b^{(1)})$
    Collect $(a^{(1)}, b^{(1)}, \tau^{(1)})$
5. repeat step 2-4 T times
6. after the hyperparameters are stable(balanced), take those as valid random samples, and take the sample average for the estimated hyperparameters.
## Acknowledgements

This project was inspired by a tutorial by [ asia1987 ] and his video at:

https://www.bilibili.com/video/BV17D4y1o7J2?p=6&vd_source=760a7049278c172cc4351b7cf04b9c6a

which referenced notes by [ Steven Morse ] on https://stmorse.github.io/journal/gibbs.html,

one can also find similar tutorials by [ Kieran R Campbell ] at https://kieranrcampbell.github.io/posts/bayesian-linear-regression/?locale=en
