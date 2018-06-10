# Install software on your computer


### Install [git](http://git-scm.com/).

You have it installed if you can run `git --version` at the command
line and get output like `git version 2.13.5`.

Macbooks have git pre-installed.


### Install [Anaconda](http://continuum.io/downloads).

Please install the **3.6 version** if possible.

There are two things you can verify to check your install.

First, from the command line, all of the following should start up
some kind of Python interpreter:

```bash
python3
ipython3
jupyter notebook
spyder
```

Second, inside any of those Python interpreters, you should be able to
do all of these without error:

```python
import numpy
import scipy
import matplotlib
import pandas
import statsmodels
import sklearn
```

### Install [Homebrew](http://brew.sh/) on Mac _(optional)_

For users who are already familiar with package managers, you may prefer to install Homebrew.

However, this is completely optional as Anaconda is an all-in-one package manager.

---

### Q1. Python Version 2 or 3

**Course material for the bootcamp is compatible with Python versions 2.7 and 3.0. All HackerRank Python pre-work is configured for Python 3 only.  Therefore, Python 3 is the recommended version.**

Did you install Python 2 or 3? Why?

>> I installed Python 3.6.5.  For one, it came with Aanaconda.  For two, it seems that Python 2 will not be supported forever and everything will eventually need to transfer over to Python 3 anyway.  Since I'm learning the syntax now, there is no point in learning some details that are soon to be defunct.

### Q2. Which Python Version Installed

In the Terminal:

`python3 --version`

>> As mentioned, I went with 3.6.5.  For iPython I've got 6.4.0, which is just the latest version.  (Or, at least the latest version that Anaconda thinks is safe.)
