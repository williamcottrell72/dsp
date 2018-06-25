[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

> First we import the usual stuff:
> ```python
> import numpy as np
> import thinkstats2
> import thinkplot
> ```
> Now we want to generate a simple uniform random sample and display its pmf and cdf.  This is relatively straightforward.  First, let's make a list of 1000 random numbers:
> ```python
> rn=[np.random.random(1)[0] for x in range(1000)]
> ```
> Now, plot the pmf:
> ```python
> rn_pmf=thinkstats2.Pmf(rn,label='Random Numbers')
>thinkplot.Cdf(rn_pmf)
>thinkplot.Config(xlabel='number',ylabel='Count')
>```
>The result is
>(image here)
> Next, we plot the CDF:
> ```python
> rn_cdf=thinkstats2.Cdf(rn,label='Random Numbers')
>thinkplot.Cdf(rn_cdf)
>thinkplot.Config(xlabel='number',ylabel='CDF')
> ```
> Almost a straight line as expected.
