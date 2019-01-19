+++
# Custom widget.
# An example of using the custom widget to create your own homepage section.
# To create more sections, duplicate this file and edit the values below as desired.
widget = "custom"
active = true
date = 2016-04-20T00:00:00

# Note: a full width section format can be enabled by commenting out the `title` and `subtitle` with a `#`.
title = "News and Articles"
subtitle = "External links and Resources"

# Order that this section will appear in.
weight = 60

# Content.
# Display content from the following folder.
# For example, `folder = "project"` displays content from `content/project/`.
# folder = "newsarticles"

# View.
# Customize how projects are displayed.
# Legend: 0 = list, 1 = cards.
# view = 1

# Filter toolbar.

# Default filter index (e.g. 0 corresponds to the first `[[filter]]` instance below).
# filter_default = 0
+++

Watch this space for interesting articles I came across or referred recently. This is going to be massive list of logs soon. I am looking for ways to handle this (suggestions welcome).

- Learn how to find gradient of determinant from this [note](https://folk.ntnu.no/hanche/notes/diffdet/diffdet.pdf) by Harald Hanche-Olsen
- Trouble finding the probability density of linear transformation of Random variables? What if it is a matrix of transformation for a vector of random variables? Check out [this](https://sccn.ucsd.edu/wiki/Random_Variables_and_Probability_Density_Functions)
- Robust machine learning is becoming of more interest as we see more of the ML algorithms becoming part of our lives - say resume screening systems in job applications, loan approving ML systems using credit data, news feed recommendations etc. For humans to explain the behaviour of such critical systems, robustness is an important criteria of ML algorithms. One of the pioneering researchers in this field [Dr.Thomas G. Dietterich](http://web.engr.oregonstate.edu/~tgd/) recently tweeted his short course on [Trustable Machine learning] (http://web.engr.oregonstate.edu/~tgd/classes/qinghua-2018/index.html). It is an interesting read and starting point of lot to come!
- Recently, I came across hamiltonian and langevin dynamics under the context of sampling probability distributions and for training deep neural networks. It is mindblowing to relate different things under one perspective (energy states of atoms -> dynamic systems -> training deep neural networks). I should really think and write about this. While it is under making, I spill some trailers [here] (https://en.wikipedia.org/wiki/Free_energy_principle) and [here] (https://www.nature.com/articles/s41593-018-0200-7)
- I figured out from lot of job descriptions that having experience of writing production level code is something good to have. But little I understood what it exactly means. I found this blog post simple and to point. As mentioned, reproduceability and documentation are key (over robust testing) for most part. For full details, check out [here](https://thuijskens.github.io/2018/11/13/useful-code-is-production-code/) 
- Following on the search for writing clean and well architected code, I am embarking on a journey of scientific computing using C/C++. In academia, it has been a norm to use MATLAB (Python - open source version) for scientific computing. I do not find a reason why someone should not use MATLAB. Again, people might have different stands on this, but paying someone for awesome software is not bad thing to do. There is no free lunch in this world. Open source is fine but someone has to support it and there are already many issues with open source (take example of Tensorflow framework which had serious flaws before it was fixed long after). Without deviating further, my point here is to add resources for scientific computing using C/C++. I am pooling in resources here [Principles of Scientific Computing](https://cs.nyu.edu/courses/spring09/G22.2112-001/book/book.pdf) [A comparison of doing in C vs C++](http://vergil.chemistry.gatech.edu/resources/programming/programming.pdf) [For examples on ODE solvers](http://index-of.es/C++/Guide%20to%20Scientific%20Computing%20in%20Cpp.pdf) [A Quick Refresher](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.464.2638&rep=rep1&type=pdf). I am sure there are accelerated libraries like BLAS but yet not commonly seen in day-to-day use. (WHY?)
