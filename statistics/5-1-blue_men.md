[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

>> First, load the requisite libraries:
> ```python
> import numpy as np
> import scipy.stats
>import brfss
>import first
>import analytic
>import thinkstats2
>import thinkplot
> ```
> We are told that the distribution is roughly normal and given the mean and standard deviation of weights in the brfss dataset, but let's go ahead and read these off anyway.  First, read in the data:
> ```python
> df = brfss.ReadBrfss()
>heights = df[df.sex==1].htm3.dropna()
> ```
>Notice we set 'sex==1' to get the data pertaining to men.  Next, we fit this to a normal model using the
>thinkstats2 function, 'MakeNormalModel'.
> ```python
> MakeNormalModel(heights)
> thinkplot.Config(title=`Height (Males)', xlabel='Height (cm)',ylabel='CDF',loc='upper right')
> ```
> ```python
> n, mean, std =  154407 178.102789 7.017054
> ```
> ![male_heights_cdf](https://github.com/williamcottrell72/dsp/blob/master/statistics/images/HeightsNormalCDF.png)
>
> Now, let's make a CDF from the data:
> ```python
> cdf=thinkstats2.Cdf(heights)
> ```
> To get the percentage of men who fall between 6'1'' and 5'10'' we first need to convert these to centimeters and then we can use the built-in `PercentileRank` function. It is convenient to introduce the following imperial-to-metric conversion function:
> ```python
> def imp_met(ft,inch):
>    return (ft*12+inch)*2.54
> ```
> Now, the percentage within the stated range is:
> ```python
> heights_cdf.PercentileRank(imp_met(6,1))-heights_cdf.PercentileRank(imp_met(5,10))
>47.729053734610474
>```
> It appears that a whopping 47.73% falls within this range.  Looks like the chances of joining Blue Man Group are not so bad after all.
>
>Now, suppose we just use the Gaussian approximation rather than using the actual dataset.  We use the normal cdf provided by scipy as well as the mean and standard deviation found above.  The result is:
> ```python
> mu = 178.1
>sigma = 7.0
>scipy.stats.norm.cdf(imp_met(6,1),loc=mu, scale=sigma)-scipy.stats.norm.cdf(imp_met(5,10),loc=mu,scale=sigma)
> 0.36682450059854094
> ```
>So, only 36% of the male population falls in the desired range. Notice that I used the standard deviation derived above for men.  The book quotes a standard deviation of 7.7cm, which seems a bit large and I'm not sure where this number comes from.  If we use the books standard deviation, we find that only 34% fall within the desired range.
