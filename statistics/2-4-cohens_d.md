[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

>> To begin, import the relevant libraries:
> ```python
> from __future__ import print_function, division
>matplotlib inline
> imoprt thinkstats2
>import numpy as np
>import nsfg
>import first
> ```
>> Next, we read in the data using the built-in function `ReadFemPreg' from ThinkStats:
> ```python
> preg = nsfg.ReadFemPreg()
>live = preg[preg.outcome == 1]
> ```
> Now, we have \'live', a pandas dataframe containing information about the live births.  From this we can separate this into a set of first borns and the others:
> ```python
> firsts = live[live.birthord == 1]
>others = live[live.birthord != 1]
> ```
> It is not easy to compare weights between these two groups:
> ```python
> print(firsts.totalwgt_lb.mean(), firsts.totalwgt_lb.std())
>print(others.totalwgt_lb.mean(), others.totalwgt_lb.std())
> ```
> ```
>7.201094430437772 1.4205728777207374
>7.325855614973262 1.3941954762143138
> ```
> It is hard to tell whether or not the difference seen above is significant or not just by looking at these numbers.  A more formal metric for how different these are is the Cohen Effect Size.  This is computed also with a function in ThinkStats.
> ```python
> CohenEffectSize(firsts.totalwgt_lb,others.totalwgt_lb)
> ```
> ```python
> -0.088672927072602
> ```
> This is not very large, indicating that the difference in popoulations is not so big.  For pregnancy lengths the Cohen Size Effect was .029 - also not so big.
