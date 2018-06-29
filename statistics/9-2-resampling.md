[Think Stats Chapter 9 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2010.html#toc90) (resampling)

>We want to see what happens if we `resample' the original distribution rather than treating both populations as coming from the same larger homogenous sample.  The assignment was described in a somewhat ambiguous manner, but here is how I interpreted it.  We have some large populations which can be divided into two groups, pop1 and pop2.  These two groups are themselves very large and so we will assume that we cannot access all of pop1 or pop2 conveniently.  We thus draw a sample of predetermined size from pop1 and pop2 and compare the statistics between these two popultions.  If some statistic differs between these two we want to know if it is significant.  I am interpreting 'resamplling' just to mean that we try and pick different samples from pop1 and pop2 instead, though we will never consider permuting elements of pop1 and pop2 as with the previous method.  To implement this method, we first import some useful libraries:
```python
import numpy as np
import thinkstats2
import nsfg
import randoml
import first
```
>In addition, we will also need some classes from this chapter, in particular:
```python
class HypothesisTest(object):

    def __init__(self, data):
        self.data = data
        self.MakeModel()
        self.actual = self.TestStatistic(data)

    def PValue(self, iters=1000):
        self.test_stats = [self.TestStatistic(self.RunModel())
                           for _ in range(iters)]

        count = sum(1 for x in self.test_stats if x >= self.actual)
        return count / iters

    def TestStatistic(self, data):
        raise UnimplementedMethodException()

    def MakeModel(self):
        pass

    def RunModel(self):
        raise UnimplementedMethodException()

class DiffMeansPermute(thinkstats2.HypothesisTest):

    def TestStatistic(self, data):
        group1, group2 = data
        test_stat = abs(group1.mean() - group2.mean())
        return test_stat

    def MakeModel(self):
        group1, group2 = self.data
        self.n, self.m = len(group1), len(group2)
        self.pool = np.hstack((group1, group2))

    def RunModel(self):
        np.random.shuffle(self.pool)
        data = self.pool[:self.n], self.pool[self.n:]
        return data
```

>Now we need to write a class that inherits from DiffMeansPermute but has a slightly different RunModel.  The new model selects a subset from each group.  We want to introudce a parameter, 'n' to specifiy how many elements we want in our sample since, as explained above, we are assuming we cannot look at the full dataset.  (Here, we are pretending that the full dataset is analogous to the full popultion.)  The following code does the job.

```python
class DiffMeansResample(DiffMeansPermute):

    def Resample(xs):
        return np.random.choice(xs,len(xs),replace=True)

    def PValue(self,n,iters=1000):
        self.test_stats = [self.TestStatistic(self.RunModel(n))
                           for _ in range(iters)]

        count = sum(1 for x in self.test_stats if x >= self.actual)
        return count / iters


    def RunModel(self,n):
        d1,d2 = self.data
        data = np.random.choice(d1,n,replace=True), np.random.choice(d2,n,replace=True)
        return data
```
>Let's run this for firstborns and other children and compare weight and length of pregnancy.  First, let's look at pregnancy length::
```python
data=firsts.prglngth.values, others.prglngth.values
plres=DiffMeansResample(data)
p_value = plres.PValue(100)
print('p-value =', p_value)
print('actual =', plres.actual)
print('ts max =', plres.MaxTestStat())
```
>This gives
```python
p-value = 0.832
actual = 0.07803726677754952
ts max = 1.480000000000004
```
>Now, for weight:
```python
data2=firsts.totalwgt_lb.dropna().values, others.totalwgt_lb.dropna().values
plres2=DiffMeansResample(data2)
p_value2 = plres2.PValue(100)
print('p-value =', p_value)
print('actual =', plres2.actual)
print('ts max =', plres2.MaxTestStat())
```
```python
p-value = 0.638
actual = 0.12476118453549034
ts max = 0.7193750000000003
```
>So apparently, neither of these affects are significant by this metric.

>Update:  After looking at the author's solutions I realize that I've interpreted this somewhat differently than was intended.  Rather than picking a small sample from the two separate populations, we are just supposed to take the size of pop1 and pop2, and draw two samples of equivalent sizes.  We will still use the full dataset with this method.  A priori I would expect this to give the same result as the original method (permutation) described in the book since we making a random selection from the larger population, albeit in a slightly different way.  The code for this method is:
```python
class DiffMeansResample(DiffMeansPermute):

    def RunModel(self):

        group1 = np.random.choice(self.pool, self.n, replace=True)
        group2 = np.random.choice(self.pool, self.m, replace=True)
        return group1, group2
```

> Noticd that this mthod pools the data so it is very similar to permutation.  Now leet's compare the two datasets:
```python
data = firsts.prglngth.values, others.prglngth.values
ht = DiffMeansResample(data)
p_value = ht.PValue(iters=10000)
print('\ndiff means resample preglength')
print('p-value =', p_value)
print('actual =', ht.actual)
print('ts max =', ht.MaxTestStat())

data = (firsts.totalwgt_lb.dropna().values,
        others.totalwgt_lb.dropna().values)
ht = DiffMeansPermute(data)
p_value = ht.PValue(iters=10000)
print('\ndiff means resample birthweight')
print('p-value =', p_value)
print('actual =', ht.actual)
print('ts max =', ht.MaxTestStat())
```
>Running this, I find:
```python
diff means resample preglength
p-value = 0.1638
actual = 0.0780372667775
ts max = 0.214769744092

diff means resample birthweight
p-value = 0.0
actual = 0.124761184535
ts max = 0.106289293414
```
>Similar to previous results as expected.
