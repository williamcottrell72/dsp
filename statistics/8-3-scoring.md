[Think Stats Chapter 8 Exercise 3](http://greenteapress.com/thinkstats2/html/thinkstats2009.html#toc77)


>Modules we might need:
>```python
> import thinkplot
>import math
>import matplotlib.pyplot as plt
> import numpy as np
>import thinkstats2
> ```
> We are asked to simulate an exponential distribution, parameterized by `lam`, for a period of one game time.  (I'll choose units where the game time is 1.)  We will be estimating `lam` by looking at the scores within one game time, which, a-priori, might introduce a bias since the true `lam` should be thought of as a rate determined by taking the infinite time limit.  To test whether or not there is any bias, we can simulate many samples.  First, we need a stochastic function that returns the number of goals in one game for a given `lam`.
>```python
>def goals(lam):
>    time=0
>    goals=-1
>    while time <1:
>        y=np.random.random(1)[0]
>        x=-math.log(1-y)/lam
>        time=time+x
>        goals=goals+1
>    return goals
> ```
> This basically represents one game.  We now want to simulate N=n*m games.  Here, `n` represents the number of games we group together in order to take an average and `m` represents the number of times we take an average over `n` games.  For each group of `n` games we get an estimate for `lam` by looking at the number of goals scored.  Call this estimate `l` and the collection of `m` estimates `ls`.  The function we need is:
>```python
> def many_games(lam,n,m):
>    ls=[]
>    l=[]
>    for _ in range(m):
>        for _ in range(n):
>            l.append(goals(lam))
>        ls.append(np.mean(l))
>    return ls
> ```
> Now we can pot a histogram of the resultant estimates.
>```python
> bins=np.linspace(8.5,10.3,50)
>ls=many_games(10,10,1000)
>plt.hist(les,bins=bins)
>plt.xlabel('lam estimate')
>plt.ylabel('Count')
>plt.show()
> ```
>
> (lam_hist_8p3)
> We can also plot the estimate for lam, the confidence interval, and how the RMS error changes with n, keeping lam and m fixed.  The following code does this:
> ```python
>def errors(lam,m,nsteps=40,step=1):
>    ns=np.arange(1,nsteps*step+step,step)
>    rms=[]
>    ninety_u=[]
>    ninety_l=[]
>    ls_mean=[]
>    for n in ns:
>        ls=many_games(lam,n,m)
>        ls_mean.append(np.mean(ls))
>        rms.append(np.std(ls))
>        cdf=thinkstats2.Cdf(ls)
>        ninety_u.append(cdf.Value(.95))
>        ninety_l.append(cdf.Value(.05))
>    plt.plot(ns,ls_mean,'bs',ns,ninety_l,'r--',ns,ninety_u,'r--')
>    plt.xlabel('sample size')
>    plt.ylabel('estimate of lam')
>    plt.show()
>    plt.plot(ns,rms)
>    plt.xlabel('sample size')
>    plt.ylabel('standard error')
>    plt.show()
>    return rms
> ```
> (CI_hist and se_versus_n_8p3)
> Empirically this seems to be an unbiased estimator.  This makes sense because this distribution is *memoryless*, i.e, this distribution doesn't care when we start the clock.  This is important since otherwise one might worry about edge effects at t=0.  For instance, we are assuming that there is no goal at t=0, but then we are using t=0 as the first time for out *between-goal periods*. Thus, one might wonder whether or not we should include a goal at t=0 or not, or maybe .5 goals?  In any case, this doesn't matter since the distribution is the same regardless of when we start the clock.  Thus, the game interval that we selected is typical and representative of randomly picking a one game period somewhere in an infinitely long period of game play.

>Finally, we are asked what happens if **lam** is increased.  Naturally, this should reduce the ratio of the relative error to the mean since we now have more goals scored.  The ratio of relative error to mean reduces with the number of samples.  
