[Think Stats Chapter 8 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77) (scoring)

>In this exercise we want to investigate the sampling error in an exponenetial distribution. We will need math, thinkstats2 and one module for plotting:
> ```python
> import thinkstats2
> import math
> import matplotlib.pyplot as plt
> ```
> The pdf and cdf we are interested in is:
>```python
> P(x)= lam e^(-lam x)
> CDF=1-e^(-lam x)
> ```
> To generate a random sample with this pdf we can generate a random sample in the interval [0,1) and then transform such that the new distribution is exponential.  If y is uniformally distributed between 0 and 1, then `x` is exponentially distributed, where
> ```python
> x=-math.log(1-y)/lam
> ```
> Now, we just need to generate a sample.  The following function generates a random sample of size n and then computes L=1/np.mean(xs).  The process is repeated m times.
> ```python
> def LSampDist(lam=2,n=10,m=1000):
>    Ls=[]
>    ys=[]
>    for _ in range(m):
>        for _ in range(n):
>            y=np.random.random(1)[0]
>            ys.append(y)
>        xs=map(lambda y: -math.log(1-y)/lam,ys)
>        Ls.append(1/np.mean(list(xs)))
>    return Ls
>```
> Now we have a collection of measured values for L, which are really estimates of lam.  The following function computes the standard error given a collection of measurements, denoted by Ls.
> ```python
> def SE(Ls,lam=2):
>    se=0
>    for i in range(len(Ls)):
>        se+=(Ls[i]-lam)**2
>    return (se/len(Ls))**1/2
> ```
> For n=10, m=1000 and lam=2 we can compute then compute the standard error as follows:
> ```python
> ls=LSampDist()
> se=SE(ls)
> se
>  0.0005749315022924327
> ```
> (image here)
>To get the 90% confidence interval it is convenient to convert this to a CDF.
> ```python
> cdf=thinkstats2.Cdf(ls)
> cdf.Value(.05), cdf.Value(.95)
>(1.989846326031517, 2.078673934517343)
> ```
> We can repeat this for different values of n. Let's scan between n=2 and n=40 in steps of 2.
> ```python
> ns=np.arrange(2,42,2)
> ses=[]
> for n in ns:
>    ls=LSampDist(2,n)
>    ses.append(SE(ls))
> plt.plot(ns,ses)
>plt.xlabel('Sample Size')
>plt.ylabel('Standard Error')
>plt.show()
> ```
> (image here)
