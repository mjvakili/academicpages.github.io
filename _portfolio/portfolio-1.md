---
title: "Automated detection and characterization of objects in multiband imaging data and large tabular data sets"
excerpt: "<br/><img src='/images/vmax/Screenshot 2020-05-06 at 3.56.08 PM.png'>"
collection: portfolio
---

I have been involved in projects designed with the aim of understanding the nature of dark matter and measuring the expansion rate of the universe with high precision.
[Sloan Digital Sky Survey](https://www.sdss.org/), [the Kilo Degree Survey](http://kids.strw.leidenuniv.nl/), 
and [Euclid](https://sci.esa.int/web/euclid). 

Here are some examples of what I have done in order to derive information from the large noisy data sets coming from these projects. 

Automated detection of red-sequence galaxies
----------------------------------

Bright red-sequence galaxies are rare objects that are ideal candidates for tracing the dark matter. 
I built a model for the purpose of detecting these galaxies from the large photometric data. 
This model consisted of these steps:

* Detection of objects (stars & galaxies) in multi-band imaging
* Engineering features that are informative about the red-sequence objects and their position in the space-time (lookback time or distance)
* Designing and training a data-driven model in order to detect red-sequence objects from 
the engineered features
* Selection of red-sequence objects from the data using the trained model 
* Using Bayesian statistics to estimate the positions (or the lookback time) of these objects

The following figures demonstrates a slice of the large-scale structure of our universe traced by the detected objects in our pipeline:

![](https://home.strw.leidenuniv.nl/~vakili/images/vmax/red_age.png)

In order to accurately measure the growth of structure, we need to be able to predict the lookback time of the detected objects with percent level errors.  Here is a comparison between the predicted lookback times (the x-axis) and the exact lookback times of 
the red-sequence objects for which we have exact lookback time measurements:

![](https://home.strw.leidenuniv.nl/~vakili/zphotscatter.png)

Automated Object classification in imaging surveys
------------------------------------

The ground based images of the dark sky are *noisy* and *blurry*:

* They are noisy because amount of time we can point the camera to a certain 
Object is finite resulting in receiving only a finite number of photons from the object. 
* They are blurry because the true light profiles of objects are [convolved](https://en.wikipedia.org/wiki/Convolution) with atmospheric turbulence and the telescope optics. 

Therefore, it may not be so straightforward to classify different object types with images taken from one band alone. Apart from the very bright and large star and galaxies, everything else looks like a blob:

![](https://arxiver.files.wordpress.com/2015/07/jongetal-1507-00742_f15.jpg?w=1200&h=)

The initial star-galaxy separation pipeline in our ground-based imaging project *Kilo Degree Survey* involves 
extraction of morphological features from objects and then feeding these features to a 
[Neural Network](https://stanford.edu/~shervine/teaching/cs-229/cheatsheet-deep-learning). 
This is motivated by the fact that galaxies tend to have more extended profiles than stars.
However, the shortcoming of this approach is that it fails in the regime where the objects are 
faint and small. 

In order to address this issue we take advantage of the fact that the light profiles from these 
objects are sampled in multiple filters. The number of filters can vary depending on the instrument. For instance in a mobile phone camera that we carry around there are three filters: *RGB*, whereas in our telescope camera, there are usually more filters:

![](https://github.com/mjvakili/mjvakili.github.io/blob/master/images/redsq/filters.png?raw=true)

The differences between the amount of light captured by these filters is a very good indicator of the type of object we are looking at in an image. Here are some examples of using these highly informative features to differentiate between objects

* Star-galaxy classification

With optical images alone, it is not possible to obtain a clean sample of red-sequence galaxies since old stars tend to cover the same regions of the color space. With the help of near infrared bandpasses (z, K bands above), it is possible to properly separate these two samples: 

![](https://home.strw.leidenuniv.nl/~vakili/images/redsq/stars_SVM_lum.png)

The red points in the following figure are the detected red-sequence candidates while the high confidence stars are shown in blue. The red points with open circles are red-sequence candidates that most likely belong to the sample of stars. These are considered anomalies and should be removed from the sample of red-sequence objects. Otherwise, our large-scale structure inference will be biased.

* Detection of massive blackholes 

Quasi Stellar Objects (Shortly Quasars) are massive blackholes that are constantly accreting matter. These objects are very useful for estimating the expansion rate of the unviverse. A big challenge in clean detection of these objects is that, in terms of morphology, they seem to be similar to a certain population of stars. Here are two examples. The object in the image on the left is a supermassive blackhole whereas the object in the right image is a bright star.

![](https://github.com/mjvakili/mjvakili.github.io/blob/master/images/vmax/QSO.png?raw=true)
![](https://github.com/mjvakili/mjvakili.github.io/blob/master/images/vmax/STAR.png?raw=true) 

Thanks to the multi-band pass imaging data, we can use the colors of these objects to pull them out of a large number of star-like in galaxy surveys. You can see the performances of different algorithms at detecting quasars in a sample of objects dominated by stars:

![](https://github.com/mjvakili/mjvakili.github.io/blob/master/images/vmax/roc.png?raw=true)

This is a roc curve with the y-axis showing the true positive rate (coverage of the underlying positive sample) and the x-axis showing the false-positive rate (1 - coverage of the underlying negative sample). 

Data-driven super-resolution recovery of the undersampled observations
------------------

Observations of the WFC3-IR band of the Hubble Space Telescope are undersampled by approximately a factor of 4. Reliable characterizations of objects requires having a model that can describe the high resolution light profiles of objects in the absence of undersampling. This is an example image taken by the camera in a crowded field:

![](https://home.strw.leidenuniv.nl/~vakili/images/superresolution/Screenshot%202020-05-04%20at%207.23.16%20PM.png)

Now, if you zoom into one of the objects, you see the following low-resolution image:

![](https://home.strw.leidenuniv.nl/~vakili/images/superresolution/star.png)

We designed a data-driven forward model of the observed profiles of individual objects. In this forward model, a local high resolution light profile is undersampled, shifted (according to the centers of the objects), scaled according to the brightness values of the objects, and lifted according to the brightness of the background. 
All of the following items are parameters of our model:

* The position of each objects
* The brightness of each object
* The brightness of the background
* The local high resolution profile of the stars

We used a gradient-based optimization method to find the local high resolution profile of the stars:

![](https://home.strw.leidenuniv.nl/~vakili/images/superresolution/Screenshot%202020-04-27%20at%2012.16.40%20AM.png)
