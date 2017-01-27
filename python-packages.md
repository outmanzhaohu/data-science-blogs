# [Draft] Ranking Popular Python Packages for Data Science

Below is a ranking of Python packages applicable in Data Science, based on
Github and Stack Overflow activity, as well as PyPI Downloads.

The table shows standardized scores, where a value of 1 means one standard
deviation above average (average = score of 0). See below for methods.

<img src="img/python-rank.png" width=500px></img>

*For example, `numpy` is 2 standard deviations above average in Stack Overflow
activity, while `tensorflow` is close to average.*

# Results

Due to massive total downloads and strong Stack Overflow activity, the clear
leader is `numpy` (Scientific Computing). However, the scalable machine
learning package `tensorflow` (stared at Google) trounces the other packages in
Github activity (based on both stars and forks), with the more general machine
learning module `scikit-learn` a distant second, but fifth overall.

Both `numpy` and `pandas` (Data Manipulation) are only average on Github, but
strong in the other two categories.

The interactive interpreter `ipython` is fourth overall, while the `jupyter`
project (of the popular notebook) is 19th overall (not shown).

The [full ranking is here](output/python-ranks-with-na.csv), while
the [raw data is here](output/python-data-wide.csv).


# Discussion

There appears to be an inverse correlation in Github activity compared to Stack
Overflow and Downloads for the top packages. For example, there are a lot of
Stack Overflow questions for `numpy` and `pandas` compared to `tensorflow` and
`scikit-learn`. Since the former are two "utility" packages, perhaps more people are
actually using them (and need help).

The other deep-learning package, `Theano`, is a big distance behind `tensorflow`
in this ranking. 

As expected, `Matplotlib` is the most popular graphics package, but `plotly`
and `bokeh` both feature. `ggpy` (Python port for `ggplot`) was 18th overall,
but the data is less reliable, as noted in the next section. 


# Limitations

As
with [any analysis](https://twitter.com/benhamner/status/732392995610198016),
decisions were made along the way. All source code and data is on [our Github Page](https://github.com/thedataincubator/data-science-blogs).

First, our list of packages was [pre-selected](#Methods). 

The full list of machine learning packages [came from a few sources](#Methods),
and a few packages were unranked, due to unavailable downloads or Github
data. These are: `basemap` (mapping with `matplotlib`), `d3py` (D3-like
plotting), `jupyter-notebook`, `mlpy` (machine learning based on `scipy` and
`numpy`), `pylearn2` (machine learning, based on `theano`), `pytables` (big
tables), and `shogun` (machine learning). They were all below average compared
to the ranked packages, in all categories.

The data presented a few difficulties;

* The python port for `ggplot` was recently renamed to `ggpy`, and we used the
  latter for all metrics, except downloads (which used `ggplot`).
* `ipython notebook` is now `jupyter notebook`. Stack overflow auto-corrected
  `ipython-notebook` to `jupyter-notebook` so we combined these results with
  `jupyter-notebook` results from the other two sources. But `jupyter-notebook`
  didn't have a downloads count, so it doesn't feature in the final
  ranking. Instead, we also performed analyses for `ipython` and `jupyter`
  individually, which do appear in the ranking.
* The [Pattern package](https://github.com/clips/pattern) has inflated Stack
  Overflow (SO) question metrics since it's a common word. It has no tags data,
  as SO auto-corrects the query "[pattern]" to something unrelated.
* SO data for `plotly` may also be inflated, as it's both an R and Python
  package.



# Methods

All source code and data is on [our Github Page](https://github.com/thedataincubator/data-science-blogs).

We first generated a list of Data Science packages
from
[these](https://github.com/rasbt/pattern_classification/blob/master/resources/python_data_libraries.md) [three](https://www.upwork.com/hiring/data/15-python-libraries-data-science/) [sources](http://www.datasciencecentral.com/profiles/blogs/9-python-analytics-libraries-1),
and then collected metrics for all of them, to come up with the ranking.

Github data is based on both stars and forks, while Stack Overflow data is based
on tags and questions containing the package name. Downloads are from PyPI,
using
a [fork of the `vanity` project](https://github.com/pavopax/vanity). Other
projects to get download counts
include [pypi-download-stats](https://github.com/jantman/pypi-download-stats)
(couldn't get it to work)
and [pypi-ranking.info](http://pypi-ranking.info/alltime) (gives smaller
numbers than `vanity`).

`sqlite3` was removed from analysis, as it is a base Python module.

`ggplot` downloads were combined with `ggpy` results from Github and Stack
Overflow.

Any unavailable Stack Overflow counts were converted to zero count.

Counts were standardized to mean 0 and deviation 1, and then averaged to get
Github and Stack Overflow scores, and, combined with the Downloads, the Overall
score.

Some manual checks were done to confirm Github repository location.

All data was downloaded on January 26, 2017.