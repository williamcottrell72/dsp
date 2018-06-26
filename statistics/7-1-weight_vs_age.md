[Think Stats Chapter 7 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2008.html#toc70) (weight vs. age)

>We want to find the correlation between mother's
> age and babies birthweight. The data is in nsfg.  Let's import this as well as a few other libraries:
> ```python
> import nsfg
> import thinkstats2
> import thinkplot
> import pandas as pd
> import numpy as np
> ```
> Now, read in the data and select out the baby weright (totalwgt_lb) and mother's age (agepreg) from the set of live births:
> ```python
> preg=nsfg.ReadFemPreg()
> live=preg[preg.outcome==1]
>weight=live['totalwgt_lb']
>age=live['agepreg']
> ```
>Next, make a plot, tuning the size and opacity of dots to give a reasonable representation of the data.
> ```python
> thinkplot.Scatter(age, weight, alpha=1, s=3)
>thinkplot.Config(xlabel='Age (yrs)',
>                 ylabel='Weight (lb)',
>                 axis=[15, 45, 0, 15],
>                 legend=False)
>```
> ![scatter](https://github.com/williamcottrell72/dsp/blob/master/statistics/images/AgeWeightScatter.png)
>
> Now let's plot percentiles.  First, bin the data:
> ```python
> bins_new = np.arange(15, 45, 5)
>indices_new = np.digitize(new.agepreg, bins_new)
>groups_new = new.groupby(indices_new)
> ```
> We have just gropued the data into bins consisting of 5 year periods.  Next, we get the mean age and CDF for each group.
> ```python
> mean_age = [groups_new.agepreg.mean() for i, group in groups_new]
>cdfs = [thinkstats2.Cdf(group.totalwgt_lb) for i, group in groups_new]
> ```
>Finally, we plot the 75th, 50th, and 25th percentile:
> ```python
> for percent in [75, 50, 25]:
>    weight_percentiles = [cdf.Percentile(percent) for cdf in cdfs]
>    label = '%dth' % percent
>    thinkplot.Plot(mean_age, weight_percentiles, label=label)
>
>thinkplot.Config(xlabel='Mother\'s Age',
>                 ylabel='Weight (lb)',
>                 axis=[15,45, 0, 15],
>                 legend=False)
>```
>  ![percentiles](https://github.com/williamcottrell72/dsp/blob/master/statistics/images/AgeWeightPercentiles.png)
>
> Finally, the two metrics of correlation are:
> ```python
> SpearmanCorr(age, weight)
> 0.09461004109658226
>Corr(age, weight)
> 0.06883397035410908
> ```
