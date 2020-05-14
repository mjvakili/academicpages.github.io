---
title: "Robust cosmological predictions with statistical inference and deep learning"
excerpt: "Reliable and tractable tests of cosmological models <br/><img src='/images/vmax/Screenshot 2020-05-06 at 9.54.19 AM.png'>"
collection: portfolio
---


Constraining models with the large datasets from galaxy surveys require employing statistical methods and complex numerical simulations. 

Bayesian modelling of the [SDSS](https://www.sdss.org/) data and testing the Hypothesis of galaxy-halo connection models
----------------------------------

We used Bayesian inference to test two *expensive* numerical models of galaxy formation with the clustering of galaxies in the SDSS data. These two models are expensive since they require generation of galaxy positions. 

One of the models relies on the assumption that mass of dark matter halos alone is sufficient in determining the statistical distribution of galaxies while the other states that you need other properties beyond mass in order to characterize the distribution of galaxies:

![](https://cdn.iopscience.com/images/0004-637X/872/1/115/Full/apjaaf1a1f2_lr.jpg)

Where the data points with error bars correspond to excess probability of finding pairs of galaxies in a given distance (the x-axis), the blue(red) correspond to the posterior predictions of the complex(simple) model. Each panel corresponds to a sample of galaxies within a brightness range. For the brightest galaxies (the bottom panels) the predictions of the two models are consistent but for less bright galaxies (the top panels) the more complex model provides a better description of the data. 

We use two information criteria: AIC & BIC to compare the models. We compute the difference in information criteria after considering a more complex model. The model with  less AIC (BIC) value is more favorable:

![](https://cdn.iopscience.com/images/0004-637X/872/1/115/Full/apjaaf1a1f6_lr.jpg)

 We note that, except for one of the galaxy samples, adding more complexity to the model does not necessarily lower the AIC(BIC). 

Bayesian Statistics versus Approximate Bayesian Computation in modelling large galaxy data
----------------------------------

Constraining cosmological models is done by approximating the posterior probability, probability of model conditioned on data (or in practice some statistical summary of the data). This is evaluated by using the Bayes rule in which we need to know the likelihood, probability of the data conditioned on the model, and the prior, the probability over the parameters of the model.

The likelihood is often assumed to be a multivariate Gaussian which is a sensible assumption for certain cosmological observables, but not all observables are Gaussian distributed. Furthermore, evaluating the likelihood requires knowing the covariance matrix of the observables which is expensive to compute. All of these may lead to biases in our approximation of posterior. 

Likelihood free inference methods do not rely on such assumptions. In particular, we looked at a complex simulated galaxy data set. We compared the performances of a likelihood free method (approximate Bayesian Computation with Population Monte Carlo Sampling) and a method relying on Multivariate Gaussian likelihood (with Markov Chain Monte Carlo Sampling of the posterior). 

![](https://github.com/mjvakili/mjvakili.github.io/blob/master/images/ABC/Screenshot%202020-05-04%20at%207.13.01%20PM.png?raw=true)

The *true* input parameters (parameters of the dark matter-galaxy connection model) are shown with stars while the 68% and 95% confidence intervals of the approximated posteriors are shown in blue (for the Gaussian likelihood method) and orange (for the likelihood free method). The methods seem to be consistent at recovering the input parameters. However, the Multivariate Gaussian likelihood method seems to give rise to wider tails, which could lead to mis-characterization of the uncertainties. 


Galaxy-dark matter connection with machine learning
-----------------------------------
One of the bottlenecks in statistical inference of cosmological models is connecting galaxies to dark matter in expensive numerical models. The most realistic frameworks of galaxy-dark matter connection are full hydrodynamic simulations. However, we cannot afford to run these simulations inside Bayesian inference of cosmological models. 

We came up with a fast and efficient approach to use these simulations in inference setups. 
What we are interested in is the probability of galaxy properties conditioned on dark matter properties. We learn this conditional probability distribution by training a mixture density network on hydrodynamic simulations:

![](https://home.strw.leidenuniv.nl/~vakili/images/vmax/MDN.png)

Estimating the covariance matrix of cosmological observables
-----------------------------------
Evaluating the cosmological likelihoods requires specifying the covariance matrix of a data vector which contains all the possible cosmological observables. The following matrix show the correlation matrix of some observales in a typical galaxy survey:

![](https://oup.silverchair-cdn.com/oup/backfile/Content_public/Journal/mnras/482/2/10.1093_mnras_sty2757/2/m_sty2757fig4.jpeg?Expires=1592154987&Signature=SXzCvjnaD0Ll~b8ej-iethtMjwVKkme0GW15djT16DluTxJfGV4Y3pwxIVgAZbmJ~W8sJbzjEq7LEP7npNvClqjBXHyxROVVfvDDN-57Jxc-aL5skxrtaoZh4Di13oqwC02Ta6hyzkNrzPJ71wTls2pQ9SjdBsuzIMkHmhHCO~sBBk3QNCxgbsUPxvqRYXGd8CtZ-TY9jucm0zn0ahpuegLQhiHKLsmTceaqIi48Ugp4grfn-KU9dTa5Nr3yUe-eQ7gQeQ5MBtO4eKwmPVZKTrOLANauQrXsJVHMD3MXCUNbnthvMtVKK~vAwdtx~dmWiMUgVLemlETDYzcENVWUqg__&Key-Pair-Id=APKAIE5G5CRDK6RD3PGA)

These observales include the excess probabiliy of finding pairs of galaxies within a given distance (in units of Mpc), and in different directions (hence, w1, w2, w3). In practice, these covariance matrices can be quite large. 

We need to estimate these matrices from many copies of the galaxy survey under consideration. 
The number of simulations is constrained by the noise properties of the estimated covariance matrix which itself depends on the length of observables.

A big challenge faced by large galaxy surveys is that these numerical simulations are prohibitively expensive, each requiring 1000s of CPU hours. We came up with some statistical tricks to make these simulations fast, each requiring a few hours. 
Our fast empirical method has a few parameter that are automatically fixed by MCMC.
You can look at the details of our work [here](https://academic.oup.com/mnras/article-abstract/472/4/4144/4094892?redirectedFrom=fulltext) and [here](https://academic.oup.com/mnras/article/482/2/1786/5136430).

































