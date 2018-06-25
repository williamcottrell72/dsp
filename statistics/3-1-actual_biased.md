[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)



>> We start by importing the usual suspects:
> ```python
> import numpy as np
> import pandas as pd
> import thinkstats2
> import thinkplot
> import nsfg
>```
>> Now we read in the data:
> ```python
> resp = nsfg.ReadFemResp()
> ```
>> From this we can form two distributions: One distribution obtained by polling households regarding the number of children and another distribution based on polling children.  When polling children, we simply need to weight the household distribpution \'P(x)' by the number of children, so that the new distribution is proportional to \'x P(x)'.  Finally, we must normalize.  First, the unbiased distribution is:
> ```python
> numk_pmf=thinkstats2.Pmf(resp.numkdhh,label='Number of Children')
> ```
> Although there are built in functions to get the biased distribution, it is just as easy to repeat the steps here:
> ```python
> def BiasedPmf(pmf,label):
>    new=thinkstats2.Pmf()
>
>    for x,p in pmf.Items():
>        new[x]=x*pmf[x]
>
>    tot=new.Total()
>    for x,p in new.Items():
>        new[x]=new[x]/tot
>
>    return new
> ```
> The biased distribution we want is now:
> ```python
> numk_bias=BiasPmf(numk_pmf,`Observed Number Distribution`)
> ```
> Finally, let's plot and compare:
> ```python
> thinkplot.PrePlot(2)
> thinkplot.Pmfs([numk_pmf,numk_bias])
> thinkplot.Config(xlabel='Number of Children',ylabel='PMF')
> ```

![test_mew](https://github.com/williamcottrell72/dsp/blob/master/statistics/images/Bias_vs_Unbiased.png)


[Think Stats Chapter 3 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased for footrace)
>> As a runner I found this question interesting.  We are asked to find the observed speed PMF of each runner given the actual distribution of sppeds.  First, read in and format the actual distribution:
> ```python
> import relay

>results = relay.ReadResults()
>speeds = relay.GetSpeeds(results)
>speeds = relay.BinData(speeds, 3, 12, 100)
> pmf_speeds = thinkstats2.Pmf(speeds, 'actual speeds')
> ```
> Next, we define a function which takes the real distribution to an observed distrubtion.  There are two effects to consider.  For one, each person observes other runners moving at a velocity which is the difference between their velocity and the opponents velocity.  Secondly, the number of opponents one observes is proportional to the relative velocity.  So, we first need to shift the distribution and then we need to bias it by a multiplicative factor.  This is accomplished by the following function.
> ```python
> def ObservedPmf(pmf,v):
>    new_pmf=thinkstats2.Pmf()
>    for x, f in pmf.Items():
>        new_pmf[x-v]=f
>    for x,f in new_pmf.Items():
>        new_pmf.Mult(x,x)
>        new_pmf[x]=abs(new_pmf[x])
>    new_pmf.Normalize()
>    return new_pmf
> ```
> Applying this to the relay speeds (and assuming that we are a \`typical' runner moving at 7mph) we get:
> ```python
> new=ObservedPmf(pmf_speeds,7)
> ```
> Finally, let's plot the new distribution:
> ```python
> new_hist=thinkstats2.Pmf(new,label='Biased Speed Distribution')
>thinkplot.Pmf(new_hist)
>thinkplot.Config(xlabel='Relative Velocity',ylabel='PMF')
> ```
> ![image2](https://github.com/williamcottrell72/dsp/blob/master/statistics/images/Biased_Speed_Distribution.png)
>
>As expected, this appears to be bimodal.
